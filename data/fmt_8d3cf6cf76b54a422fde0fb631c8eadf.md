
```
void ToyToAffineLoweringPass::runOnOperation() {
  // The first thing to define is the conversion target. This will define the
  // final target for this lowering.
  mlir::ConversionTarget target(getContext());

  // We define the specific operations, or dialects, that are legal targets for
  // this lowering. In our case, we are lowering to a combination of the
  // `Affine`, `Arith`, `Func`, and `MemRef` dialects.
  target.addLegalDialect<affine::AffineDialect, arith::ArithDialect,
                         func::FuncDialect, memref::MemRefDialect>();

  // We also define the Toy dialect as Illegal so that the conversion will fail
  // if any of these operations are *not* converted. Given that we actually want
  // a partial lowering, we explicitly mark the Toy operations that don't want
  // to lower, `toy.print`, as *legal*. `toy.print` will still need its operands
  // to be updated though (as we convert from TensorType to MemRefType), so we
  // only treat it as `legal` if its operands are legal.
  target.addIllegalDialect<ToyDialect>();
  target.addDynamicallyLegalOp<toy::PrintOp>([](toy::PrintOp op) {
    return llvm::none_of(op->getOperandTypes(),
                         [](Type type) { return type.isa<TensorType>(); });
  });
  ...
}
```

Above, we first set the toy dialect to illegal, and then the print operation as legal. We could have done this the other way around. Individual operations always take precedence over the (more generic) dialect definitions, so the order doesn’t matter.

## Conversion Patterns

```
/// Lower the `toy.transpose` operation to an affine loop nest.
struct TransposeOpLowering : public mlir::ConversionPattern {
  TransposeOpLowering(mlir::MLIRContext *ctx)
      : mlir::ConversionPattern(TransposeOp::getOperationName(), 1, ctx) {}

  /// Match and rewrite the given `toy.transpose` operation, with the given
  /// operands that have been remapped from `tensor<...>` to `memref<...>`.
  mlir::LogicalResult
  matchAndRewrite(mlir::Operation *op, ArrayRef<mlir::Value> operands,
                  mlir::ConversionPatternRewriter &rewriter) const final {
    auto loc = op->getLoc();

    // Call to a helper function that will lower the current operation to a set
    // of affine loops. We provide a functor that operates on the remapped
    // operands, as well as the loop induction variables for the inner most
    // loop body.
    lowerOpToLoops(
        op, operands, rewriter,
        [loc](mlir::PatternRewriter &rewriter,
              ArrayRef<mlir::Value> memRefOperands,
              ArrayRef<mlir::Value> loopIvs) {
          // Generate an adaptor for the remapped operands of the TransposeOp.
          // This allows for using the nice named accessors that are generated
          // by the ODS. This adaptor is automatically provided by the ODS
          // framework.
          TransposeOpAdaptor transposeAdaptor(memRefOperands);
          mlir::Value input = transposeAdaptor.input();

          // Transpose the elements by generating a load from the reverse
          // indices.
          SmallVector<mlir::Value, 2> reverseIvs(llvm::reverse(loopIvs));
          return rewriter.create<mlir::AffineLoadOp>(loc, input, reverseIvs);
        });
    return success();
  }
};
```

## Design Considerations

In our lowering, we transform from a value-type, `TensorType`, to an allocated (buffer-like) type, `MemRefType`.

However, given that we do not lower the `toy.print` operation, we need to temporarily bridge these two worlds. There are many ways to go about this, each with their own tradeoffs:

+   Generate load operations from the buffer

One option is to generate load operations from the buffer type to materialize an instance of the value type. This allows for the definition of the `toy.print` operation to remain unchanged. The downside to this approach is that the optimizations on the affine dialect are limited, because the load will actually involve a full copy that is only visible after our optimizations have been performed.

+   Generate a new version of `toy.print` that operates on the lowered type

Another option would be to have another, lowered, variant of `toy.print` that operates on the lowered type. The benefit of this option is that there is no hidden, unnecessary copy to the optimizer. The downside is that another operation definition is needed that may duplicate many aspects of the first. Defining a base class in ODS may simplify this, but you still need to treat these operations separately.

+   Update `toy.print` to allow for operating on the lowered type

A third option is to update the current definition of `toy.print` to allow for operating the on the lowered type. The benefit of this approach is that it is simple, does not introduce an additional hidden copy, and does not require another operation definition. The downside to this option is that it requires mixing abstraction levels in the `Toy` dialect.

```
def PrintOp : Toy_Op<"print"> {
  ...

  // The print operation takes an input tensor to print.
  // We also allow a F64MemRef to enable interop during partial lowering.
  let arguments = (ins AnyTypeOf<[F64Tensor, F64MemRef]>:$input);
}
```

## Complete Toy Example

```
toy.func @main() {
  %0 = toy.constant dense<[[1.000000e+00, 2.000000e+00, 3.000000e+00], [4.000000e+00, 5.000000e+00, 6.000000e+00]]> : tensor<2x3xf64>;
  %2 = toy.transpose(%0 : tensor<2x3xf64>) to tensor<3x2xf64>;
  %3 = toy.mul %2, %2 : tensor<3x2xf64>;
  toy.print %3 : tensor<3x2xf64>;
  toy.return
}
```

```
func.func @main() {
  %cst = arith.constant 1.000000e+00 : f64
  %cst_0 = arith.constant 2.000000e+00 : f64
  %cst_1 = arith.constant 3.000000e+00 : f64
  %cst_2 = arith.constant 4.000000e+00 : f64
  %cst_3 = arith.constant 5.000000e+00 : f64
  %cst_4 = arith.constant 6.000000e+00 : f64

  // Allocating buffers for the inputs and outputs.
  %0 = memref.alloc() : memref<3x2xf64>;
  %1 = memref.alloc() : memref<3x2xf64>;
  %2 = memref.alloc() : memref<2x3xf64>;

  // Initialize the input buffer with the constant values.
  affine.store %cst, %2[0, 0] : memref<2x3xf64>;
  affine.store %cst_0, %2[0, 1] : memref<2x3xf64>;
  affine.store %cst_1, %2[0, 2] : memref<2x3xf64>;
  affine.store %cst_2, %2[1, 0] : memref<2x3xf64>;
  affine.store %cst_3, %2[1, 1] : memref<2x3xf64>;
  affine.store %cst_4, %2[1, 2] : memref<2x3xf64>;

  // Load the transpose value from the input buffer and store it into the
  // next input buffer.
  affine.for %arg0 = 0 to 3 {
    affine.for %arg1 = 0 to 2 {
      %3 = affine.load %2[%arg1, %arg0] : memref<2x3xf64>;
      affine.store %3, %1[%arg0, %arg1] : memref<3x2xf64>;
    }
  }

  // Multiply and store into the output buffer.
  affine.for %arg0 = 0 to 3 {
    affine.for %arg1 = 0 to 2 {
      %3 = affine.load %1[%arg0, %arg1] : memref<3x2xf64>;
      %4 = affine.load %1[%arg0, %arg1] : memref<3x2xf64>;
      %5 = arith.mulf %3, %4 : f64
      affine.store %5, %0[%arg0, %arg1] : memref<3x2xf64>;
    }
  }

  // Print the value held by the buffer.
  toy.print %0 : memref<3x2xf64>;
  memref.dealloc %2 : memref<2x3xf64>;
  memref.dealloc %1 : memref<3x2xf64>;
  memref.dealloc %0 : memref<3x2xf64>;
  return
}
```

Adding the `LoopFusion` and `AffineScalarReplacement` passes to the pipeline gives the following result:

右侧是 fusion 后的，`transpose` 和 `mul` 做了算子融合。并优化掉了不必要的 memory load/store。

![](img/f114f0b9a25434c5d209368e7a885348.png)

## Lowering to LLVM and CodeGeneration
