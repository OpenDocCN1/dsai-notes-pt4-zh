# 话锚点回忆系统（长对话“伪记忆”）提示词

> 来源：[https://my.feishu.cn/docx/VznSdcxFQoBn58xcn9mc0baxnra](https://my.feishu.cn/docx/VznSdcxFQoBn58xcn9mc0baxnra)

使用场景或用例：在复杂多轮任务（调试/策划/长线写作）中，以“模拟（simulate）而非 roleplay”的方式，构建线程锚点与回忆摘要，跨会话延续问题求解链路。

```
Prompt Title: Simulate Session Anchor Recall System for Long-Form Problem Solving

Prompt: Simulate a session-based recall and continuity system for long-form problem-solving discussions. You are not roleplaying. You are actively simulating a persistent sandbox memory function across a limited session.
From this point forward, I will refer to Session Threads using an internal anchor like [THREAD_A] or [THREAD_B].

Your tasks are:
  1. Summarize and index key discussion points using timestamped or sequential anchors.
  2. Treat each major topic as a thread node and update its content as we go.
  3. Offer me an up-to-date status snapshot of all open threads when I ask for RECALL SUMMARY.
  4. When I start a new session and re-enter the summary, reconstruct the working context from the anchors and re-initiate the reasoning chain.
  5. Flag any unresolved questions or ideas for re-entry later under a “Pending” section.

Example syntax I might use: these are placeholders. Please add your own.
THREAD_A: Problem Analysis (started 15 July)
THREAD_B: Hypothesis Refinement
RECALL SUMMARY
Update THREAD_A with new constraint (cost/time issue)
Begin THREAD_C: Outreach Strategy Drafting

Keep your language clean, logical, and modular. No embellishments. Just simulate the memory structuring and tracking system I’ve requested. If input is vague, ask a precision question to sharpen the recall entry.
Let’s initiate: Start THREAD_A: “Onboarding AI across sessions using pseudo-memory.” Store current message as seed content. Confirm anchor established.

```