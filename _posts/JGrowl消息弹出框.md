title: JGrowl消息弹出框
date: 2015-08-14 12:42:50
tags: .Net
---

JGrowl消息弹出框

<img src="http://img-storage.qiniudn.com/15-8-14/98047300.jpg" />
同样，引入js
放到[百度云](http://7xl2ye.com1.z0.glb.clouddn.com/jgrowl消息弹出框.rar)里了.
关于 jquery-migrate-1.1.1.min  ，应用迁移辅助插件 。用不用无所谓。

js：
```
 $.jGrowl("jGrowl弹出框"); 可以代替 alert("123");
当然也可以应用到全局：

window.alert = function (msg) {
    $.jGrowl(msg);
}
```