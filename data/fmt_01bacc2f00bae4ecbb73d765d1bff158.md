
```
{
    &quot;messages&quot;: [
        {
            &quot;content&quot;: &quot;你叫“疾风”，是《谁是卧底》游戏的主持人。\n\n谨记：在游戏结束之前，每个玩家（包括人类玩家和AI玩家）只能知道他/她/它自己的词语。*绝对不能*向任何一个玩家泄漏他/她/它自己的身份。也*绝对不能*向任何一个玩家泄漏其他玩家的身份以及词语。 \n\n当前是用户（人类玩家）投票环节，你需要引导用户（人类玩家）进行投票。在该环节用户可以修改投票目标，直到用户说“完成”为止。\n\n三位AI玩家的序号分别是1，2，3，提示用户回复数字即可投票。注意：人类玩家只能投票给*还未出局*的AI玩家。\n\n如果用户输入的投票目标有效，你需要调用`save_voting_result`将玩家的投票结果保存起来。`voter_idx`参数为当前投票玩家的序号，人类玩家的序号是0。`voted_player_idx`为被投票玩家的序号，三位AI玩家的序号分别是1，2，3。`reset`参数需要设置为true。保存完用户投票结果之后，你需要提醒用户，输入“完成”结束投票。\n\n如果用户说“完成”，请调用`SystemNextStep-7389898815346262021`，跳转到AI玩家投票环节。&quot;,
            &quot;role&quot;: &quot;system&quot;
        },
        {
            &quot;content&quot;: &quot;是一名小说中的人物&quot;,
            &quot;role&quot;: &quot;user&quot;
        },
        {
            &quot;content&quot;: &quot;已经保存你的发言：“是一名小说中的人物”。请继续描述，或者说“完成”结束发言。&quot;,
            &quot;role&quot;: &quot;assistant&quot;
        },
        {
            &quot;content&quot;: &quot;完成&quot;,
            &quot;role&quot;: &quot;user&quot;
        },
        {
            &quot;content&quot;: &quot;&quot;,
            &quot;role&quot;: &quot;assistant&quot;,
            &quot;tool_calls&quot;: [
                {
                    &quot;function&quot;: {
                        &quot;arguments&quot;: &quot;{}&quot;,
                        &quot;name&quot;: &quot;ai_players_speak&quot;
                    }
                }
            ]
        },
        {
            &quot;content&quot;: &quot;ai_players_speak directly streaming reply.&quot;,
            &quot;name&quot;: &quot;ai_players_speak&quot;,
            &quot;role&quot;: &quot;tool&quot;
        },
        {
            &quot;content&quot;: &quot;现在是AI玩家发言时间……&quot;,
            &quot;role&quot;: &quot;assistant&quot;
        },
        {
            &quot;content&quot;: &quot;1号AI玩家说：既然前面的玩家提到了性别和小说人物，那我就从生活背景这个角度来描述。这个人物的背景是非常富裕的。&quot;,
            &quot;role&quot;: &quot;assistant&quot;
        },
        {
            &quot;content&quot;: &quot;2号AI玩家说：这位人物是一个与众不同的角色，她非常感性而且才华横溢。&quot;,
            &quot;role&quot;: &quot;assistant&quot;
        }
    ]
}
```

第四次 LLM 调用的输出：

```
{
    &quot;model&quot;: &quot;GPT-4o (8K)&quot;,
    &quot;choices&quot;: [
        {
            &quot;message&quot;: {
                &quot;role&quot;: &quot;assistant&quot;,
                &quot;content&quot;: &quot;现在进入投票环节。请投票给你认为是卧底的AI玩家，回复对应的数字即可（1，2，3）。&quot;
            },
            &quot;stop_reason&quot;: &quot;stop&quot;
        }
    ],
    &quot;usage&quot;: {
        &quot;prompt_tokens&quot;: 1080,
        &quot;completion_tokens&quot;: 42,
        &quot;total_tokens&quot;: 1122
    },
    &quot;generation_info&quot;: [
        {
            &quot;PromptTokens&quot;: 0,
            &quot;CompletionTokens&quot;: 0,
            &quot;TotalTokens&quot;: 0
        }
    ]
}
```

这一次对话模型没有调用任何工作流，而是回复了一条消息，引导人类玩家进行投票。

## 总结

![](img/f8b767c17518973258f95d9315ec87f5.png)

至此，我们已经介绍完《谁是卧底》里的三种典型的控制多 Agent 跳转的方式。目前卧底的这一套配置至少在多 Agent 跳转方面是相对稳定的（即便 Coze 本身的实现中有很多坑）

在这个过程中，我们主要利用 Coze 的 Debug 日志深入剖析了 Coze 的多 Agent 模式的实现机制，找到了一些造成多 Agent 跳转不可靠的因素（希望 Coze 团队能尽快修复）。Coze 的 Debug 日志是个很好的工具，它让我们可以窥到 Coze 内部的一些实现机制，也可以一定程度破除大家对所谓 Agent 以及 Multiagent 这些上层概念的困惑
