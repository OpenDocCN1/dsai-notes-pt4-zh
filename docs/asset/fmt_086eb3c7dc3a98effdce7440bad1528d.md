
```javascript
// main function after the helper functions

async function yourMainFunctionName(bot) {

  // ...

}
```

新版的Prompt进行了以下改动：

1. 在“Plan”部分的描述中，增加了一个括号部分，明确指出这个部分需要“多个点，逐步思考”。这个改动可以帮助LLM更清晰、更具体地理解plan的构建应该是分点的过程，并且解决了原始Prompt模板总是输出为3点的情况。

1. 在“Code”部分的描述中，增加了一个“Reuse Existing programs”的说明。再次强调充分利用已有的代码资源，避免重复造轮子，提高代码编写的效率和质量。

原始Prompt：

````
Explain: ...
Plan:
1) ...
2) ...
3) ...
...
Code:
```javascript

// helper functions (only if needed, try to avoid them)

...

// main function after the helper functions

async function yourMainFunctionName(bot) {

  // ...

}

```
````

# 实现效果

## 指标说明

### 统计量及图表说明

#### 关键统计量说明

1. `Rounds`：`MetaGPT`智能体执行迭代的轮数。每一轮游戏玩家的行动有以下几个关键过程：`CurriculumDesigner` 提出下一个任务-->`ActionDeveloper` 代码生成-->`CriticReviewer` 自我验证(成功则下一步，失败则重复代码生成验证过程，直到成功或达到4次循环)-->`SkillManager` 添加新技能和获取技能(或记录失败信息)

与`Voyager`不同的是，本文中将每次`CurriculumDesigner`开始提出下一个任务作为一个`round`循环(即上述完整的一次流程)，而`Voyager`原文中使用的是每次`CriticReviewer`开始验证代码作为一个`Round`循环(内部迭代过程也进行了计数)

1. `Variety items collected over rounds`：代表游戏玩家从第一轮进入游戏时(未获得过任何物品)，到当前轮次获得过的物品种类的总数。

1. `Items collected over rounds`：代表游戏玩家从第一轮进入游戏时(未获得过任何物品)，到当前轮次背包中物品的总数(包含重复物品)。

1. `Completed Tasks`：代表游戏玩家从游戏开始后，成功制定并且完成的任务数量。

1. `Failed Tasks`：代表游戏玩家从游戏开始后，制定任务，但由于环境因素/任务不合理/代码生成出错等原因，失败的任务数量。

1. `Wood_rounds`，`Stone_rounds`，`Iron_rounds`，`Diamond_rounds`：这些代表游戏玩家每次实验中获得对应物品所需的迭代轮次。不论在一轮中获得该物品的数量，都记录为对应的轮数`Rounds`。

1. `Total distance`：代表游戏玩家每一轮所处的初始和结束游戏时终止的位置间的欧氏距离求和，即游戏玩家行走的总路程。

#### 统计指标业务含义

1. **探索性能**：这个指标用发现的不同物品的数量来衡量游戏玩家在开放世界中的探索能力。"发现的物品"定义为被收集到玩家的库存中的物品。发现的物品越多，说明代理越能完成各种任务，解锁各种技术，发现各种新奇的事物。本文使用玩家发现的物品种类`Variety items collected over rounds`，收集的物品总数`Items collected over rounds`，完成的任务数`Completed Tasks`来衡量智能体控制下游戏玩家的探索性能。

![](img/871b777ed4e554ec7fb78dde358465d9.png)

![](img/846384fe2477e3d4f62a2fd422cde8a6.png)

1. **技术树掌握**：这个指标用完成不同层级的工具制作任务所需的运行轮次`Rounds`来衡量游戏玩家在Minecraft中的技术进步。提示运行轮次`Rounds`越少，说明玩家越能快速有效地学习和利用新技能，制作更高级的工具。本文使用木制工具`Wood_rounds`，石制工具`Stone_rounds`，铁制工具`Iron_rounds`，钻石工具`Diamond_rounds`等作为关键节点（锚定任务），衡量玩家在不同阶段的迭代速度。

![](img/a95e8cbf94dd1b3bf4fe62dc89c51221.png)

1. **地图覆盖**：这个指标即通过游戏玩家在Minecraft世界中行走的距离`Total distance`和经过的不同地形来衡量玩家的空间探索能力。行走距离`Total distance`越长，地形种类越多，说明玩家越能广泛地探索环境，适应各种情况。本文使用游戏玩家行走的总距离`d`来衡量。

![](img/c43f10ce3bc402d09b2706405d16c4ce.png)

## 效果对比

### 获取钻石效果

1. 20231014 （14轮）挖到钻石，by

![](img/2f5fe61fda3a8de07413b5b850df5e65.png)

![](img/8ded7a2696b16056a9a1f417161e8d23.png)

![](img/d91aea9dd8e7bb6366a26d6f3bec6089.png)

1. 20231014 （20轮）挖到钻石， by

![](img/f3cf53869a84aa96eccc25525ba3b142.png)

![](img/2ac4b160a906ea86ddd06709cf43c91c.png)

![](img/aab1973ec54d4f3c74a34f1a85dbfea2.png)

![](img/f537e199764a2b6e07533f78393e5db8.png)

### 任务口径下效果对比

我们以`Voyager`的实现效果作为baseline，分别进行了`MetaGPT`和`Voyager`在物品采集和任务完成数上的直观表现说明。以`MetaGPT`的任务轮次作为观测周期，从图中可以看到，`MetaGPT`控制的游戏玩家在物品采集上有一定优势。

![](img/af7b749ac96be60e9e4bc2d3f21d1024.png)

### 代码执行口径下工具制作平均轮次

为了和相关工作（如`Voyager`）进行统一口径的评估，我们也将工具制作的平均轮次按代码执行次数进行了换算和统计。

表1：按代码执行次数的统计口径下，`MetaGPT`控制的玩家在工具制作上的平均轮次

![](img/1325c340131d52697565718104e255a1.png)

## 流程演示

### 玩家执行任务展示

![](img/e243961a06d38fdb8d0cff90f6ecc18d.png)

[`20231102-000514.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QLlYbZLS6oHgerxVZuPcn7O1nbb)

![](img/303c1cba21dfe0580b232768dd66beec.png)

[`20231102-000503.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PSt7b56NpocBoxxccVlc1upunqf)

游戏过程中，任务执行统计 & 物品采集统计图

[`MG1_Success_Failure_and_Types_over_Rounds  (1).mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M7kwbIrLeoDzt0xCfLFcERujnxh)

![](img/0db7e30098d4acf01bd9f339d08550bb.png)

[`MG2_Success_Failure_and_Types_over_Rounds .mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PcV0bTQ02ok6bIx09Y1cNgjlnde)

### 高光时刻展示

#### 第14轮挖到钻石的完整过程

第14轮的初始状态：

```
Curriculum Agent human message
Nearby blocks: stone, andesite, lapis_ore, diorite, copper_ore, iron_ore
 
Other blocks that are recently seen: None
 
Nearby entities: bat
 
Position: x=55.5, y=32.0, z=98.4
 
Equipment: [None, None, None, None, 'iron_pickaxe', None]
 
Inventory (20/36): {'andesite': 8, 'iron_ingot': 5, 'raw_iron': 10, 'granite': 6, 'cobblestone': 112, 'coal': 15, 'stick': 7, 'raw_copper': 40, 'lapis_lazuli': 76, 'diorite': 14, 'dirt': 7, 'furnace': 1, 'wooden_pickaxe': 1, 'oak_sapling': 3, 'oak_planks': 17, 'stone_pickaxe': 1, 'crafting_table': 2, 'iron_pickaxe': 1}
 
Chests: None
 
Completed tasks so far: Mine 1 wood log; Craft a wooden pickaxe using 2 sticks and 3 wooden planks; Mine 8 stone blocks; Mine 5 coal ore; Obtain 4 wood logs; Smelt 5 coal ore in a furnace; Craft all the oak logs into oak planks; Mine 8 iron ore; Smelt 8 iron ore in a furnace; Craft an iron pickaxe using 2 sticks and 3 iron ingots; Mine 5 copper ore; Mine 5 lapis ore; Craft a lapis lazuli block using 9 lapis lazuli
 
Failed tasks that are too hard: None
```

可以看到此时的玩家一帆风顺，曾经失败的任务也都被成功解决。

也是在这一轮，`CurriculumDesigner`使用`DesignTask`提出了制定挖钻石的任务：

```
Reasoning: You have already obtained some valuable resources such as iron ingots and lapis lazuli. It would be a good idea to continue exploring and gathering more resources. Since you have an iron pickaxe, you can mine more valuable ores and expand your collection.
 
Task: Mine 5 diamond ores.
```

制定任务后，`CurriculumDesigner` 进一步使用`DesignCurriculum`自我提问的方式进一步思考：

在得到指导后，`SkillManager`通过`RetrieveSkills`对之前轮次学会的技能进行检索：

```
Question: How to mine 5 diamonds in Minecraft?
Answer: To mine 5 diamonds in Minecraft, you will need to gather the necessary tools and resources. First, you will need an iron or diamond pickaxe, as these are the only tools capable of mining diamonds. Next, you will need to locate a diamond ore vein, which can be found deep underground in caves or mineshafts. Once you have found a diamond ore vein, use your pickaxe to mine the diamond ore blocks. Each diamond ore block will drop one diamond, so you will need to mine at least 5 diamond ore blocks to obtain 5 diamonds. Be careful, as diamond ore blocks can sometimes be surrounded by other blocks such as stone or gravel. Good luck!
```

在得到指导后，`SkillManager`通过`RetrieveSkills`对之前轮次学会的技能进行检索：

```
Skill Manager retrieved skills: obtainWoodLogs, craftLapisLazuliBlock, smeltIronOre, mineLapisOre, mineWoodLog
```

可以看到，智能体重新回顾了用于“开采”和“挖掘”的技能，在本轮次的Mine 5 diamond ores 任务的代码撰写中，这些技能的代码将会被`ActionDeveloper`作为一定的参考。

在制定好任务，检索好技能后，`ActionDeveloper` 开始使用`GenerateActionCode`进行代码撰写

```
Some Failed Codes
```

运行`Code`后`CriticReviewer` 则开始用`VerifyTask`比较前后的状态，来检查这次生成的`Code`是否达成了目标：
