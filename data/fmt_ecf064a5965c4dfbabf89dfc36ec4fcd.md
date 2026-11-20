
# 特征工程

```python
# 为了保证时间顺序的一致性，故进行排序
train_df = train_df.sort_values(['file', 'time'])
test_df = test_df.sort_values(['file', 'time'])

# 当前时间特征
# 围绕买卖价格和买卖量进行构建
# 暂时只构建买一卖一和买二卖二相关特征，进行优化时可以加上其余买卖信息
train_df['wap1'] = (train_df['n_bid1'] * train_df['n_bsize1'] + train_df['n_ask1'] * train_df['n_asize1']) / (train_df['n_bsize1'] + train_df['n_asize1'])
test_df['wap1'] = (test_df['n_bid1'] * test_df['n_bsize1'] + test_df['n_ask1'] * test_df['n_asize1']) / (test_df['n_bsize1'] + test_df['n_asize1'])

train_df['wap2'] = (train_df['n_bid2'] * train_df['n_bsize2'] + train_df['n_ask2'] * train_df['n_asize2']) / (train_df['n_bsize2'] + train_df['n_asize2'])
test_df['wap2'] = (test_df['n_bid2'] * test_df['n_bsize2'] + test_df['n_ask2'] * test_df['n_asize2']) / (test_df['n_bsize2'] + test_df['n_asize2'])

train_df['wap_balance'] = abs(train_df['wap1'] - train_df['wap2'])
train_df['price_spread'] = (train_df['n_ask1'] - train_df['n_bid1']) / ((train_df['n_ask1'] + train_df['n_bid1']) / 2)
train_df['bid_spread'] = train_df['n_bid1'] - train_df['n_bid2']
train_df['ask_spread'] = train_df['n_ask1'] - train_df['n_ask2']
train_df['total_volume'] = (train_df['n_asize1'] + train_df['n_asize2']) + (train_df['n_bsize1'] + train_df['n_bsize2'])
train_df['volume_imbalance'] = abs((train_df['n_asize1'] + train_df['n_asize2']) - (train_df['n_bsize1'] + train_df['n_bsize2']))

test_df['wap_balance'] = abs(test_df['wap1'] - test_df['wap2'])
test_df['price_spread'] = (test_df['n_ask1'] - test_df['n_bid1']) / ((test_df['n_ask1'] + test_df['n_bid1']) / 2)
test_df['bid_spread'] = test_df['n_bid1'] - test_df['n_bid2']
test_df['ask_spread'] = test_df['n_ask1'] - test_df['n_ask2']
test_df['total_volume'] = (test_df['n_asize1'] + test_df['n_asize2']) + (test_df['n_bsize1'] + test_df['n_bsize2'])
test_df['volume_imbalance'] = abs((test_df['n_asize1'] + test_df['n_asize2']) - (test_df['n_bsize1'] + test_df['n_bsize2']))

# 历史平移
# 获取历史信息
for val in ['wap1', 'wap2', 'wap_balance', 'price_spread', 'bid_spread', 'ask_spread', 'total_volume', 'volume_imbalance']:
    for loc in [1, 5, 10, 20, 40, 60]:
        train_df[f'file_{val}_shift{loc}'] = train_df.groupby(['file'])[val].shift(loc)
        test_df[f'file_{val}_shift{loc}'] = test_df.groupby(['file'])[val].shift(loc)

# 差分特征
# 获取与历史数据的增长关系
for val in ['wap1', 'wap2', 'wap_balance', 'price_spread', 'bid_spread', 'ask_spread', 'total_volume', 'volume_imbalance']:
    for loc in [1, 5, 10, 20, 40, 60]:
        train_df[f'file_{val}_diff{loc}'] = train_df.groupby(['file'])[val].diff(loc)
        test_df[f'file_{val}_diff{loc}'] = test_df.groupby(['file'])[val].diff(loc)

# 窗口统计
# 获取历史信息分布变化信息
# 可以尝试更多窗口大小已经统计方式，如min、max、median等
for val in ['wap1', 'wap2', 'wap_balance', 'price_spread', 'bid_spread', 'ask_spread', 'total_volume', 'volume_imbalance']:
    train_df[f'file_{val}_win7_mean'] = train_df.groupby(['file'])[val].transform(lambda x: x.rolling(window=7, min_periods=3).mean())
    train_df[f'file_{val}_win7_std'] = train_df.groupby(['file'])[val].transform(lambda x: x.rolling(window=7, min_periods=3).std())

    test_df[f'file_{val}_win7_mean'] = test_df.groupby(['file'])[val].transform(lambda x: x.rolling(window=7, min_periods=3).mean())
    test_df[f'file_{val}_win7_std'] = test_df.groupby(['file'])[val].transform(lambda x: x.rolling(window=7, min_periods=3).std())
```

## 模型融合

模型融合代码参考：

定义`cv_model`函数，内部可以选择使用`lightgbm`、`xgboost`和`catboost`模型，可以依次跑完这三个模型，然后将三个模型的结果进行取平均进行融合。
