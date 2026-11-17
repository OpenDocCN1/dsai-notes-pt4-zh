
```
XTransformer(
  (encoder): TransformerWrapper(
    (token_emb): TokenEmbedding(
      (emb): Embedding(18, 512)
    )
    (pos_emb): AbsolutePositionalEmbedding(
      (emb): Embedding(32, 512)
    )
    (post_emb_norm): Identity()
    (emb_dropout): Dropout(p=0, inplace=False)
    (project_emb): Identity()
    (attn_layers): Encoder(
      (layers): ModuleList(
        (0): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (1): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
        (2): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (3): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
        (4): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): Attention(
            (to_q): Linear(in_features=512, out_features=512, bias=False)
            (to_k): Linear(in_features=512, out_features=512, bias=False)
            (to_v): Linear(in_features=512, out_features=512, bias=False)
            (attend): Attend(
              (attn_dropout): Dropout(p=0.0, inplace=False)
            )
            (to_out): Linear(in_features=512, out_features=512, bias=False)
          )
          (2): Residual()
        )
        (5): ModuleList(
          (0): ModuleList(
            (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
            (1-2): 2 x None
          )
          (1): FeedForward(
            (ff): Sequential(
              (0): Sequential(
                (0): Linear(in_features=512, out_features=2048, bias=True)
                (1): GELU(approximate=&#x27;none&#x27;)
              )
              (1): Dropout(p=0.0, inplace=False)
              (2): Linear(in_features=2048, out_features=512, bias=True)
            )
          )
          (2): Residual()
        )
      )
      (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
    )
    (to_logits): Linear(in_features=512, out_features=18, bias=False)
  )
  (decoder): AutoregressiveWrapper(
    (net): TransformerWrapper(
      (token_emb): TokenEmbedding(
        (emb): Embedding(18, 512)
      )
      (pos_emb): AbsolutePositionalEmbedding(
        (emb): Embedding(65, 512)
      )
      (post_emb_norm): Identity()
      (emb_dropout): Dropout(p=0, inplace=False)
      (project_emb): Identity()
      (attn_layers): Decoder(
        (layers): ModuleList(
          (0-1): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (2): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
          (3-4): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (5): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
          (6-7): 2 x ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): Attention(
              (to_q): Linear(in_features=512, out_features=512, bias=False)
              (to_k): Linear(in_features=512, out_features=512, bias=False)
              (to_v): Linear(in_features=512, out_features=512, bias=False)
              (attend): Attend(
                (attn_dropout): Dropout(p=0.0, inplace=False)
              )
              (to_out): Linear(in_features=512, out_features=512, bias=False)
            )
            (2): Residual()
          )
          (8): ModuleList(
            (0): ModuleList(
              (0): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
              (1-2): 2 x None
            )
            (1): FeedForward(
              (ff): Sequential(
                (0): Sequential(
                  (0): Linear(in_features=512, out_features=2048, bias=True)
                  (1): GELU(approximate=&#x27;none&#x27;)
                )
                (1): Dropout(p=0.0, inplace=False)
                (2): Linear(in_features=2048, out_features=512, bias=True)
              )
            )
            (2): Residual()
          )
        )
        (final_norm): LayerNorm((512,), eps=1e-05, elementwise_affine=True)
      )
      (to_logits): Linear(in_features=512, out_features=18, bias=False)
    )
  )
)

## 数据源序列

```
Source Sequence:  tensor([[10,  8,  4, 15, 16,  5, 14, 14, 16, 13,  6,  7,  5,  2,  7,  8, 13, 16,
          2, 14,  8, 12, 12,  3, 10,  3,  8,  5,  8,  8, 10, 10]],
       device=&#x27;cuda:0&#x27;)
```

## 预测结果

```
Generated Sequence:  tensor([[10,  8,  4, 15, 16,  5, 14, 14, 16, 13,  6,  7,  5,  2,  7,  8, 13, 16,
          2, 14,  8, 12, 12,  3, 10,  3,  8,  5,  8,  8, 10, 10]],
       device=&#x27;cuda:0&#x27;)
```
