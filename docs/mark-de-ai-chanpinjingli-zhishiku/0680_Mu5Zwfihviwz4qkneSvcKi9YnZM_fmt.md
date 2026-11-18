
# S10：团队规矩 — 不能想停就停，想做就做

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/QyGSd2D9boYHsYxcf1vczEvDnWe](https://qqs7y1hozd1.feishu.cn/docx/QyGSd2D9boYHsYxcf1vczEvDnWe)

## Team Protocols (团队协议)

有了团队和通信，但没有规矩。直接杀掉一个队友的线程，它可能正写到一半，留下烂摊子。重大操作也没有审批，队友一拿到任务就开干。

这一课加了两个规矩，用的是同一套模式——请求-响应 + 唯一 ID：

关机：领导发请求（带 ID），队友收到后收尾完毕再回复同意（引用同一个 ID）

审批：队友提交方案（带 ID），领导审核后回复批准或拒绝（引用同一个 ID）

![](img/7ffd8ba1c5b6ec610b65f3ca46bc1aa9.png)

所有需要商量着来的场景，都可以套这个模式

![](img/d4df107483542fad7bab89c272f4d860.png)

![](img/b2b243dd6c9f498e4f473f203768669d.png)

![](img/c616eb669595872e610b697e6c59e713.png)

`s01` > `s02` > `s03` > `s04` > `s05` > `s06` | `s07` > `s08` > `s09` > [ `s10` ] `s11` > `s12`

> "队友之间要有统一的沟通规矩" -- 一个 `request-response` 模式驱动所有协商。

> `Harness` 层: 协议 -- 模型之间的结构化握手。

## 问题

`s09` 中队友能干活能通信, 但缺少结构化协调:

关机: 直接杀线程会留下写了一半的文件和过期的 `config.json`。需要握手 -- 领导请求, 队友批准 (收尾退出) 或拒绝 (继续干)。

计划审批: 领导说 "重构认证模块", 队友立刻开干。高风险变更应该先过审。

两者结构一样: 一方发带唯一 ID 的请求, 另一方引用同一 ID 响应。

## 解决方案

```
Shutdown Protocol            Plan Approval Protocol
==================           ======================

Lead             Teammate    Teammate           Lead
  |                 |           |                 |
  |--shutdown_req-->|           |--plan_req------>|
  | {req_id:"abc"}  |           | {req_id:"xyz"}  |
  |                 |           |                 |
  |<--shutdown_resp-|           |<--plan_resp-----|
  | {req_id:"abc",  |           | {req_id:"xyz",  |
  |  approve:true}  |           |  approve:true}  |

Shared FSM:
  [pending] --approve--> [approved]
  [pending] --reject--> [rejected]

Trackers:
  shutdown_requests = {req_id: {target, status}}
  plan_requests     = {req_id: {from, plan, status}}
```

## 工作原理

1. 领导生成 `request_id`, 通过收件箱发起关机请求。

```
shutdown_requests = {}

def handle_shutdown_request(teammate: str) -> str:
    req_id = str(uuid.uuid4())[:8]
    shutdown_requests[req_id] = {"target": teammate, "status": "pending"}
    BUS.send("lead", teammate, "Please shut down gracefully.",
             "shutdown_request", {"request_id": req_id})
    return f"Shutdown request {req_id} sent (status: pending)"
```

1. 队友收到请求后, 用 `approve`/`reject` 响应。

```
if tool_name == "shutdown_response":
    req_id = args["request_id"]
    approve = args["approve"]
    shutdown_requests[req_id]["status"] = "approved" if approve else "rejected"
    BUS.send(sender, "lead", args.get("reason", ""),
             "shutdown_response",
             {"request_id": req_id, "approve": approve})
```

1. 计划审批遵循完全相同的模式。队友提交计划 (生成 `request_id`), 领导审查 (引用同一个 `request_id`)。

```
plan_requests = {}

def handle_plan_review(request_id, approve, feedback=""):
    req = plan_requests[request_id]
    req["status"] = "approved" if approve else "rejected"
    BUS.send("lead", req["from"], feedback,
             "plan_approval_response",
             {"request_id": request_id, "approve": approve})
```

一个 `FSM`, 两种用途。同样的 `pending` -> `approved` | `rejected` 状态机可以套用到任何 `request-response` 协议上。

## 相对 `s09` 的变更

## 试一试

```
cd learn-claude-code
python agents/s10_team_protocols.py
```

试试这些 `prompt` (英文 `prompt` 对 `LLM` 效果更好, 也可以用中文):

1. Spawn alice as a coder. Then request her shutdown.

1. List teammates to see alice's status after shutdown approval

1. Spawn bob with a risky refactoring task. Review and reject his plan.

1. Spawn charlie, have him submit a plan, then approve it.

1. 输入 `/team` 监控状态
