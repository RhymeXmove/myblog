---
title: Vue中使用Vue-jsonp请求jsonp数据
date: 2021-08-09 21:28:17
tags:
- Vue
categories:
- Vue
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Vue.jpeg
---



====

<!--more-->

# Vue-jsonp

为了解决axios无法发起jsonp格式的请求问题，使用`vue-jsonp`组件。

## 一、添加依赖

`npm install vue-jsonp -save`



## 二、main.js

在vue cli项目main.js中添加

```js
import { VueJsonp } from 'vue-jsonp' // 网上很多博客引用不加{}，会报错
Vue.use(VueJsonp)
```



## 三、使用

```js
let url = "https://view.inews.qq.com/g2/getOnsInfo?";
let param = {
          name: "disease_h5"
        };
this.$jsonp(url, param).then(res => {
    console.log(res);
});
```

