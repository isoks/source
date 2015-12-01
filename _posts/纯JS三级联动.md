title: 纯JS三级联动
date: 2015-08-13 17:23:09
tags: .Net
---


没法传文件，放到[百度云](http://7xl2ye.com1.z0.glb.clouddn.com/jsAddress.rar)里面了
---
html:

``` bash

省：<select id="School_Province"></select>
市：<select id="School_City"></select>
区：<select id="School_Area"></select>

```
js:
``` bash

addressInit('School_Province', 'School_City', 'School_Area', '山东省', '烟台市', '莱山区');

```

直接引入百度云js