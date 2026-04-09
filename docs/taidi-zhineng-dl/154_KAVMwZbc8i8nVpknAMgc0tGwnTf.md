# Docker configs

> 来源：[https://learning.feishu.cn/docx/WnD0dJzW7oqeeux7w5bce91Mnyh](https://learning.feishu.cn/docx/WnD0dJzW7oqeeux7w5bce91Mnyh)

https://stackoverflow.com/questions/24832972/docker-apt-get-update-fails

/etc/docker/daemon.json

```
{
    &quot;dns&quot;: [&quot;10.1.2.3&quot;, &quot;8.8.8.8&quot;],
    &quot;dns&quot;: [&quot;8.8.8.8&quot;, &quot;8.8.4.4&quot;],
}
```

sudo systemctl restart docker