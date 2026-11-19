
# 虚拟小镇

> 来源：[https://deepwisdom.feishu.cn/docx/GlAPdjh3poONiyxprBhcHZ0hnUb](https://deepwisdom.feishu.cn/docx/GlAPdjh3poONiyxprBhcHZ0hnUb)

## 构建你的小世界：虚拟小镇

![](img/69053501e5a77c7631cb59fbfaab747a.png)

斯坦福小镇中构建了一个虚拟RPG世界，AI能够在其中自由探索、互相合作、甚至发展友情、举办活动、构建家庭。

本节中，你将学会使用`MetaGPT`提供的模块，重现斯坦福小镇的互动空间，并以此为切入点来学习设计自己的互动场景。

## 快速开始

> 需要注意，由于本项目使用到了`Embedding`服务，因此只能使用`OpenAI`的`Key`进行模拟

斯坦福小镇`demo`的入口在`examples/stanford_town/run_st_game.py`中，在命令行输入如下命令即可开始模拟

```
cd examples/stanford_town
python run_st_game.py "Host a open lunch party at 13:00 pm" "base_the_ville_isabella_maria_klaus" "test_sim" --temp_storage_path "temp_storage"
```

目前主要有如下几个重要参数，可以输入`python run_st_game.py --help`查看：

1. `idea`：将传给小镇中的第一位居民，并以此开始模拟

2. `fork_sim_code`：可以沿用[过去的模拟结果](https://github.com/joonspk-research/generative_agents/tree/main/environment/frontend_server/storage)，相当于一套居民状况模板，存放在`examples/stanford_town/storage`中。也可以使用原版斯坦福小镇中的其他模板。

3. `sim_code`：当前模拟结果保存的文件夹命名，模拟中会不断更新该文件夹

4. `temp_storage_path`：存储模拟的`step`，推荐用`--temp_storage_path "temp_storage"`

## 小镇环境讲解

为了复现斯坦福小镇的交互逻辑，这里使用了[`MetaGPT`的环境模块](https://github.com/geekan/MetaGPT/blob/main/metagpt/environment/stanford_town/env_space.py)来实现，从而每个角色可以与环境交互，获取观察并更新状态。具体代码

### 导入环境相关的类并初始化环境

这里选取了斯坦福小镇自带的`Maze`

```
from metagpt.environment.stanford_town.stanford_town_ext_env import StanfordTownExtEnv
from metagpt.environment.stanford_town.env_space import (
    EnvAction,
    EnvActionType,
    EnvObsParams,
    EnvObsType,
)
from metagpt.ext.stanford_town.utils.const import MAZE_ASSET_PATH

env = StanfordTownExtEnv(maze_asset_path=MAZE_ASSET_PATH)
```

### 观察环境

这里我们选取了小镇地图坐标的`(72,14)`，即设定的伊莎贝拉初始位置作为案例，可以通过传给环境观察类型来获取需要的信息，例如：

1. 当前坐标地址，可以发现伊莎贝拉初始在自己公寓的主卧床上：

```
env.observe(EnvObsParams(obs_type=EnvObsType.TILE_PATH, coord=(72, 14))) 
>>> "the Ville:Isabella Rodriguez's apartment:main room:bed"
```

2. 当前坐标的详细信息，其中`collision`代表当前位置是否不可通行（如墙、障碍等）

```
env.observe(EnvObsParams(obs_type=EnvObsType.GET_TITLE, coord=(72, 14))) 
# 输出如下
{'world': 'the Ville', # 当前所属地图
 'sector': "Isabella Rodriguez's apartment", # 当前区域、如广场、公寓……
 'arena': 'main room', # 当前活动场所，如浴室、厨房……
 'game_object': 'bed', # 当前位置的物品，如床、门、桌子等
 'spawning_location': 'sp-A',
 'collision': False, # 是否不可通行
 'events': {("the Ville:Isabella Rodriguez's apartment:main room:bed",None,None,None)}}
```

可以传递动作给环境来获取完整的观察：

```
# 删除在(72,14)位置，由伊莎贝拉产生的事件
action = EnvAction(action_type=EnvActionType.RM_TITLE_SUB_EVENT, coord=(72, 14), subject="Isabella Rodriguez")  
# 执行动作并得到新的完整观察
observation,*_ = env.step(action)  
```

这里的观测值是一个字典，总共有三个键，分别如下：

| 字段 | 说明 | 取值 |
| --- | --- | --- |
| `collision_maze` | 描述地图不可通过区域的二维矩阵 | （地图高度，地图长度），0为可通过，32125为不可通过 |
| `address_tiles` | 存放地址所在格点范围的字典，例如：`{"正方形区域":{(0,0),(0,1),(1,0),(1,1)}}` | （地图高度，地图长度） |
| `tiles` | 存放每个格点的详细信息，例如`obs['tiles'][14][72]`可以获得和该输出一致的结果 | （地图高度，地图长度） |

### 执行动作

角色接受到[`st_role.py`](https://github.com/geekan/MetaGPT/blob/main/metagpt/ext/stanford_town/roles/st_role.py#L546-L591)的观察值后，会结合其他信息传入大模型并获取动作，其中一个动作的字段如下：

| 字段 | 说明 | 取值 |
| --- | --- | --- |
| `action_type` | 动作类型 | 不同动作对应不同`IntEnum`值，依次`None`、`ADD_TILE_EVENT`、`RM_TILE_EVENT`、`TURN_TILE_EVENT_IDLE`、`RM_TITLE_SUB_EVENT` |
| `coord` | 地图上的坐标，尺寸为`(2,)`的`ndarray` | 第一个数值最小值为0，最大值为地图宽度。第二个数值最小值为0，最大值为地图高度。 |
| `subject` | 事件的主题，文本类型 | 最大长度256 |
| `event` | 事件，包含4个文本的元组类型 | 每个元素最大长度256 |

例如，若伊莎贝拉在`(72,14)`处睡觉，则`action`为

```
EnvAction(action_type=1, coord=array([72, 14]), subject='Isabella Rodriguez', 
event=["the Ville:Isabella Rodriguez's apartment:main room:bed", 
"Isabella Rodriguez",
"is",
"sleep"])
```

## 总结

本节中，我们学习了如何使用`MetaGPT`版本的斯坦福小镇，并了解了斯坦福小镇环境的基本使用，例如从环境中获取观察，使用动作更新环境等。

## What's Next!

+ 目前`第四面墙`的`MetaGPT`的斯坦福小镇仍存在一定的版本适配问题（如前端服务、`Embedding`适配其他模型等），欢迎想参与开发的朋友们多提`issue`、`PR`，让`MetaGPT`的小镇更加热闹

+ 本案例主要依照[原版斯坦福小镇](https://github.com/joonspk-research/generative_agents)复现，尝试更改地图和人物模板，创造一个自己的小镇
