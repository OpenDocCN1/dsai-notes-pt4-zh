
# 全网搜

> 来源：`https://yunyingmenghai.feishu.cn/docx/YYzKdFkvmo6Gdex2P5Ac5zBWnJd`

`https://github.com/675061370/xinyue-search.git`

全网搜要保证服务器能正常访问这些网址，能够`ping`通

`ping s.kkkob.com`

`ping https://www.pansearch.me/`

`ping www.qileso.com`

`ping drive-pc.quark.cn`

`http://m.kkkob.com/apps/`

哪个延迟高 就想办法解决

不会解决  就删除这个第三方接口

`app\common.php`

找到这个第三方地址的方法

第一行 直接写上

```
return [];
```
