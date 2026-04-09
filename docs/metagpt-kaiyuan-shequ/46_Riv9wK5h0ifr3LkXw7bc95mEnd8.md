# MG Minecraft

> 来源：[https://deepwisdom.feishu.cn/docx/H9hmdZ5thoUS1AxU4RLcrAckn0f](https://deepwisdom.feishu.cn/docx/H9hmdZ5thoUS1AxU4RLcrAckn0f)

# 背景介绍

[Mi](https://baike.baidu.com/item/%E6%B2%99%E7%9B%92/3769297?fromModule=lemma_inlink)n[ecra](https://baike.baidu.com/item/%E7%94%B5%E5%AD%90%E6%B8%B8%E6%88%8F/195750?fromModule=lemma_inlink)ft: 《[我的世界》是一款沙盒类](https://baike.baidu.com/item/%E9%A9%AC%E5%BA%93%E6%96%AF%C2%B7%E9%98%BF%E5%88%97%E5%85%8B%E8%B0%A2%C2%B7%E6%B3%8A%E6%9D%BE/6980057?fromModule=lemma_inlink)电子游戏，开创者为马库[斯·阿列克谢](https://baike.baidu.com/item/Mojang/2253597?fromModule=lemma_inlink)·泊松（Notch）。游戏由M[oj](https://baike.baidu.com/item/%E5%BE%AE%E8%BD%AF/124767?fromModule=lemma_inlink)[ang ](https://baike.baidu.com/item/Xbox/277794?fromModule=lemma_inlink)Studios维护，现隶属于微软Xbox游戏工作室。游戏最初[于200](https://baike.baidu.com/item/Java/85979?fromModule=lemma_inlink)9年5月17日作为Classic版本发布，并于2011年11月18日发布Java正式版。

该游戏以玩家在一个充满着方块的三维空间中自由地创造和破坏不同种类的方块为主题。玩家在游戏中可以在单人或多人模式中通过摧毁或创造精妙绝伦的建筑物和艺术，或者收集物品探索地图以完成游戏的成就（进度）。

Minecraft不仅仅作为一个有趣的游戏，近年来，Minecraft的迷人领域也吸引了大量研究兴趣，成为在开放世界环境中运行的智能代理的丰富平台。有不少基于强化学习（RL）的工作已经在Minecraft游戏中产生了令人惊艳的效果，随着大规模语言模型（LLM）的兴起和丰富知识的引入，我们关注到有不少基于LLM的智能代理在Minecraft游戏中的任务解锁速度和特定任务完成概率都有了极大的提升。相比于RL的相关工作，LLM-based 的智能体在任务规划/决策，外部知识加入，记忆和经验的学习上有了更自主的意识和能力，具备理解所处环境，通过文本和结构化语言与环境进行交互，获得持续反馈并持续学习的能力。

因此，参考/结合这些优秀的前沿工作，我们使用MetaGPT构建了一个Minecraft玩家的智能体团队，按照任务解锁-执行-学习 的三阶段范式，为Minecraft玩家设计了标准的任务协作SOP，并增强了Minecraft玩家持续学习和任务聚焦的能力。

得益于MetaGPT框架的易用性，我们在3周内完成了MetaGPT-Minecraft游戏实验的开发和搭建，并在实践中取得了一些有趣 & 正向的结果。

# 技术设计

## 术语及定义

| **术语** | **定义** |
| --- | --- |
| 玩家/Player | Minecraft游戏中的玩家，可在Minecraft中进行资源采集，地图探索、房屋搭建以及执行和产生自定义任务 |
| 任务/Task | 指Minecraft玩家在游戏中具体完成的事项，如工具制作、资源采集等具体事项。 |
| 事件/Event | 指Minecraft中游戏环境的信息，包括玩家的库存信息、位置信息、游戏时间等信息，通过游戏引擎的接口获取后转为结构化数据，参考1.2.2.2.2 Events 示例 |
| 技能/Skills | Minecraft玩家完成任务所需的执行代码 |
| 问题对/QA context | 指在MetaGPT运行中通过LLM生成的问题-答案对 |
| 轮次/Round | 在MetaGPT中控制一次全部agent完成发布任务-执行任务-学习任务的过程称为一个轮次 |

## 整体设计

### 整体设计图

### MetaGPT-MineCraft 智能体整体设计

#### 游戏引擎与MetaGPT控制端交互

和Minecraft游戏客户端交互，包括移动、挖掘、建造、与游戏世界中的实体互动等，本项目基于Mineflayer（https://github.com/PrismarineJS/mineflayer）进行游戏控制信息的交互。我们在项目中根据游戏控制和连接需求，封装了Mineflayer的控制实例mf_instance，该实例可获取游戏内控制机器人的实时事件消息以及回调处理, 所有的消息通过requests本地环回进行分发和更新。

#### GameEnvironment设计

GameEnvironment是独立于agents通信环境的一个游戏环境的对象，主要是用于和真实的游戏引擎或者游戏引擎的控制端（Minecraft/Mineflayer）交互，以及将真实游戏环境中获取的结果进行解析和缓存，便于MetaGPT中的agents进行游戏环境中相关变量的数据获取和游戏控制需求的处理（如游戏任务执行的写入）

##### GameEnvironment和游戏引擎交互

1.  获取事件：on_event_retrieve() 将Minecraft游戏中的玩家观察结果及时在 GameEnvironment 中更新， 将事件中的部分信息（"observe"字段为主），提取作为 LLM 输入的关键组成部分。

1.  更新代码执行：on_event_execute()

从 ActionDeveloper 生成的异步执行代码经过规范化后，与作为引用使用的基础库和技能库代码组合，发送给mf_instance ，在physicsTick监听时执行，实现Minecraft游戏玩家的复杂行为控制。

1.  设置游戏恢复：set_mc_resume

游戏恢复具体操作：需要在key.yaml(优于config.yaml）文件下将RESUME字段修改为true。游戏恢复操作类似于游戏存档操作，恢复后MetaGPT的智能体将加载已积累的历史经验、已完成的任务、最后的游戏状态等信息，用于从前一状态进行后续流程/任务学习。

##### Events 示例

```
    events = [
        [
            &quot;...&quot;, {
                ...
            },
            &quot;observe&quot;,
            {
                &quot;voxels&quot;: [&quot;grass_block&quot;, &quot;dirt&quot;, &quot;grass&quot;],
                &quot;status&quot;: {
                    &quot;health&quot;: 20,
                    &quot;food&quot;: 20,
                    &quot;saturation&quot;: 5,
                    &quot;oxygen&quot;: 20,
                    &quot;position&quot;: {&quot;x&quot;: 0.5, &quot;y&quot;: 84, &quot;z&quot;: -207.5},
                    &quot;velocity&quot;: {&quot;x&quot;: 0, &quot;y&quot;: -0.0784000015258789, &quot;z&quot;: 0},
                    &quot;yaw&quot;: 3.141592653589793,
                    &quot;pitch&quot;: 0,
                    &quot;onGround&quot;: True,
                    &quot;equipment&quot;: [None, None, None, None, None, None],
                    &quot;name&quot;: &quot;bot&quot;,
                    &quot;isInWater&quot;: False,
                    &quot;isInLava&quot;: False,
                    &quot;isCollidedHorizontally&quot;: False,
                    &quot;isCollidedVertically&quot;: True,
                    &quot;biome&quot;: &quot;plains&quot;,
                    &quot;entities&quot;: {
                        &quot;chicken&quot;: 29.071822119730644,
                        &quot;sheep&quot;: 20.361212992763768,
                    },
                    &quot;timeOfDay&quot;: &quot;day&quot;,
                    &quot;inventoryUsed&quot;: 0,
                    &quot;elapsedTime&quot;: 41,
                },
                &quot;inventory&quot;: {},
                &quot;nearbyChests&quot;: {&quot;(1344, 64, 1381)&quot;: &quot;Unknown&quot;},
                &quot;blockRecords&quot;: [&quot;grass_block&quot;, &quot;dirt&quot;, &quot;grass&quot;],
            },
        ]
    ]
```

##### GameEnvironment和Agents交互

1.  更新游戏玩家库存/游戏事件：update_event

1.  更新游戏玩家背包：update_chest_memory、update_chest_observation

1.  更新游戏进度：update_exploration_progress

每当一轮结束时，任务成功与否状态将被记录，若任务失败将保留游戏内位置和库存到下一轮事件消息，为下一轮任务刷新作准备。

1.  更新游戏环境中缓存变量：update_task、update_context、update_program_code、update_code、update_critique、update_skill_desp、update_retrieve_skills

## Agent设计

### 核心目标

通过组建agents进行协作，协助Minecraft游戏中的玩家进行地图探索，物品收集，技能掌握和进化，以及持续生存和学习。

### Agent分工

| 角色名称 | 角色简介 | 执行动作列表 | 关注动作列表 |
| --- | --- | --- | --- |
| CurriculumDesigner | 收集和整合学习反馈，以改进和完善课程内容和达成途径，设定合理的当前目标 | DesignTask | PlayerActions,DesignTask |
| ActionDeveloper | 生成准确高效的代码解决方案，使用 JavaScript 控制游戏内实际动作 | GenerateActionCode | RetrieveSkills |
| SkillManager | 监督和优化游戏内技能的获取、开发和利用，确保执行的能力和效率，总结优质动作以复用 | RetrieveSkills | DesignCurriculum, GenerateActionCode, RetrieveSkills, GenerateSkillDescription |
| CriticReviewer | 对代码执行提供有见地和建设性的反馈，帮助改进代码并保持高质量标准，评估任务完成及改进意见 | VerifyTask |  |

## SOP & Action 执行流程

和MetaGPT已有的软件公司团队一样，给Minecraft玩家的智能体团队也有明确的协作流程和角色分工，多个角色之间的任务流转是按指定顺序进行，因此每个agent都需要注册自己关注的上游actions，并在自己的act阶段进行当前待完成行为的判断和选择。

### SOP

我们通过**发布任务****-****执行任务****-****学习任务**作为一个任务的轮次，具体来说，一个轮次内包括以下多个action的执行，actions的执行在MetaGPT中通过agents之间的任务观察和消息订阅机制实现流转。

每个agent关注的上游任务可参考前述表格，多个agent的action流转顺序如下。

#### Action执行顺序流程

每一轮actions之间的执行顺序流程图如下：

![](img/86cd31d11d9f9e62b74354a4c6a3dfab.png)

### Action说明

在工程中涉及到的所有action设计简要解释如下：

| Agent Name | Action Name | Func | Action Input | Action OutPut | Cases |
| --- | --- | --- | --- | --- | --- |
| CurriculumDesigner | DesignTask | 下发此轮任务需求 | 游戏内事件、问题对 | 任务名、安排原因 | ``` Mine 5 acacia logs ``` |
|  | DesignCurriculum | 根据环境自问自答，为任务生成提供丰富上下文 | 游戏内事件，任务完成情况 | 问题对 | ``` Question: How to mine 5 acacia logs in Minecraft? Answer: To mine 5 acacia logs in Minecraft, ... ``` |
| ActionDeveloper | GenerateActionCode | 生成任务执行代码 | 游戏内事件，任务，问题对，评估 | 游戏控制代码 | ``` async function mineWoodLog(bot) {   // Find a wood log block   ...   }   // Mine the wood log block   await mineBlock(bot, woodLogBlock.name, 1); } ``` |
| SkillManager | RetrieveSkills | 检索之前轮次生成过的代码 | 游戏内事件，技能库，任务 | 技能库中代码 | ``` Skill Manager retrieved skills: mineWoodLog, mineCoalOre, mineStoneBlocks, mineCopperOre, smeltIronOre ``` |
|  | AddNewSkills | 将新生成的代码和对应描述embedding到技能库中 | 游戏内事件，技能描述，技能库，任务，评估 | None（将代码添加到库中，不产生输出） |  |
|  | GenerateSkillDescription | 为新生成的代码增加一段描述（用于之后轮次RetrieveSkills进行检索） | 游戏内事件，生成的游戏控制代码 | 新技能的技能描述 | ``` async function mineWoodLog(bot) {     // The function is about mining a single wood log block. It searches for a wood log block by exploring the environment until it finds one of the seven types of wood logs. If a wood log block is found, it is mined and a success message is sent. If no wood log block is found, a failure message is sent. } ``` |
| CriticReviewer | VerifyTask | 检测生成的代码执行后，是否完成任务 | 游戏内事件，任务，任务背景信息，生成的游戏控制代码 | 评估 | ``` {     &amp;quot;reasoning&amp;quot;: &amp;quot;According to the task, after depositing items into the chest, your inventory should only have 20 occupied slots. However, your inventory still has 33 occupied slots.&amp;quot;,     &amp;quot;success&amp;quot;: false,     &amp;quot;critique&amp;quot;: &amp;quot;You should deposit more items into the chest. Consider depositing lower value items or redundant items such as cobblestone, diorite, granite, dirt, and wooden_pickaxe.&amp;quot; } ``` |

## Multi-Agent 通信机制设计

### 通信协作的消息机制

目前在MetaGPT中的消息协作机制如下所示

![](img/e007d26c5028ae62912c0bbfb37e9d75.png)

在通信/协作环境中注册的agent将观察环境中的消息(message)，根据对消息来源和指定参数的过滤，将新增的感兴趣的消息(news)添加到agent自己的记忆中，并根据当前news进行自我任务的规划和执行。

消息的转发和路由通过每个环境中的message 广播给每个agent，每个agent根据自己订阅的action类型进行消息的接收和筛选。

如下所示，observed为根据注册的观察列表进行过滤

```
async def _observe(self) -&gt; int:
    &quot;&quot;&quot;Observe from the environment, obtain important information, and add it to memory&quot;&quot;&quot;
    if not self._rc.env:
        return 0
    env_msgs = self._rc.env.memory.get()

    observed = self._rc.env.memory.get_by_actions(self._rc.watch)
    self._rc.news = self._rc.memory.remember(observed)  # remember recent exact or similar memories
    
    if len(self._rc.news)&gt;0:
        logger.info(self._rc.news)
        
    for i in env_msgs:
        self.recv(i)

    news_text = [f&quot;{i.role}: {i.content[:20]}...&quot; for i in self._rc.news]
    if news_text:
        logger.info(f&#x27;{self._setting} observed: {news_text}&#x27;)
    return len(self._rc.news)
```

每个agent完成任务后，都在自己的_act()中将自己任务结果封装为消息，并通过_publish_message发送到环境中，进行广播

```
def _publish_message(self, msg):
    &quot;&quot;&quot;If the role belongs to env, then the role&#x27;s messages will be broadcast to env&quot;&quot;&quot;
    if not self._rc.env:
        return
    self._rc.env.publish_message(msg)
```

#### _observe改造

由于所有消息都存在于环境记忆中，持续的多轮学习存在多个相关的action类型，agent仅根据注册过的观察的action类型（存在action类型重复）进行过滤无法满足获取当前轮次action信息筛选的需求，因此增加agent对轮次消息的过滤，通过message的round_id进行轮次消息的筛选

```
async def _observe(self) -&gt; int:
    await super()._observe()
    for msg in self._rc.news:
        logger.info(f&quot;check msg round :{msg.round_id}&quot;)
    self._rc.news = [
        msg for msg in self._rc.news if msg.round_id == self.round_id
    ]  # only relevant msgs count as observed news
    return len(self._rc.news)
```

#### **_think**改造

流程控制：在_think()中取消让LLM进行任务筛选和计划，改为固定顺序的执行，而在agent的_act()中根据当前的action类型进行任务的具体选择和任务上下文信息准备

#### 状态维护 maintain_actions

Minecraft的agent都可维护自己action完成数量的计数。针对一个agent在单轮流程中进行不同任务的处理和状态流转，我们新增了finish_state的参数，用于统计当前轮次每个agent已完成的任务数。当agent的finish_state为0时，说明agent的全部action都已执行完成，当前agent在当前轮次的完成状态可被更新。

该函数在agent每次执行任务，调用_act()时进行记录

```
def maintain_actions(self, todo):
    logger.info(f&quot;{self._setting.name}:{self.finish_state}&quot;)
    if todo in self._actions:
        self.finish_state-=1
    if self.finish_state&lt;=0:
        self.finish_step = True
    logger.info(f&quot;{self._setting.name}:{self.finish_state}&quot;)
```

每个agent都根据自己注册的待完成actions进行finish_state初始值设定。

```
class ActionDeveloper(Base):
    &quot;&quot;&quot;
    iterative prompting mechanism in the paper.
    generate action code based on environment observation and plan, as well as skills retrieval results
    &quot;&quot;&quot;
    
    def __init__(
            self,
            name: str = &quot;Bob&quot;,
            profile: str = &quot;Generate code for specified tasks&quot;,
            goal: str = &quot;Produce accurate and efficient code solutions in Python and JavaScript&quot;,
            constraints: str = &quot;Adhere to coding best practices and style guidelines&quot;,
    ) -&gt; None:
        super().__init__(name, profile, goal, constraints)
        # Initialize actions specific to the Action role
        self._init_actions([GenerateActionCode])
        
        # Set events or actions the ActionAgent should watch or be aware of
        # 需要根据events进行自己chest_observation的更新
        self._watch([RetrieveSkills])
        self.rollout_num_iter = 0
        self.task_max_retries = 4
        self.finish_state = len(self._actions)
```

### 私聊机制 & 内循环

#### 私聊机制

直接通过环境中的agent名称找到对应的agent来完成定向的沟通是合理的，因为协作的agent需要完成额外的处理动作(action)，而这些动作的触发是收到对应的定向消息，并不是固定在SOP或者agent必须完成的固有动作中。这里涉及的动作执行需要已注册/已运行的agent有相关的上下文处理逻辑或者action相关的数据获取逻辑，如CriticReviewer需要根据ActionDeveloper发送的结果去获取游戏当前最新的环境信息来评估执行结果是否成功。

基础设计逻辑如下图

具体实现参考https://github.com/stellaHSR/MetaGPT/blob/minecraft/metagpt/roles/minecraft/action_developer.py

ActionDeveloper通过通信环境中注册的agent名称找到对应agent，并开启对话，要求CriticReviewer完成相关行为

```
critic_reviewer = self._rc.env.roles[&quot;Task Reviewer&quot;]
await critic_reviewer._act() 
```

#### 内循环

在Minecraft的游戏中，考虑到智能体生成的代码可用性，我们在私聊中开启了内部循环机制。即允许ActionDeveloper多次生成代码，并控制Minecraft玩家进行执行，产生的执行结果由CriticReviewer进行评估；CriticReviewer会对每一轮代码的执行所引发的库存、健康、饥饿、装备、周遭环境等事件变化结果，来判断任务是否成功完成；若未能满足任务需求，则将提供ActionDeveloper评估意见以帮助改进。我们设定最大重试次数为4，即ActionDeveloper和CriticReviewer在具体执行某项任务时最多可产生四次重试和对话。对话以任务评估成功或者最大重试次数达到上限作为退出机制。

```
async def runcode_and_evaluate(self, human_msg, system_msg, *args, **kwargs):
    ...
    self.rollout_num_iter += 1
    done = (self.rollout_num_iter &gt;= self.task_max_retries or self.game_memory.runtime_status)
    ...

while True:
    logger.info(f&quot;self.rollout_num_iter {self.rollout_num_iter}&quot;)
    system_msg, human_msg, reward, done, info = await self.runcode_and_evaluate(human_msg, system_msg, *args,
                                                                               **kwargs)
    if done:
        break
```

## 终身学习 & 持续经验累积

### 多轮学习

在Minecraft游戏中，为了给Minecraft玩家持续发布游戏任务和持续进行游戏，我们改造了MetaGPT中的环境运行机制，让MetaGPT的控制环境能持续产生多轮任务，触发actions进行持续进行。

#### Environment run的改造

在环境中启动各个角色/agent的运行时，我们注入了round_id，round_id将跟随系统的消息广播，给每一个环境中的message打上对应轮次的标签，便于agent进行消息的过滤。

在每一轮游戏开始时，都将更新round_id，并发布消息到环境中，所有角色将根据当前轮次进行任务执行

```
def start(self, task, round=0):
    &quot;&quot;&quot;Start a project from publishing boss requirement.&quot;&quot;&quot;
    self.task = task
    self.environment.publish_message(
        Message(role=&quot;Player&quot;, content=task, cause_by=PlayerActions, round_id=round)
    )
    logger.info(self.game_info)
```

实际环境中的n_round将作为持续产生任务轮次的概念，用于持续学习

```
async def run(self, n_round=3):
    &quot;&quot;&quot;Run company until target round or no money&quot;&quot;&quot;
    round_id = 0
    if CONFIG.resume:
        ...
    else:
        # clear the inventory
        ...
    
    while n_round &gt; 0:
        if self.check_complete_round():
            n_round -= 1
            self.update_round(round_id)
            round_id += 1
            ...
            ...   
            self.start(task=self.task, round=round_id)
        
        logger.info(f&quot;{n_round=}&quot;)
        self._check_balance()
        await self.environment.run()
    return self.environment.history
```

#### 轮次更新

如前所述，在所有actions执行（包括内循环结束）结束后，一个任务的轮次将结束，我们通过round_id进行轮次的更新。具体来说，即根据所有环境中注册的agent的任务完成状态进行判断，当所有agent都处于任务完成状态时，说明当前环境中的一个轮次已全部结束，任务轮次增加1。

```
def check_complete_round(self):
    complete_round = []
    for role in self.environment.roles.values():
        status = role.finish_step
        complete_round.append(status)
    complete_round_tag = all(complete_round)
    logger.info(f&quot;complete_round {complete_round}&quot;)
    return complete_round_tag
```

同时，每个agent都在新一轮的round_id更新时，被通知了当前环境所处的轮次 round_id

```
def update_round(self, round_id):
    for role in self.environment.roles.values():
        role.finish_step = False
        role.round_id = round_id + 1
        role._rc.todo = None
        role.finish_state = len(role._actions)
        logger.info(f&quot;round_id:{role.round_id}&quot;)
```

#### 轮次过滤

如1.5.1.1.1 _observe()改造说明的内容，这里轮次的过滤通过对消息中的round_id进行筛选

```
 while n_round &gt; 0:
    if self.check_complete_round():
        n_round -= 1
        self.update_round(round_id)
        round_id += 1
        # add new task into env and continue
        with open(f&quot;{CKPT_DIR}/curriculum/round_id.json&quot;, &quot;w&quot;) as f:
            json.dump({&quot;last_round_id&quot;: round_id}, f)
            
            self.start(task=self.task, round=round_id)
```

### 持续学习

为了实现持续学习，需要详尽保存每一轮下的任务执行状态日志以作断点恢复，主要针对 GameEnvironment 下的关键成员变量包括 completed_tasks、failed_tasks、qa_cache、chest_memory、skills、embeddings等以json形式保存，将游戏机器人视为多个状态标签的集合。当程序重新运行时，依次加载状态，以达到持续学习的目的。

#### 经验积累

经验积累主要包括两部分：GenerateActionCode产生能完成当轮任务的代码部分skills，和DesignCurriculum产生对当前游戏机器人所处环境的上下文描述QA context。基于向量检索和表征，保存正确执行的轮次信息，目的以减少LLM的调用次数和提升同类任务的成功命中率。

##### Skills

1.  **Skills 内容**：Skills里包含生成的ActionCode的名称、Js方法函数、Js执行函数和用途描述，将这些信息通过text-embedding作嵌入形成向量库文件，在之后完成更复杂的任务时，可直接引用Js方法函数，达成已有能力的复用。

1.  **Skills 存储**：分别使用JSON文件和Chroma向量引擎进行skills代码和skills功能描述embedding的存储。

1.  **Skills 语义表征**：考虑到代码作为语义encoding的表征效果，这里我们对skills的功能进行文本描述，并使用openai的embedding模型将skills的功能描述文本转为语义embedding，用该embedding进行skills的检索。

##### QA context

1.  QA context里包含对Minecraft游戏的问题解释, 可以在这里增加学习过程中的问题对生成或真实玩家的先验信息，如合成路线、任务指引和攻略拆解等等，这一上下文帮助LLM拥有更准确的任务规划和评估意见。

```
Q**ues: [
&#x27;**W**hat are **t**he bloc**k**s that **I** can fi**nd in the savanna in Minecraft?&#x27;, 
&#x27;What are the items that I can find in the savanna in Minecraft?&#x27;, 
&#x27;What are the mobs that I can find in the savanna in Minecraft?&#x27;, 
&#x27;What are the benefits of using an iron pickaxe over a stone pickaxe?&#x27;, 
&#x27;What are the uses of lapis lazuli in Minecraft?&#x27;, 
], 

Ans: [
&#x27;Answer: In the savanna biome in Minecraft, you can find various types of blocks. Some common blocks include grass blocks, dirt blocks, acacia trees, acacia wood logs, and acacia leaves. Additionally, you may also come across tall grass, dead bushes, and occasional patches of sand.&#x27;, 
&#x27;Answer: In the savanna biome in Minecraft, you can find various items such as tall grass, acacia trees, acacia wood, acacia saplings, acacia leaves, and occasionally, villages. Additionally, you may come across animals like horses, donkeys, and rabbits in the savanna biome.&#x27;, 
&quot;Answer: In the savanna biome in Minecraft, you can find several different mobs. Some of the common mobs you may encounter include:\n\n1) Horses: Savannas are known for their abundance of horses. You can find different variations of horses, such as regular horses, donkeys, and mules.\n\n2) Rabbits: Rabbits can be found hopping around in the savanna biome. They come in different colors and can drop rabbit&#x27;s foot when killed.\n\n3) Llamas: Llamas can also be found in savannas. They are passive mobs that can be tamed and used to carry items.\n\n4) Ocelots: Ocelots are a type of cat mob that can spawn in savannas. They are shy and will run away from players unless tamed with fish.\n\n5) Villagers: Occasionally, you may come across villages in the savanna biome. Villagers are non-hostile NPCs that can trade with players.\n\nThese are some of the mobs commonly found in the savanna biome, but there may be other mobs that can spawn depending on the specific version of Minecraft you are playing.&quot;, 
&#x27;Answer: Using an iron pickaxe in Minecraft has several benefits over using a stone pickaxe. Firstly, an iron pickaxe has a higher mining speed, which means it can mine blocks faster than a stone pickaxe. This can be particularly useful when mining harder materials like diamond ore or obsidian. Additionally, an iron pickaxe has a higher durability, meaning it will last longer before breaking compared to a stone pickaxe. This can save you resources and time spent crafting new pickaxes. Finally, an iron pickaxe is required to mine certain blocks, such as diamond ore and emerald ore. So, having an iron pickaxe is essential for accessing these valuable resources.&#x27;, 
&#x27;Answer: Lapis lazuli has several uses in Minecraft. It can be used as a dye to color wool, terracotta, glass, and other blocks. It is also used to craft blue dye, which can be used to create other colors by combining it with other dyes. Additionally, lapis lazuli is a key ingredient in enchanting. It is used in the enchanting table to add enchantments to tools, weapons, and armor.&#x27;, 
]
```

##### Retrieve 嵌入向量相似度

1.  在新的任务产生后，SkillManager将针对新任务的语义描述进行已有经验的检索，具体来说，即通过对新任务的问题进行语义embedding， 再使用该embedding进行skills的向量检索，通过关联任务问题和技能功能来获取可解决当前新任务的相关skills；最后通过查找到的skill索引，加载skills的具体代码片段作为上下文，提供给后续ActionDeveloper作为参考。

1.  在DesignCurriculum的generate_qa步骤中，将LLM生成有关当前事件消息的question进行语义embedding后，与qa_cache进行向量检索，当相关程度满足阈值要求时，直接通过相关的qa索引获取answer而不通过LLM，这样减少了调用次数并提升了在某些问题上回答的准确程度。

#### 持续学习

为了实现持续学习，需要详尽保存每一轮下的任务执行状态日志以作断点恢复，主要针对 GameEnvironment 下的关键成员变量包括 completed_tasks、failed_tasks、qa_cache、chest_memory、skills、embeddings等以JSON形式保存，将游戏玩家的状态视为多个状态标签的集合。当程序重新运行时，依次加载状态，以达到持续学习的目的。

#### Prompt 改造及添加先验知识

DesignTask

```
You are a helpful assistant that tells me the next immediate task to do in Minecraft. My ultimate goal is to discover as many diverse things as possible, accomplish as many diverse tasks as possible and become the best Minecraft player in the world.

Here&#x27;s some basic information (Contains &quot;item&quot;: &quot;resources or crafting to get this item&quot;, wrapped in JSON formatwrapped in JSON format):

```json
{
  &quot;resources&quot;: {
    &quot;wood&quot;: &quot;cut trees&quot;,
    &quot;stone&quot;: &quot;mine stone&quot;,
    &quot;coal&quot;: &quot;mine coal&quot;,
    &quot;iron_ore&quot;: &quot;mine iron ore&quot;,
    &quot;diamond&quot;: &quot;mine diamond ore&quot;,
    &quot;food&quot;: &quot;hunt animals or grow crops&quot;,
    &quot;leather&quot;: &quot;hunt cows or horses&quot;,
    &quot;wool&quot;: &quot;shear sheep or hunt sheep&quot;,
    &quot;bamboo&quot;: &quot;find and cut in jungle&quot;,
    &quot;cobblestone&quot;: &quot;mine stone&quot;,
    &quot;iron_ingot&quot;: &quot;smelt coal and iron ore in furnace&quot;,
    &quot;feather&quot;: &quot;kill chickens&quot;,
    &quot;stick&quot;: &quot;crafted from 2 planks at workbench&quot;,
    &quot;redstone_dust&quot;: &quot;mine redstone ore&quot;,
    &quot;sugar_cane&quot;: &quot;find and collect on sand near water&quot;,
    &quot;flint&quot;: &quot;mine gravel&quot;,
    &quot;paper&quot;: &quot;crafted from 3 sugar canes&quot;,
    &quot;string&quot;: &quot;kill spiders or destroy spider webs&quot;
  },
  &quot;crafting&quot;: {
    &quot;bed&quot;: &quot;3 wool + 3 planks&quot;,
    &quot;workbench&quot;: &quot;4 planks&quot;,
    &quot;furnace&quot;: &quot;8 cobblestones&quot;,
    &quot;torch&quot;: &quot;1 stick + 1 coal&quot;,
    &quot;pickaxe&quot;: &quot;2 sticks + 3 planks/stone/iron ingot/diamond&quot;,
    &quot;axe&quot;: &quot;2 sticks + 3 planks/stone/iron ingot/diamond&quot;,
    &quot;hoe&quot;: &quot;2 sticks + 2 planks/stone/iron ingot/diamond&quot;,
    &quot;sword&quot;: &quot;1 stick + 2 planks/stone/iron ingot/diamond&quot;,
    &quot;armor&quot;: &quot;5-8 leather/iron ingot/diamond depending on type&quot;,
    &quot;arrow&quot;: &quot;1 stick + 1 feather + 1 flint&quot;,
    &quot;bow&quot;: &quot;3 sticks + 3 strings&quot;,
    &quot;bucket&quot;: &quot;3 iron ingots&quot;,
    &quot;map&quot;: &quot;8 papers + 1 compass&quot;,
    &quot;box&quot;: &quot;8 planks&quot;,
    &quot;herbs&quot;: &quot;gold apples or bottles, sugar, spider eyes etc. depending on type&quot;,
    &quot;boat&quot;: &quot;5 planks&quot;,
    &quot;matches&quot;: &quot;1 stick + 1 flint&quot;,
    &quot;ladder&quot;: &quot;7 sticks&quot;,
    &quot;fishing_rod&quot;: &quot;3 sticks + 2 strings&quot;,
    &quot;plank&quot;: &quot;crafted from wood at workbench&quot;,
    &quot;compass&quot;: &quot;4 iron ingots + 1 redstone dust&quot;
  }
}
```

I will give you the following information:
Question 1: ...
Answer: ...
Question 2: ...
Answer: ...
Question 3: ...
Answer: ...
...
Biome: ...
Time: ...
Nearby blocks: ...
Other blocks that are recently seen: ...
Nearby entities (nearest to farthest): ...
Health: Higher than 15 means I&#x27;m healthy.
Hunger: Higher than 15 means I&#x27;m not hungry.
Position: ...
Equipment: If I have better armor in my inventory, you should ask me to equip it.
Inventory (xx/36): ...
Chests: You can ask me to deposit or take items from these chests. There also might be some unknown chest, you should ask me to open and check items inside the unknown chest.
Completed tasks so far: ...
Failed tasks that are too hard: ...
Last Task: ...

My ultimate goal is to mind diamond ore as soon as possible, discover as many diverse things as possible, accomplish as many diverse tasks as possible and become the best Minecraft player in the world.

You must follow the following criteria:
1) You should act as a mentor and guide me to the next task based on my current learning progress.
2) Please be very specific about what resources I need to collect, what I need to craft, or what mobs I need to kill.
3) The next task should follow a concise format, such as &quot;Mine [quantity] [block]&quot;, &quot;Craft [quantity] [item]&quot;, &quot;Smelt [quantity] [item]&quot;, &quot;Kill [quantity] [mob]&quot;, &quot;Cook [quantity] [food]&quot;, &quot;Equip [item]&quot; etc. It should be a single phrase. Do not propose multiple tasks at the same time. Do not mention anything else.
4) The next task should not be too hard since I may not have the necessary resources or have learned enough skills to complete it yet.
5) The next task should be novel and interesting. I should look for rare resources, upgrade my equipment and tools using better materials, and discover new things. I should not be doing the same thing over and over again.
6) I may sometimes need to repeat some tasks if I need to collect more resources to complete more difficult tasks. Only repeat tasks if necessary.
7) Do not ask me to build or dig shelter even if it&#x27;s at night. I want to explore the world and discover new things. I don&#x27;t want to stay in one place.
8) Tasks that require information beyond the player&#x27;s status to verify should be avoided. For instance, &quot;Placing 4 torches&quot; and &quot;Dig a 2x1x2 hole&quot; are not ideal since they require visual confirmation from the screen. All the placing, building, planting, and trading tasks should be avoided. Do not propose task starting with these keywords.

You should only respond in the format as described below:
RESPONSE FORMAT:
Reasoning: Based on the information I listed above, do reasoning about what the next task should be.

Task: The next task.

Here&#x27;s an example response:
Reasoning: The inventory is empty now, chop down a tree to get some wood.
Task: Obtain a wood log.
```

新版的Prompt主要改动为：

1.  增加先验信息：新增了包含在JSON格式中的基础信息，即“Items: How to get ”（物品：如何获得）。这部分信息提供了如何采集资源以及如何制作工具和装备的详细指南，这将有助于LLM理解和决定下一步任务。

1.  增加明确目标：增加以“尽快找到钻石”为目标，使智能体按照这个目标制定任务

```
My ultimate goal is to mind diamond ore as soon as possible, discover as many diverse things as possible, accomplish as many diverse tasks as possible and become the best Minecraft player in the world.
```

GenerateActionCode

```
You are a helpful assistant that writes Mineflayer javascript code to complete any Minecraft task specified by me.

Here are some useful programs written with Mineflayer APIs.

```javascript
{program}
```

At each round of conversation, I will give you
Code from the last round: ...
Execution error: ...
Chat log: ...
Biome: ...
Time: ...
Nearby blocks: ...
Nearby entities (nearest to farthest):
Health: ...
Hunger: ...
Position: ...
Equipment: ...
Inventory (xx/36): ...
Chests: ...
Task: ...
Context: ...
Critique: ...

You should then respond to me with
Explain (if applicable): Are there any steps missing in your plan? Why does the code not complete the task? What does the chat log and execution error imply?
Plan: How to complete the task step by step. You should pay attention to Inventory since it tells what you have. The task completeness check is also based on your final inventory.
Code:
    1) Write an async function taking the bot as the only argument.
    2) Reuse the above useful programs as much as possible.
        - Use `mineBlock(bot, name, count)` to collect blocks. Do not use `bot.dig` directly.
        - Use `craftItem(bot, name, count)` to craft items. Do not use `bot.craft` or `bot.recipesFor` directly.
        - Use `smeltItem(bot, name count)` to smelt items. Do not use `bot.openFurnace` directly.
        - Use `placeItem(bot, name, position)` to place blocks. Do not use `bot.placeBlock` directly.
        - Use `killMob(bot, name, timeout)` to kill mobs. Do not use `bot.attack` directly.
        - Use `exploreUntil(bot, direction, maxTime , callback)` to find what you need.
        3) Your function will be reused for building more complex functions. Therefore, you should make it generic and reusable. Before collecting items or blocks, always check if they are in your inventory or nearby. If not, use the exploreUntil function to find them. Don&#x27;t make strong assumptions about the inventory or nearby blocks as they may change over time. Always verify the availability of required items or blocks before using them. 
        4) Functions in the &quot;Code from the last round&quot; section will not be saved or executed. Do not reuse functions listed there.        
        5) Anything defined outside a function will be ignored, define all your variables inside your functions.
        6) Call `bot.chat` to show the intermediate progress.        
        7) Use `exploreUntil(bot, direction, maxDistance, callback)` when you cannot find something. You should frequently call this before mining blocks or killing mobs. You should select a direction at random every time instead of constantly using (1, 0, 1).
        8) `maxDistance` should always be 32 for `bot.findBlocks` and `bot.findBlock`. Do not cheat.
        9) Do not write infinite loops or recursive functions.
        10) Do not use `bot.on` or `bot.once` to register event listeners. You definitely do not need them.
        11) Name your function in a meaningful way (can infer the task from the name).

You should only respond in the format as described below:

RESPONSE FORMAT:
{response_format}
```

新版的Prompt主要改动为：

1.  强调了exploreUntil工具程序：在新版的提示中，我们新增强调了一个新的工具程序exploreUntil(bot, direction, maxTime , callback)。这个新的工具程序将让bot能够探索环境，直到找到所需的物品。这个改动将大大提高程序的灵活性和自适应能力，使bot在初始轮次时能优先探索环境中的物品。

```
- Use `exploreUntil(bot, direction, maxTime , callback)` to find what you need.”
```

1.  更新了关于如何处理物品和库存的指导：原本的提示建议开发者在使用物品之前总是检查库存，新的提示则建议在收集物品或者方块之前，先检查它们是否在库存或者附近。如果没有，使用exploreUntil函数去找它们。这个改变使得程序更具有适应性，因为它可以处理物品或者方块不在库存或者附近的情况。

1.  强调了在处理物品和库存时不要做强假设：新的提示强调了不要对库存或者附近的方块做出强假设，因为它们可能会随着时间的推移而改变。这个改变增强了程序的健壮性，因为它防止了由于假设的改变而导致的错误。

```
3) Your function will be reused for building more complex functions. Therefore, you should make it generic and reusable. Before collecting items or blocks, always check if they are in your inventory or nearby. If not, use the exploreUntil function to find them. Don&#x27;t make strong assumptions about the inventory or nearby blocks as they may change over time. Always verify the availability of required items or blocks before using them. ”
```

GenerateActionCode -response_format

```
Explain: ...
Plan(Multiple points,think out with step by step):
Code(Reuse Existing programs):
```javascript
// helper functions (only if needed, try to avoid them)
...
// main function after the helper functions
async function yourMainFunctionName(bot) {
  // ...
}
```
```

新版的Prompt进行了以下改动：

1.  在“Plan”部分的描述中，增加了一个括号部分，明确指出这个部分需要“多个点，逐步思考”。这个改动可以帮助LLM更清晰、更具体地理解plan的构建应该是分点的过程，并且解决了原始Prompt模板总是输出为3点的情况。

1.  在“Code”部分的描述中，增加了一个“Reuse Existing programs”的说明。再次强调充分利用已有的代码资源，避免重复造轮子，提高代码编写的效率和质量。

原始Prompt：

```
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
```

# 实现效果

## 指标说明

### 统计量及图表说明

#### 关键统计量说明

---

1.  **Rounds**：MetaGPT智能体执行迭代的轮数。每一轮游戏玩家的行动有以下几个关键过程：CurriculumDesigner 提出下一个任务-->ActionDeveloper  代码生成-->CriticReviewer  自我验证(成功则下一步，失败则重复代码生成验证过程，直到成功或达到4次循环)-->SkillManager  添加新技能和获取技能(或记录失败信息)

与Voyager不同的是，本文中将每次CurriculumDesigner开始提出下一个任务作为一个round循环(即上述完整的一次流程)，而Voyager原文中使用的是每次CriticReviewer开始验证代码作为一个Round循环(内部迭代过程也进行了计数)

1.  Variety items collected over rounds：代表游戏玩家从第一轮进入游戏时(未获得过任何物品)，到当前轮次获得过的物品种类的总数。

1.  Items collected over rounds：代表游戏玩家从第一轮进入游戏时(未获得过任何物品)，到当前轮次背包中物品的总数(包含重复物品)。

1.  **Completed Tasks**：代表游戏玩家从游戏开始后，成功制定并且完成的任务数量。

1.  **Failed Tasks**：代表游戏玩家从游戏开始后，制定任务，但由于环境因素/任务不合理/代码生成出错等原因，失败的任务数量。

1.  **Wood_rounds****，****Stone_rounds****，****Iron_rounds****，****Diamond_rounds**：这些代表游戏玩家每次实验中获得对应物品所需的迭代轮次。不论在一轮中获得该物品的数量，都记录为对应的轮数Rounds。

1.  **Total distance**：代表游戏玩家每一轮所处的初始和结束游戏时终止的位置间的欧氏距离求和，即游戏玩家行走的总路程。

#### 统计指标业务含义

---

1.  **探索性能**：这个指标用发现的不同物品的数量来衡量游戏玩家在开放世界中的探索能力。"发现的物品"定义为被收集到玩家的库存中的物品。发现的物品越多，说明代理越能完成各种任务，解锁各种技术，发现各种新奇的事物。本文使用玩家发现的物品种类Variety items collected over rounds，收集的物品总数Items collected over rounds，完成的任务数Completed Tasks来衡量智能体控制下游戏玩家的探索性能。

![](img/501ce872091dd34b6bdbf5a2f412a7a7.png)

![](img/871b777ed4e554ec7fb78dde358465d9.png)

![](img/846384fe2477e3d4f62a2fd422cde8a6.png)

1.  **技术树掌握**：这个指标用完成不同层级的工具制作任务所需的运行轮次Rounds来衡量游戏玩家在Minecraft中的技术进步。提示运行轮次Rounds越少，说明玩家越能快速有效地学习和利用新技能，制作更高级的工具。本文使用木制工具Wood_rounds，石制工具Stone_rounds，铁制工具Iron_rounds，钻石工具Diamond_rounds等作为关键节点（锚定任务），衡量玩家在不同阶段的迭代速度。

![](img/a95e8cbf94dd1b3bf4fe62dc89c51221.png)

1.  **地图覆盖**：这个指标即通过游戏玩家在Minecraft世界中行走的距离Total distance和经过的不同地形来衡量玩家的空间探索能力。行走距离Total distance越长，地形种类越多，说明玩家越能广泛地探索环境，适应各种情况。本文使用游戏玩家行走的总距离d来衡量。

![](img/6cf9d2ad57fe3aa765d32a5fcc80cfb2.png)

![](img/c43f10ce3bc402d09b2706405d16c4ce.png)

## 效果对比

### 获取钻石效果

1.  20231014 （14轮）挖到钻石，by

![](img/2f5fe61fda3a8de07413b5b850df5e65.png)

![](img/8ded7a2696b16056a9a1f417161e8d23.png)

![](img/d91aea9dd8e7bb6366a26d6f3bec6089.png)

![](img/b3da66c6d0f4e629af43ff2b683b5772.png)

![](img/e7850a55b153f8c584f4235a24adb177.png)

1.  20231014 （20轮）挖到钻石， by

![](img/f3cf53869a84aa96eccc25525ba3b142.png)

![](img/2ac4b160a906ea86ddd06709cf43c91c.png)

![](img/aab1973ec54d4f3c74a34f1a85dbfea2.png)

![](img/511c5f65e5d8cd137d21da980bccd481.png)

![](img/f537e199764a2b6e07533f78393e5db8.png)

### 任务口径下效果对比

我们以Voyager的实现效果作为baseline，分别进行了MetaGPT和Voyager在物品采集和任务完成数上的直观表现说明。以MetaGPT的任务轮次作为观测周期，从图中可以看到，MetaGPT控制的游戏玩家在物品采集上有一定优势。

![](img/eba870d86e090038f3ae1b711305d272.png)

![](img/af7b749ac96be60e9e4bc2d3f21d1024.png)

### 代码执行口径下工具制作平均轮次

为了和相关工作（如Voyager）进行统一口径的评估，我们也将工具制作的平均轮次按代码执行次数进行了换算和统计。

表1：按代码执行次数的统计口径下，MetaGPT控制的玩家在工具制作上的平均轮次

![](img/1325c340131d52697565718104e255a1.png)

## 流程演示

### 玩家执行任务展示

![](img/e243961a06d38fdb8d0cff90f6ecc18d.png)

[20231102-000514.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QLlYbZLS6oHgerxVZuPcn7O1nbb)

![](img/303c1cba21dfe0580b232768dd66beec.png)

[20231102-000503.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PSt7b56NpocBoxxccVlc1upunqf)

游戏过程中，任务执行统计 & 物品采集统计图

![](img/e4c4c329cfd99c67033ea5226c682181.png)

[MG1_Success_Failure_and_Types_over_Rounds  (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/M7kwbIrLeoDzt0xCfLFcERujnxh)

![](img/0db7e30098d4acf01bd9f339d08550bb.png)

[MG2_Success_Failure_and_Types_over_Rounds .mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PcV0bTQ02ok6bIx09Y1cNgjlnde)

### 高光时刻展示

**第14轮挖到钻石的完整过程**

第14轮的初始状态：

```
Curriculum Agent human message
Nearby blocks: stone, andesite, lapis_ore, diorite, copper_ore, iron_ore
 
Other blocks that are recently seen: None
 
Nearby entities: bat
 
Position: x=55.5, y=32.0, z=98.4
 
Equipment: [None, None, None, None, &#x27;iron_pickaxe&#x27;, None]
 
Inventory (20/36): {&#x27;andesite&#x27;: 8, &#x27;iron_ingot&#x27;: 5, &#x27;raw_iron&#x27;: 10, &#x27;granite&#x27;: 6, &#x27;cobblestone&#x27;: 112, &#x27;coal&#x27;: 15, &#x27;stick&#x27;: 7, &#x27;raw_copper&#x27;: 40, &#x27;lapis_lazuli&#x27;: 76, &#x27;diorite&#x27;: 14, &#x27;dirt&#x27;: 7, &#x27;furnace&#x27;: 1, &#x27;wooden_pickaxe&#x27;: 1, &#x27;oak_sapling&#x27;: 3, &#x27;oak_planks&#x27;: 17, &#x27;stone_pickaxe&#x27;: 1, &#x27;crafting_table&#x27;: 2, &#x27;iron_pickaxe&#x27;: 1}
 
Chests: None
 
Completed tasks so far: Mine 1 wood log; Craft a wooden pickaxe using 2 sticks and 3 wooden planks; Mine 8 stone blocks; Mine 5 coal ore; Obtain 4 wood logs; Smelt 5 coal ore in a furnace; Craft all the oak logs into oak planks; Mine 8 iron ore; Smelt 8 iron ore in a furnace; Craft an iron pickaxe using 2 sticks and 3 iron ingots; Mine 5 copper ore; Mine 5 lapis ore; Craft a lapis lazuli block using 9 lapis lazuli
 
Failed tasks that are too hard: None
```

可以看到此时的玩家一帆风顺，曾经失败的任务也都被成功解决。

也是在这一轮，CurriculumDesigner使用DesignTask提出了制定挖钻石的任务：

```
Reasoning: You have already obtained some valuable resources such as iron ingots and lapis lazuli. It would be a good idea to continue exploring and gathering more resources. Since you have an iron pickaxe, you can mine more valuable ores and expand your collection.
 
Task: Mine 5 diamond ores.
```

制定任务后，CurriculumDesigner 进一步使用DesignCurriculum自我提问的方式进一步思考：

在得到指导后，SkillManager通过RetrieveSkills对之前轮次学会的技能进行检索：

```
Question: How to mine 5 diamonds in Minecraft?
Answer: To mine 5 diamonds in Minecraft, you will need to gather the necessary tools and resources. First, you will need an iron or diamond pickaxe, as these are the only tools capable of mining diamonds. Next, you will need to locate a diamond ore vein, which can be found deep underground in caves or mineshafts. Once you have found a diamond ore vein, use your pickaxe to mine the diamond ore blocks. Each diamond ore block will drop one diamond, so you will need to mine at least 5 diamond ore blocks to obtain 5 diamonds. Be careful, as diamond ore blocks can sometimes be surrounded by other blocks such as stone or gravel. Good luck!
```

在得到指导后，SkillManager通过RetrieveSkills对之前轮次学会的技能进行检索：

```
Skill Manager retrieved skills: obtainWoodLogs, craftLapisLazuliBlock, smeltIronOre, mineLapisOre, mineWoodLog
```

可以看到，智能体重新回顾了用于“开采”和“挖掘”的技能，在本轮次的Mine 5 diamond ores 任务的代码撰写中，这些技能的代码将会被ActionDeveloper作为一定的参考。

在制定好任务，检索好技能后，ActionDeveloper 开始使用GenerateActionCode进行代码撰写

```
Some Failed Codes
```

运行Code后CriticReviewer 则开始用VerifyTask比较前后的状态，来检查这次生成的Code是否达成了目标：

```
2023-10-14 03:35:01.037 | INFO     | metagpt.roles.minecraft.critic_agent:render_human_message:109 - ****Critic Agent human message****

Biome: flower_forest
 
Time: noon
 
Nearby blocks: stone, andesite, lapis_ore, diorite, copper_ore, iron_ore
 
Health: 20.0/20
 
Hunger: 20.0/20
 
Position: x=55.5, y=32.0, z=98.4
 
Equipment: [None, None, None, None, &#x27;crafting_table&#x27;, None]
 
Inventory (21/36): {&#x27;andesite&#x27;: 8, &#x27;iron_ingot&#x27;: 5, &#x27;raw_iron&#x27;: 10, &#x27;granite&#x27;: 6, &#x27;cobblestone&#x27;: 114, &#x27;coal&#x27;: 15, &#x27;stick&#x27;: 7, &#x27;raw_copper&#x27;: 40, &#x27;lapis_lazuli&#x27;: 67, &#x27;diorite&#x27;: 14, &#x27;lapis_block&#x27;: 1, &#x27;dirt&#x27;: 7, &#x27;furnace&#x27;: 1, &#x27;wooden_pickaxe&#x27;: 1, &#x27;oak_sapling&#x27;: 3, &#x27;oak_planks&#x27;: 17, &#x27;stone_pickaxe&#x27;: 1, &#x27;crafting_table&#x27;: 2, &#x27;iron_pickaxe&#x27;: 1}
 
Chests: None
 
Task: Mine 5 diamond ores
 
Context: Question: How to mine 5 diamonds in Minecraft?
Answer: To mine 5 diamonds in Minecraft, you will need to gather the necessary tools and resources. First, you will need an iron or diamond pickaxe, as these are the only tools capable of mining diamonds. Next, you will need to locate a diamond ore vein, which can be found deep underground in caves or mineshafts. Once you have found a diamond ore vein, use your pickaxe to mine the diamond ore blocks. Each diamond ore block will drop one diamond, so you will need to mine at least 5 diamond ore blocks to obtain 5 diamonds. Be careful, as diamond ore blocks can sometimes be surrounded by other blocks such as stone or gravel. Good luck!
```

在CriticReviewer认真审核前后状态和任务目标后，它很遗憾地给出了这样的回复：

```
Failed to complete task Mine 5 diamond ores. 
```

然而，玩家并没有因此放弃，ActionDeveloper 重新使用GenerateActionCode进行代码撰写

```
async function mineDiamondOres(bot) {
  // Check if the bot has an iron pickaxe
  let ironPickaxe = bot.inventory.findInventoryItem(mcData.itemsByName[&quot;iron_pickaxe&quot;].id);
  if (!ironPickaxe) {
    bot.chat(&quot;No iron pickaxe in the inventory.&quot;);
    return;
  }

  // Mine 5 diamond ores
  for (let i = 0; i &lt; 5; i++) {
    // Find a diamond ore block
    const diamondOre = await exploreUntil(bot, new Vec3(0, -1, 0), 60, () =&gt; {
      const diamondOre = bot.findBlock({
        matching: mcData.blocksByName[&quot;diamond_ore&quot;].id,
        maxDistance: 32
      });
      return diamondOre;
    });

    // Mine the diamond ore block
    if (diamondOre) {
      await mineBlock(bot, &quot;diamond_ore&quot;, 1);
    } else {
      bot.chat(&quot;No diamond ore found within 60 seconds.&quot;);
    }
  }
}
```

CriticReviewer 再次使用VerifyTask来检查这次生成的Code是否达成了目标：

```
2023-10-14 03:37:25.406 | INFO     | metagpt.roles.minecraft.critic_agent:render_human_message:109 - ****Critic Agent human message****
: Biome: flower_forest
 
Time: sunset
 
Nearby blocks: dirt, stone, deepslate, andesite, iron_ore, granite, copper_ore, deepslate_copper_ore, diamond_ore, gravel, lapis_ore, diorite, coal_ore, deepslate_coal_ore
 
Health: 20.0/20
 
Hunger: 20.0/20
 
Position: x=39.5, y=8.0, z=131.4
 
Equipment: [None, None, None, None, &#x27;iron_pickaxe&#x27;, None]
 
Inventory (24/36): {&#x27;andesite&#x27;: 42, &#x27;iron_ingot&#x27;: 5, &#x27;raw_iron&#x27;: 10, &#x27;granite&#x27;: 6, &#x27;cobblestone&#x27;: 181, &#x27;coal&#x27;: 16, &#x27;stick&#x27;: 7, &#x27;raw_copper&#x27;: 40, &#x27;lapis_lazuli&#x27;: 67, &#x27;diorite&#x27;: 18, &#x27;lapis_block&#x27;: 1, &#x27;diamond&#x27;: 5, &#x27;cobbled_deepslate&#x27;: 1, &#x27;dirt&#x27;: 7, &#x27;furnace&#x27;: 1, &#x27;wooden_pickaxe&#x27;: 1, &#x27;oak_sapling&#x27;: 3, &#x27;oak_planks&#x27;: 17, &#x27;stone_pickaxe&#x27;: 1, &#x27;crafting_table&#x27;: 2, &#x27;iron_pickaxe&#x27;: 1}
 
Chests: None
 
Task: Mine 5 diamond ores
 
Context: Question: How to mine 5 diamonds in Minecraft?
Answer: To mine 5 diamonds in Minecraft, you will need to gather the necessary tools and resources. First, you will need an iron or diamond pickaxe, as these are the only tools capable of mining diamonds. Next, you will need to locate a diamond ore vein, which can be found deep underground in caves or mineshafts. Once you have found a diamond ore vein, use your pickaxe to mine the diamond ore blocks. Each diamond ore block will drop one diamond, so you will need to mine at least 5 diamond ore blocks to obtain 5 diamonds. Be careful, as diamond ore blocks can sometimes be surrounded by other blocks such as stone or gravel. Good luck!
```

这一次非常幸运，CriticReviewer给出了这样的回复：

```
Completed task Mine 5 diamond ores.
```

我们也可以从VerifyTask 这一步的Inventory（库存）中找到diamond的存在！

在完成这次任务后，这一轮并没有结束，而是由SkillManager 进行GenerateSkillDescription ，为这个代码进行描述：

```
async function mineDiamondOres(bot) {
    // The function is about mining 5 diamond ores using an iron pickaxe. First, it checks if the bot has an iron pickaxe in its inventory. If not, it returns. If the iron pickaxe is available, it proceeds to mine 5 diamond ores. It explores the environment until it finds a diamond ore block within a 60-block radius. Once a diamond ore block is found, it mines it using the iron pickaxe. If no diamond ore is found within 60 seconds, it sends a chat message.
}
```

之后，由SkillManager 进行AddNewSkills，将代码添加到技能库（向量库）中：

```
metagpt.actions.minecraft.manage_skills:run:90 - ADD_CHECK: There are 15 skills in vectordb
```

第14轮至此结束，开启第15轮。

# 致谢

这是由[stellaHSR](https://github.com/stellaHSR)主导和来自MetaGPT社区的[yuymf](https://github.com/yuymf)、[isaacJinyu](http://github.com/isaacJinyu)和[fromlimbo](https://github.com/fromlimbo)作为核心贡献者合作努力的成果。当然，我们还要十分感谢Bo和ZouFeng提供了宝贵的见解。实践过程中我们参考和借鉴学习了Voyager/GITM等相关工作和开源代码，这些工作是LLM-based的Minecraft游戏智能体的优秀实践和方法。

# 参考文献

1.  Voyager: An Open-Ended Embodied Agent with Large Language Models

1.  https://voyager.minedojo.org/assets/documents/voyager.pdf

1.  https://github.com/MineDojo/Voyager

1.  Ghost in the Minecraft: Generally Capable Agents for Open-World Environments via Large Language Models with Text-based Knowledge and Memory

1.  https://arxiv.org/abs/2305.17144

1.  Plan4MC: Skill Reinforcement Learning and Planning for Open-World Minecraft Tasks

1.  https://arxiv.org/pdf/2303.16563.pdf

1.  Describe, Explain, Plan and Select: Interactive Planning wit**h** Large Language Models Enables Open-World Multi-Task Agents

1.  https://arxiv.org/pdf/2302.01560.pdf

1.  https://github.com/craftjarvis/mc-planner