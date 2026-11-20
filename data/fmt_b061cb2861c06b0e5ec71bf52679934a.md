
# S11：自治 — 别等老板分配，有活就认领

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/YOH6dpSqDoCvwLx9UVtcZBfZnhg](https://qqs7y1hozd1.feishu.cn/docx/YOH6dpSqDoCvwLx9UVtcZBfZnhg)

## Autonomous Agents (Autonomous Agent)

前面已经有了队友、通信、规则和协作约束，现在要让队友更有自主性：不需要领导逐个分配任务，队友自己找活干

做法是引入任务看板机制，队友在工作阶段每轮都会检查收件箱；当手头的活干完进入空闲阶段后，每 5 秒轮询一次收件箱和任务看板，如果收件箱有新消息或看板有可认领的任务，就自动领取并回到工作状态，如果持续 60 秒既没有新消息也没有可认领的任务，就自动关机，不白白占着资源

![](img/26a61384a1faa82f93e40bdf75d0ee90.png)

此外，由于上下文压缩（S06）可能导致队友忘记自己是谁，系统会在检测到上下文过短时自动重新注入身份信息

![](img/4d8f55645b3b2876a1c469d7f47eeb73.png)

![](img/82acc77b95d2eb1dcdc43901efbc4287.png)

s01 > s02 > s03 > s04 > s05 > s06 | s07 > s08 > s09 > s10 > [ s11 ] s12

> "队友自己看看板, 有活就认领" -- 不需要领导逐个分配, 自组织。

> Harness 层: 自治 -- 模型自己找活干, 无需指派。

### 问题

s09-s10 中, 队友只在被明确指派时才动。领导得给每个队友写 prompt, 任务看板上 10 个未认领的任务得手动分配。这扩展不了。

真正的自治: 队友自己扫描任务看板, 认领没人做的任务, 做完再找下一个。

一个细节: Context Compact (s06) 后 Agent 可能忘了自己是谁。身份重注入解决这个问题。

### 解决方案

```
Teammate lifecycle with idle cycle:

+-------+
| spawn |
+---+---+
    |
    v
+-------+   tool_use     +-------+
| WORK  | &lt;------------- |  LLM  |
+---+---+                +-------+
    |
    | stop_reason != tool_use (or idle tool called)
    v
+--------+
|  IDLE  |  poll every 5s for up to 60s
+---+----+
    |
    +---&gt; check inbox --&gt; message? ----------&gt; WORK
    |
    +---&gt; scan .tasks/ --&gt; unclaimed? -------&gt; claim -&gt; WORK
    |
    +---&gt; 60s timeout ----------------------&gt; SHUTDOWN

Identity re-injection after compression:
  if len(messages) &lt;= 3:
    messages.insert(0, identity_block)
```

### 工作原理

1. 队友循环分两个阶段: `WORK` 和 `IDLE`。LLM 停止调用工具 (或调用了 `idle`) 时, 进入 `IDLE`。

```
def _loop(self, name, role, prompt):
    while True:
        # -- WORK PHASE --
        messages = [{&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: prompt}]
        for _ in range(50):
            response = client.messages.create(...)
            if response.stop_reason != &quot;tool_use&quot;:
                break
            # execute tools...
            if idle_requested:
                break

        # -- IDLE PHASE --
        self._set_status(name, &quot;idle&quot;)
        resume = self._idle_poll(name, messages)
        if not resume:
            self._set_status(name, &quot;shutdown&quot;)
            return
        self._set_status(name, &quot;working&quot;)
```

1. 空闲阶段循环轮询收件箱和任务看板。

```
def _idle_poll(self, name, messages):
    for _ in range(IDLE_TIMEOUT // POLL_INTERVAL):  # 60s / 5s = 12
        time.sleep(POLL_INTERVAL)
        inbox = BUS.read_inbox(name)
        if inbox:
            messages.append({&quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: f&quot;&lt;inbox&gt;{inbox}&lt;/inbox&gt;&quot;})
            return True
        unclaimed = scan_unclaimed_tasks()
        if unclaimed:
            claim_task(unclaimed[0][&quot;id&quot;], name)
            messages.append({&quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: f&quot;&lt;auto-claimed&gt;Task #{unclaimed[0][&#x27;id&#x27;]}: &quot;
                           f&quot;{unclaimed[0][&#x27;subject&#x27;]}&lt;/auto-claimed&gt;&quot;})
            return True
    return False  # timeout -&gt; shutdown
```

1. 任务看板扫描: 找 `pending` 状态、无 `owner`、未被阻塞的任务。

```
def scan_unclaimed_tasks() -&gt; list:
    unclaimed = []
    for f in sorted(TASKS_DIR.glob(&quot;task_*.json&quot;)):
        task = json.loads(f.read_text())
        if (task.get(&quot;status&quot;) == &quot;pending&quot;
                and not task.get(&quot;owner&quot;)
                and not task.get(&quot;blockedBy&quot;)):
            unclaimed.append(task)
    return unclaimed
```

1. 身份重注入: 上下文过短（说明发生了压缩）时, 在开头插入身份块。

```
if len(messages) &lt;= 3:
    messages.insert(0, {&quot;role&quot;: &quot;user&quot;,
        &quot;content&quot;: f&quot;&lt;identity&gt;You are &#x27;{name}&#x27;, role: {role}, &quot;
                   f&quot;team: {team_name}. Continue your work.&lt;/identity&gt;&quot;})
    messages.insert(1, {&quot;role&quot;: &quot;assistant&quot;,
        &quot;content&quot;: f&quot;I am {name}. Continuing.&quot;})
```

### 相对 s10 的变更

### 试一试

```
cd learn-claude-code
python agents/s11_autonomous_agents.py
```

试试这些 prompt (英文 prompt 对 LLM 效果更好, 也可以用中文):

1. Create 3 tasks on the board, then spawn `alice` and `bob`. Watch them auto-claim.

1. Spawn a coder teammate and let it find work from the task board itself

1. Create tasks with dependencies. Watch teammates respect the blocked order.

1. 输入 `/tasks` 查看带 `owner` 的任务看板

1. 输入 `/team` 监控谁在工作、谁在空闲
