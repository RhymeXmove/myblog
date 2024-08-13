---
title: Cesium中billboard广告牌使用PinBuilder创建的自定义样式地图图钉
date: 2021-08-08 15:16:31
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Cesium.jpeg
tags:
- Cesium
categories:
- Cesium
---



==============

<!--more-->



官方文档：
http://cesium.xin/cesium/cn/Documentation1.72/PinBuilder.html?classFilter=pin

1. HTMLCanvasElement.toDataURL()方法返回一个包含图片展示的 data URI （可以使用 type 参数其类型，默认为 PNG 格式。图片的分辨率为96dpi）。
2. 将生成的dataURI直接添加到billboard的image属性进行配置。
3. 添加heightReference: Cesium.HeightReference.CLAMP_TO_GROUND属性，实现billboard贴地形。



```js
	let pinBuilder = new Cesium.PinBuilder();
    let pinURI = pinBuilder.fromText("展示的内容", Cesium.Color.GREEN, 48).toDataURL();
    this.GLOBAL.viewer.entities.add({
      name: name,
      position: Cesium.Cartesian3.fromDegrees(lng, lat),
      billboard: {
        image: pinURI,
        width: 64,
        height: 64,
        heightReference: Cesium.HeightReference.CLAMP_TO_GROUND //广告牌贴地形
      }
    });
```


图一 官网示例的各种样式

![](https://img-blog.csdnimg.cn/img_convert/3d5febef80be0aeb0eab37bf4bec7ecf.png#pic_center)