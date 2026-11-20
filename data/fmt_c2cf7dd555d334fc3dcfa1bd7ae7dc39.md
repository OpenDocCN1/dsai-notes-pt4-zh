
# GraphRAG Indexer 执行日志

日志已启用，日志文件路径为 `/root/autodl-tmp/graphrag/openl_big/logs/indexing-engine.log`。

## 工作流任务列表

GraphRAG Indexer 依次执行以下任务，其中输入加载阶段处理了 1 个文本文件，未进行过滤：

- `Loading Input` (`InputFileType.text`) - 1 files loaded (0 filtered)
- `create_base_text_units`
- `create_final_documents`
- `create_base_entity_graph`
- `create_final_entities`
- `create_final_nodes`
- `create_final_communities`
- `create_final_relationships`
- `create_final_text_units`
- `create_final_community_reports`
- `generate_text_embeddings`

## 中间数据输出

### `create_base_text_units`

执行过程中输出空 DataFrame：

```
Empty DataFrame
Columns: []
Index: []
```

随后显示基础文本单元数据：

```
                                 id  ...                                       text_unit_ids
0  8e6f8214f47f109edf9d495128e17702  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0, eb09283810a...

[1 rows x 5 columns]
```

### `create_final_entities`

```
                                  id  ...                                       text_unit_ids
0   53f687e3c6fb4fa8aca8034a33d8c949  ...                  [06c8de30f1c6aa69b90bd00a0c0a10f0]
1   6caf8d9e8e5944dfb154b2c6f52550c1  ...                  [06c8de30f1c6aa69b90bd00a0c0a10f0]
2   43cae4c3618c4299a872dd1c7b02cdad  ...                  [06c8de30f1c6aa69b90bd00a0c0a10f0]
3   42d46b75c1174bcea5c1e59e8b73a0cd  ...                  [06c8de30f1c6aa69b90bd00a0c0a10f0]
4   23a9ab39823c42a294dc14a89d5b9d42  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0, 0b2bd870cfc...
..                               ...                                                ...
64  ade2dd0eae194efa8af61d38f431d117  ...
65  5ebfaa009c2345b89596164013b4f496  ...
66  a5cb6739856440d9a23aeeae2475e616  ...  [c8f3e48d086da134b504c1f547cfb7f2, e475709fdc2...
67  a3e782a3dcfb4dafbaf60cf2e17c3163  ...
68  afccc91a08e44386bf10cd6efaf9568e  ...

[69 rows x 6 columns]
```

### `create_final_nodes`

```
                                   id  human_readable_id  ...  x  y
0    53f687e3c6fb4fa8aca8034a33d8c949                  0  ...  0  0
1    6caf8d9e8e5944dfb154b2c6f52550c1                  1  ...  0  0
2    43cae4c3618c4299a872dd1c7b02cdad                  2  ...  0  0
3    42d46b75c1174bcea5c1e59e8b73a0cd                  3  ...  0  0
4    23a9ab39823c42a294dc14a89d5b9d42                  4  ...  0  0
..                                ...                ...  ... .. ..
172  475aad32dd0147f4a5013fd452b3be6c                 55  ...  0  0
180  6e208519f7ef4d8797f5bcb253642e6b                 60  ...  0  0
181  82b60d49013541a88fc35ec89846d68b                 61  ...  0  0
190  a3e782a3dcfb4dafbaf60cf2e17c3163                 67  ...  0  0
191  afccc91a08e44386bf10cd6efaf9568e                 68  ...  0  0

[93 rows x 8 columns]
```

### `create_final_communities`

```
                                      id  human_readable_id  ...      period   size
0   8f9737a8-0f20-4abb-bc82-8953af8d29b4                  2  ...  2024-11-28     11
1   11451f95-33d3-40ea-a450-f029e0c2c494                  5  ...  2024-11-28      7
2   4f262025-431d-4e7e-8cd7-04649f891156                  1  ...  2024-11-28     13
3   f81b5cb0-84c4-4a61-a2e4-cafa6731250f                  3  ...  2024-11-28      9
4   7690f9f8-45d5-4c65-8c85-e66727fb389d                  0  ...  2024-11-28      9
5   e8e32689-ede1-4235-b529-ce7b40e44137                  4  ...  2024-11-28      2
6   275f956d-2746-468b-8edb-877f8866fba7                  9  ...  2024-11-28      2
7   c9fc9d7b-272f-4320-90af-53db4f358a7d                  8  ...  2024-11-28      7
8   8dd63107-31a7-4e23-a6af-3a1fc6270688                  7  ...  2024-11-28      3
9   59817579-b653-46a8-bc49-5623f22f6080                  6  ...  2024-11-28     10
10  8b3c3c42-ccc0-43ee-88c7-6b4256adc349                 10  ...  2024-11-28      2

[11 rows x 10 columns]
```

### `create_final_relationships`

```
                                  id  ...                       text_unit_ids
0   bc638838eac64e33af7369d5a04f2ba6  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0]
1   f3c6ac3e7e9c42488e4ee01818b5a364  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0]
2   96fe73398c5c4bc19ace2a43fbc3902f  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0]
3   96d660f0432548cfa92515f24c4e5921  ...  [06c8de30f1c6aa69b90bd00a0c0a10f0]
4   a858fd4760c64285a912158053904279  ...  [0b2bd870cfc6f1f783c16da523520769]
..                               ...                                                ...
82  4929a6e03b9f4117acc2b45463f3af1a  ...  [94c65cf6e1f53cd71e75c2422004be83]
83  880bf09b74704f628d9c950fafe7ec55  ...
84  62b2ea29f940471c90a3b0a74832638b  ...
85  3e76f55f3e0641ba9ff16de3392505c4  ...
86  15439d80a02d49fe933865d30c5cd467  ...

[87 rows x 8 columns]
```

### `create_final_text_units`

```
                                  id  ...                                    relationship_ids
0   06c8de30f1c6aa69b90bd00a0c0a10f0  ...  [bc638838eac64e33af7369d5a04f2ba6, f3c6ac3e7e9...
1   eb09283810aaf771f46001baffa58b18  ...                                        None
2   83ef1725e2b08b93e65efd7048514e12  ...                                        None
3   3de0de1563d4e3f909ea96f6ec5ddb0f  ...                                        None
4   85472ab1fb68619a806dc3f251491320  ...                                        None
5   4d236618b0271bda1fada462f0193fac  ...                                        None
6   a20df3df79c82146e51f086d8e322cb6  ...  [b0aa2438e4ac48a7925404790293056d, 70a9a89ec2e...
7   2e51979c409deaa313573f64eb6dd527  ...  [f35299162aeb4e2ca3cec7f2016562df, 7dbc8fe820b...
8   50aeeb1b70fcc1ff0eee53d07bcd4667  ...  [292f2724a8eb4f2ebcbbdf28df168027, be22c979ea9...
9   996f31a8e027c48207ad08490f424e32  ...  [547b543a1d7b490fab8a4d173b7381b0, 7dbc8fe820b...
10  3f049842e623a6c0ee144e9a226b0df1  ...                                        None
11  7aed76e5ec9bfb74514dceea559f2a3c  ...                                        None
12  0f4ffc58d9e7d2d916c9c3a5f56108ce  ...  [547b543a1d7b490fab8a4d173b7381b0, bf424711073...
13  ac799de7a8a7dc83fcdda620bf7098ff  ...                                        None
14  f36895a18172ad61c54cc8d31a69eaf2  ...                                        None
15  542771a511d1f92617f01b9404149a4a  ...                                        None
16  3cac3e1fe9913ea445624018d65a2591  ...  [a10b0a1549f848b58862ff311754f2ad, 47c52170ea8...
17  b7fdd63e7d17aea3025a617869de708e  ...                                        None
18  f5e61ef344124885ec08cdcebc3ae7b2  ...  [a10b0a1549f848b58862ff311754f2ad, 47c52170ea8...
19  561368bcd1ec8ccbca0488c7f4e78a8a  ...  [e746aadc5cfe48cdba0268b81cf80ba6, e437a76da8f...
20  700f00a2299026a9552353ac952e4d78  ...                                        None
21  4800cb8b69bcfc697ffa6f453e83b384  ...  [7dbc8fe820b74b87ba16213750ed17c7, 4613f262abd...
22  94c65cf6e1f53cd71e75c2422004be83  ...  [1ed3cd62731249909c403c9a85b54967, 05c5e36ac3e...
23  4142225df511b047dc147e3c8b4b417b  ...  [7dbc8fe820b74b87ba16213750ed17c7, 786e3d41584...
24  21f6d1bc0d62a3950efcf3f89e748cee  ...                                        None
25  0b2bd870cfc6f1f783c16da523520769  ...  [a858fd4760c64285a912158053904279, 36b80f01850...
26  89fe8b354d928ee29a4ed6f421023c91  ...                                        None
27  3da5db6a03777f2efbfb34b5e03d2f0e  ...                                        None
28  a9d16a2fecfb2dcfc5ac8d0cb1dbc713  ...  [a10b0a1549f848b58862ff311754f2ad, bf424711073...
29  72f5187bd7a77990c165331174590b13  ...                                        None
30  f3338d068740c7f5c6db66400738c7d1  ...                                        None
31  e475709fdc24b53d8bb49e90c75488c8  ...  [d706c679478249ebb3020b6038eacf9b, db0da1587a8...
32  c8f3e48d086da134b504c1f547cfb7f2  ...  [db0da1587a824f03baac93418f220bf5, 1aeae8514cb...

[33 rows x 7 columns]
```

### `create_final_community_reports`

```
                                      id  human_readable_id  ...      period   size
0   487570ff-22db-4675-9a85-bc07bed5de9f                  6  ...  2024-11-28   10.0
1   21d30332-290a-434d-83c8-973d1be25db2                  7  ...  2024-11-28    3.0
2   b2eac490-812d-403e-bbb6-8e71e6c9a802                  8  ...  2024-11-28    7.0
3   d0cffc95-0003-4482-bc5b-f31a2a695668                  9  ...  2024-11-28    2.0
4   20743dbd-d3f7-416f-8342-6205aca58b3c                 10  ...  2024-11-28    2.0
5   25d0a149-1e17-48e7-8ea1-94fc27ee250a                 -1  ...  ...         NaN    NaN
6   2672f205-e527-417d-bbb0-9b8e1d677a1e                  0  ...  2024-11-28    9.0
7   99260f64-40c3-410f-a7a1-13ec8abf759d                  1  ...  2024-11-28   13.0
8   ca817d6e-121a-45d1-9e1c-a7a770d984ab                  2  ...  2024-11-28   11.0
9   e0038a42-fa2c-4199-bc3d-849b9234965f                  3  ...  2024-11-28    9.0
10  daa253c4-2700-4d1f-8c8b-cc800b6912dd                  4  ...  2024-11-28    2.0
11  1e82d0c1-0af7-43f9-a684-4d440701b763                  5  ...  2024-11-28    7.0

[12 rows x 13 columns]
```

## 异常与警告

在 `generate_text_embeddings` 阶段，`lance::dataset` 组件提示未找到现有数据集，将自动创建：

```
No existing dataset at /root/autodl-tmp/graphrag/openl_big/output/lancedb/default-entity-description.lance, it will be created
No existing dataset at /root/autodl-tmp/graphrag/openl_big/output/lancedb/default-community-full_content.lance, it will be created
No existing dataset at /root/autodl-tmp/graphrag/openl_big/output/lancedb/default-text_unit-text.lance, it will be created
```

## 执行结果

所有工作流成功完成：

```
All workflows completed successfully.
```
