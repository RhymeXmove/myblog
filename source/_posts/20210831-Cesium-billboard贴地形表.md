---
title: Cesium billboard贴地形表
date: 2021-08-31 21:46:22
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Cesium.jpeg
tags:
  - Vue
  - Cesium
categories:
  - Cesium
---

==============

<!--more-->



# 获取点击位置高程的方法，cartographic 中包含:
 - `longitude` 经度
 - `latitude` 维度
 - `height` 高程
```javascript
let ray = viewer.camera.getPickRay(movement.position);
let earthPosition = viewer.scene.globe.pick(ray, viewer.scene);
let cartographic = Cesium.Cartographic.fromCartesian(earthPosition);
```
# billboard贴地形表
```javascript
billboard: {
              image: locationMark, 
              heightReference: Cesium.HeightReference.CLAMP_TO_GROUND,
              horizontalOrigin: Cesium.HorizontalOrigin.CENTER, // //相对于对象的原点（注意是原点的位置）的水平位置
              verticalOrigin: Cesium.VerticalOrigin.BOTTOM //相对于对象的原点的垂直位置，BOTTOM时锚点在下，对象在上
}
```

# 结合使用，点击添加图标
```javascript
if(flag){
	viewer.screenSpaceEventHandler.setInputAction(function (movement) {
		let ray = viewer.camera.getPickRay(movement.position);
    	let earthPosition = viewer.scene.globe.pick(ray, viewer.scene);
    	let cartographic = Cesium.Cartographic.fromCartesian(earthPosition);
    	let billboard = new Cesium.Entity({
    		name: "Draw-Handler-point",
        	position: Cesium.Cartesian3.fromDegrees(
           		Cesium.Math.toDegrees(cartographic.longitude),
           		Cesium.Math.toDegrees(cartographic.latitude),
           		cartographic.height),
        	billboard: {
        		image: location4, // default: undefined
        		width: 30, // default: undefined
        		height: 40, // default: undefined
       }
    });
    viewer.entities.add(billboard);
    }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
	}else{
    	viewer.screenSpaceEventHandler.removeInputAction(
    	Cesium.ScreenSpaceEventType.LEFT_CLICK); //移除事件
    	}
```