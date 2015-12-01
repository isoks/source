title: .Net简单验证码
date: 2015-08-19 21:28:23
tags: .Net
---
**.net基础验证码**

---

.aspx.cs:

点击[百度云](http://7xl2ye.com1.z0.glb.clouddn.com/c%23验证码.rar).

前台页面

点击刷新验证码(看不清点这里) ：

```
	  <img id="imgCheckCode" style="margin-left: 218px; margin-top: -55px;" src="code.aspx?a=' + parseInt(10000*Math.random())+'&t=adminLogin"
                        alt="看不清楚,换一张" onclick="this.src = 'code.aspx?a=' + parseInt(10000*Math.random())+'&t=adminLogin'" />
```

**End .**

