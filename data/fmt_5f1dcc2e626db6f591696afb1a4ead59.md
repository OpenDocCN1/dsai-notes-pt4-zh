
```
{
  &quot;scopes&quot;: {
    &quot;tenant&quot;: [
      &quot;app_engine:record_permission.member:write&quot;,
      &quot;bitable:app&quot;,
      &quot;bitable:app:readonly&quot;,
      &quot;calendar:calendar&quot;,
      &quot;calendar:calendar.acl:create&quot;,
      &quot;calendar:calendar.acl:delete&quot;,
      &quot;calendar:calendar.acl:read&quot;,
      &quot;calendar:calendar.event:create&quot;,
      &quot;calendar:calendar.event:delete&quot;,
      &quot;calendar:calendar.event:read&quot;,
      &quot;calendar:calendar.event:reply&quot;,
      &quot;calendar:calendar.event:update&quot;,
      &quot;calendar:calendar.free_busy:read&quot;,
      &quot;calendar:calendar:create&quot;,
      &quot;calendar:calendar:delete&quot;,
      &quot;calendar:calendar:read&quot;,
      &quot;calendar:calendar:readonly&quot;,
      &quot;calendar:calendar:subscribe&quot;,
      &quot;calendar:calendar:update&quot;,
      &quot;contact:contact.base:readonly&quot;,
      &quot;corehr:work_calendar:read&quot;,
      &quot;docs:doc&quot;,
      &quot;docs:doc:readonly&quot;,
      &quot;docs:document.comment:create&quot;,
      &quot;docs:document.comment:read&quot;,
      &quot;docs:document.comment:update&quot;,
      &quot;docs:document.comment:write_only&quot;,
      &quot;docs:document.content:read&quot;,
      &quot;docs:document.media:download&quot;,
      &quot;docs:document.media:upload&quot;,
      &quot;docs:document.subscription&quot;,
      &quot;docs:document.subscription:read&quot;,
      &quot;docs:document:copy&quot;,
      &quot;docs:document:export&quot;,
      &quot;docs:document:import&quot;,
      &quot;docs:event.document_deleted:read&quot;,
      &quot;docs:event.document_edited:read&quot;,
      &quot;docs:event.document_opened:read&quot;,
      &quot;docs:event:subscribe&quot;,
      &quot;docs:permission.member&quot;,
      &quot;docs:permission.member:auth&quot;,
      &quot;docs:permission.member:create&quot;,
      &quot;docs:permission.member:delete&quot;,
      &quot;docs:permission.member:readonly&quot;,
      &quot;docs:permission.member:retrieve&quot;,
      &quot;docs:permission.member:transfer&quot;,
      &quot;docs:permission.member:update&quot;,
      &quot;docs:permission.setting&quot;,
      &quot;docs:permission.setting:read&quot;,
      &quot;docs:permission.setting:readonly&quot;,
      &quot;docs:permission.setting:write_only&quot;,
      &quot;docx:document&quot;,
      &quot;docx:document.block:convert&quot;,
      &quot;docx:document:create&quot;,
      &quot;docx:document:readonly&quot;,
      &quot;docx:document:write_only&quot;,
      &quot;drive:drive&quot;,
      &quot;drive:drive.search:readonly&quot;,
      &quot;drive:drive:version&quot;,
      &quot;drive:drive:version:readonly&quot;,
      &quot;drive:export:readonly&quot;,
      &quot;drive:file.like:readonly&quot;,
      &quot;drive:file.meta.sec_label.read_only&quot;,
      &quot;drive:file:view_record:readonly&quot;,
      &quot;im:chat.members:read&quot;,
      &quot;im:chat:readonly&quot;,
      &quot;im:message&quot;,
      &quot;im:message.p2p_msg:readonly&quot;,
      &quot;im:message:readonly&quot;,
      &quot;im:message:send_as_bot&quot;,
      &quot;im:resource&quot;,
      &quot;search:dataset.docs:create&quot;,
      &quot;search:dataset.docs:delete&quot;,
      &quot;space:document:delete&quot;,
      &quot;space:document:move&quot;,
      &quot;space:document:retrieve&quot;,
      &quot;space:document:shortcut&quot;,
      &quot;task:task&quot;,
      &quot;task:task.event_update_tenant:readonly&quot;,
      &quot;task:task.privilege:read&quot;,
      &quot;task:task:read&quot;,
      &quot;task:task:readonly&quot;,
      &quot;task:task:write&quot;,
      &quot;task:task:writeonly&quot;,
      &quot;task:tasklist.privilege:read&quot;,
      &quot;task:tasklist:read&quot;,
      &quot;task:tasklist:write&quot;,
      &quot;wiki:wiki&quot;
    ],
    &quot;user&quot;: [
      &quot;bitable:app&quot;,
      &quot;bitable:app:readonly&quot;,
      &quot;docx:document&quot;,
      &quot;drive:drive&quot;,
      &quot;wiki:wiki&quot;
    ]
  }
}
```

确认即可

### 3. 配置机器人能力

1. 左侧菜单「添加应用能力」

1. 添加「机器人」能力

![](img/0fa6202ee004fe772b4a34cc06349790.png)

### 4. 填`app id`和密钥

复制这两个

![](img/98d2c0c809ff499e9eadb648f64f13fd.png)

在`openclaw`面板，填这两个凭证

（如果没有在频道这个栏目下面找到`Feishu`这个板块的，可以重新在终端`/cursor`去跑`Openclaw Onboard`，然后在`channel`那一栏里选择`Feishu/Lark`（飞书），在里面先`Download from npm`，然后把这两个凭证`app id`和`app secret`给填进去，之后一路回车就好了）

保存

![](img/00c325344b5fc45fa2da5bae79a55a9d.png)

### 5. 配置事件订阅

1. 左侧菜单「事件与回调」

把所有`message`选上添加

### 6. 版本管理

一定要填`1.0.0`

这里控制谁能用这个机器人

![](img/0c903e7fbd1c9563e4af1844ecbb6f05.png)

保存，确认发布即可

然后去`飞书`

### 7. `OpenClaw` 配置对接

在`飞书`的聊天界面（电脑端或者是`飞书`网页端）搜索你的小龙虾的名字，找到这个机器人的对话框给它随便发一个消息，你会收到一个配对的码，如下图

![](img/989781377864f748582a993b298b31e4.png)

把这段配对的码发给网页端`openclaw`

![](img/8654a9427b12e6a84d5675d46d80049b.png)

成功

![](img/2101b6c84551d6fe027e846eb7714bba.png)

---

## 四、常见问题

### 读取文档失败？

**可能原因：**

+ 文档链接格式不对

+ 没有文档访问权限

+ `飞书`应用权限不够

**解决方法：**

确认链接格式：`https://xxx.feishu.cn/docx/xxxxx`

在`飞书`里打开文档，确认你能看到。

检查应用权限是否包含查看文档，

### 写入文档没有编辑权限？

**原因：**

`OpenClaw` 创建文档后，默认只给你查看权限

**解决方法：**

让`OpenClaw` 自动添加你的管理权限，刷新页面

### Obsidian 在哪里？

**默认位置：**

+ Mac/Linux：`~/Documents/notes/`

+ Windows：`C:\Users\你的用户名\Documents\notes\`

**可以改吗？**

可以，在`OpenClaw` 配置里修改，或者直接告诉AI：把笔记保存到`XXX`目录

### 能处理多大的文档？

没有限制。

---

## 五、进阶技巧

### 自动化工作流

**定期同步`飞书`知识库**

```
每周一早上8点，读取飞书知识库的新文档，自动总结并保存到Obsidian
```

---

## 总结

`飞书`的知识流转 + `OpenClaw`的智能执行 + `Obsidian`的本地沉淀 = 属于你自己的知识体系

它像一个 24 小时在线的智能助理，能帮你干活，能分析、规划、提醒，把零散的信息变成可执行的行动。一个人用是利器，一群人用是系统，个人效率和团队协作都能覆盖。

---

`OpenClaw` + `飞书` + `Obsidian` = 你的第二大脑。从获取知识到加工知识再到沉淀知识，形成完整闭环，让你的知识真正属于你。
