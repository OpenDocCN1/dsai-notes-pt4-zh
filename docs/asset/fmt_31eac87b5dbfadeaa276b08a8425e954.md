
```python
from sklearn.metrics import mean_squared_error
from sklearn.model_selection import GridSearchCV

# 训练线性回归模型
lr.fit(X_train_scaled, y_train)
lr_preds = lr.predict(X_val_scaled)
lr_mse = mean_squared_error(y_val, lr_preds)
print(f'线性回归验证集MSE: {lr_mse}')

# 训练岭回归模型
ridge.fit(X_train_scaled, y_train)
ridge_preds = ridge.predict(X_val_scaled)
ridge_mse = mean_squared_error(y_val, ridge_preds)
print(f'岭回归验证集MSE: {ridge_mse}')

# 训练随机森林模型
rf.fit(X_train_fe, y_train)
rf_preds = rf.predict(X_val_fe)
rf_mse = mean_squared_error(y_val, rf_preds)
print(f'随机森林验证集MSE: {rf_mse}')

# 超参数调优，使用网格搜索优化随机森林
param_grid = {
    'n_estimators': [100, 200], # 随机森林中树的个数，尝试 100 和 200 两种设置
    'max_depth': [None, 10, 20], # 每棵树的最大深度，尝试不限制、10 和 20 三种设置
    'min_samples_split': [2, 5], # 继续分裂节点所需的最小样本数
}

grid_search = GridSearchCV(
    estimator=RandomForestRegressor(random_state=42), # 要优化的模型
    param_grid=param_grid, # 参数网格
    cv=5, # 将训练数据自动分为 5 个大致相等的子集，用于交叉验证
    scoring='neg_mean_squared_error', # 评估指标（负均方误差）
    n_jobs=-1 # 并行计算，使用所有可用的CPU核来加速计算
)

grid_search.fit(X_train_fe, y_train)
print(f'最佳参数: {grid_search.best_params_}')
print(f'最佳CV得分: {-grid_search.best_score_}')

# 使用最佳参数重新训练随机森林
best_rf = grid_search.best_estimator_
best_rf.fit(X_train_fe, y_train)
best_rf_preds = best_rf.predict(X_val_fe)
best_rf_mse = mean_squared_error(y_val, best_rf_preds)
print(f'优化后的随机森林验证集MSE: {best_rf_mse}')
```

执行后可以看到结果：

```
线性回归验证集MSE: 14189816400.231485
岭回归验证集MSE: 14289020855.107927
随机森林验证集MSE: 807947915.1960852
最佳参数: {'max_depth': 10, 'min_samples_split': 2, 'n_estimators': 100}
最佳CV得分: 957882699.9007219
优化后的随机森林验证集MSE: 834781203.5691917
```

优化后的随机森林验证集 MSE 竟然略大于随机森林验证集 MSE，略尴尬。。。可以使用不同的超参数组合“调参”或者使用贝叶斯优化等更高级的调优方法。

### 3.8 模型评估

在选择最优模型后，使用验证集进行最终评估，并进行误差分析以了解模型的不足。

![](img/4c02a73a2c59ef2c023f05b0dca8aef5.png)

RMSE 是“均方根误差”，也就是 MSE 的平方根，其单位与目标变量的原始单位一致，可以更直观地反映了预测误差的平均水平。

```python
final_rmse = sqrt(best_rf_mse)
print(f'最终随机森林验证集RMSE: {final_rmse}')

# 误差分析
errors = y_val - best_rf_preds
plt.rcParams['font.family'] = 'SimSong' # 设置中文字体
plt.figure(figsize=(10,6))
sns.histplot(errors, bins=30, kde=True, color='red')
plt.title('预测误差分布')
plt.xlabel('误差 (实际 - 预测)')
plt.ylabel('频数')
plt.show()

# 绘制实际值与预测值对比图
plt.figure(figsize=(10,6))
sns.scatterplot(x=y_val, y=best_rf_preds)
plt.plot([y_val.min(), y_val.max()], [y_val.min(), y_val.max()], color='black', linestyle='--')
plt.title('实际房价 vs 预测房价')
plt.xlabel('实际房价 (美元)')
plt.ylabel('预测房价 (美元)')
plt.show()
```

图中 RMSE 在 0 附近的频数最高，表明模型的大部分预测值与实际值较为接近，这是一个积极的信号，说明模型在多数情况下能够给出相对合理的预测。

![](img/2f80cc449602289ad193b4ac475d6e22.png)

+ 图中大部分点分布在对角线（理想预测线）附近，表明模型在一定程度上能够反映实际房价的趋势，对中低房价区域的预测有一定合理性。

+ 然而在高房价区域（横轴和纵轴较大值处），点的分散程度明显增加，部分点偏离对角线较远，说明模型对高房价的预测准确性较差，可能在处理高价值样本或相关特征时存在不足。

### 3.9 优化改进

基于评估结果，可以从数据、特征和模型多个层面进行优化：

#### 数据层面

+ **获取更多数据：**采集更多相关数据，提升模型的泛化能力。

+ **处理异常值：**识别并处理数据中的异常值，以减少其对模型的负面影响。

#### 特征层面

+ **生成新特征：**如房屋总面积、房屋价值比率等。

+ **特征选择：**去除低相关性或冗余的特征，简化模型。

+ **特征转换：**对数变换、`Box-Cox`变换等，处理偏态分布的特征。

#### 模型层面

+ **尝试其它模型：**如梯度提升树（`XGBoost`、`LightGBM`）、支持向量机（`SVM`）等。

+ **进一步超参数调优：**扩展搜索范围或使用更高级的搜索方法（如随机搜索、贝叶斯优化）。

+ **集成方法：**结合多个模型的预测结果，提升整体性能。

#### 三个手段优化模型

+ **生成新特征：**通过组合现有特征生成新的特征，如总房间数，可能提升模型的表现。

+ **特征选择：**通过相关性分析选择与目标变量高度相关的特征，减少模型复杂度，提升性能。

+ **重新训练模型：**使用优化后的特征集重新训练随机森林模型，并评估其性能。

```python
# 生成新特征：总房间数
X_train_fe['TotalRooms'] = X_train_fe['TotalBsmtSF'] + X_train_fe['1stFlrSF'] + X_train_fe['2ndFlrSF']
X_val_fe['TotalRooms'] = X_val_fe['TotalBsmtSF'] + X_val_fe['1stFlrSF'] + X_val_fe['2ndFlrSF']

# 特征选择：选择与目标变量相关性较高的特征
corr_matrix = train.corr(numeric_only=True)

# 假设选择相关性绝对值大于 0.12（反复试出来的）的特征
high_corr_features = corr_matrix['SalePrice'][corr_matrix['SalePrice'].abs() > 0.12].index
high_corr_features = high_corr_features.drop(['SalePrice'])  # 去除目标变量本身

# 重建特征集
X_train_selected = X_train_fe[high_corr_features]
X_val_selected = X_val_fe[high_corr_features]

# 重新特征缩放
X_train_selected_scaled = scaler.fit_transform(X_train_selected)
X_val_selected_scaled = scaler.transform(X_val_selected)

# 使用新的特征训练随机森林
best_rf.fit(X_train_selected, y_train)
new_rf_preds = best_rf.predict(X_val_selected)
new_rf_mse = mean_squared_error(y_val, new_rf_preds)
print(f'优化后随机森林验证集MSE: {new_rf_mse}')
```

![](img/eaa32489742a04e49b3207982c91fb9d.png)

```
线性回归验证集MSE: 12564956652.760109
岭回归验证集MSE: 12710807374.91197
随机森林验证集MSE: 760862367.6638939
最佳参数: {'max_depth': 20, 'min_samples_split': 2, 'n_estimators': 200}
最佳CV得分: 1096686290.3257225
优化后的随机森林验证集MSE: 756838039.5099173
最终随机森林验证集RMSE: 27510.68954988074
优化后随机森林验证集MSE: 755321480.0326563
```

### 3.10 部署与监控

一旦模型表现满意，就可以将其部署为服务，使其能够接受新数据并返回预测结果。同时，建立监控机制，确保模型在实际应用中的性能。

使用`joblib`将训练好的模型和`Scaler`持久化保存，以便在 API 中加载使用。

```python
import joblib

# 保存最佳模型和 Scaler
joblib.dump(best_rf, 'best_rf_model.joblib')
joblib.dump(scaler, 'scaler.joblib')
```

可以使用`Flask`写一个简单的测试：

```python
from flask import Flask, request, jsonify
import joblib
import numpy as np
import pandas as pd

app = Flask(__name__)

# 加载模型和Scaler
model = joblib.load('best_rf_model.joblib')
scaler = joblib.load('scaler.joblib')

@app.route('/predict', methods=['POST'])
def predict():
    try:
        data = request.get_json()
        features = pd.DataFrame(data['features'])
        
        # 预处理特征
        # 需要与训练时的特征工程步骤一致
        # 例如，生成新特征、编码类别变量、填充缺失值等
        # 这里假设数据已经按模型要求处理完毕
        prediction = model.predict(features)
        return jsonify({'prediction': prediction.tolist()})
    except Exception as e:
        return jsonify({'error': str(e)})

if __name__ == '__main__':
    app.run(debug=True)
```

### 3.11 反馈与迭代

部署后的模型需要持续监控其表现，并根据实际反馈进行改进。

1. **监控模型性能：**定期评估模型在新数据上的表现，确保其准确性。

2. **数据更新：**随着时间推移，数据分布发生变化，需要定期更新训练数据以保持模型的有效性。

3. **迭代优化：**基于监控和反馈结果，进行特征工程、模型调整或选择新模型。

假设我们发现模型在特定区域 `LotArea` 较小时预测误差较大，我们可以为这类房屋生成一个新的特征，例如是否小地块：

```python
X_train_fe['Is_Small_Lot'] = X_train_fe['LotArea'] < 5000
X_val_fe['Is_Small_Lot'] = X_val_fe['LotArea'] < 5000

# 重新特征选择和缩放
high_corr_features = corr_matrix['SalePrice'][corr_matrix['SalePrice'].abs() > 0.5].index
high_corr_features = high_corr_features.drop(['SalePrice'])  # 去除目标变量本身

X_train_selected = X_train_fe[high_corr_features]
X_val_selected = X_val_fe[high_corr_features]

# 重新缩放
X_train_selected_scaled = scaler.fit_transform(X_train_selected)
X_val_selected_scaled = scaler.transform(X_val_selected)

# 重新训练模型
best_rf.fit(X_train_selected, y_train)
new_rf_preds = best_rf.predict(X_val_selected)
new_rf_mse = mean_squared_error(y_val, new_rf_preds)
print(f'优化后随机森林验证集MSE: {new_rf_mse}')
```

## 小结

通过以上步骤，我们完成了从需求分析到模型部署开发房价预测模型的过程。

**作为AI产品经理，学习强大的机器学习库，也可以轻松地跟进构建、评估和部署模型。**

希望本文能帮助AI产品同学迈出机器学习的第一步。
