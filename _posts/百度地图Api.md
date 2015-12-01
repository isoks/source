title: 百度地图Api
date: 2015-10-08 09:16:07
tags: .Net
---


**百度地图Api**



```
   <div class="sjRbox">
                        <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=wGzrIFNWZS1SWdXKbFpfQdqQ"></script>
                        <div id="allmap" style="width: 220px; height: 260px;"></div>
                        <script type="text/javascript">
                            // 百度地图API功能
                            var map = new BMap.Map("allmap");
                            map.centerAndZoom(new BMap.Point("117.289885", "31.870254"), 15);
                            var point = new BMap.Point("117.289885", "31.870254");
                            map.centerAndZoom(point, 15);
                            var marker = new BMap.Marker(point); // 创建标注
                            map.clearOverlays();
                            map.addOverlay(marker); // 将标注添加到地图中
                            marker.setAnimation(BMAP_ANIMATION_BOUNCE); //跳动的动画
                            map.addControl(new BMap.NavigationControl()); //添加默认缩放平移控件
                        </script>
                    </div>
```