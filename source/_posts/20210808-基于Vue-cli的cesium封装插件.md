---
title: 基于Vue-cli的cesium封装插件
date: 2021-08-08 17:30:27
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Cesium.jpeg
tags:
- Vue
- Cesium
categories:
- Cesium
---



====

<!--more-->



市面上的前端框架中，Vue+Cesium 可谓是最佳搭档，一般做 Cesium B 端产品的公司都会使用 Vue，所以后续内容都将基于 Vue

通常情况下，我们要在 Vue 中使用 Cesium，首先要安装 Cesium，然后要在 vue-cli 的 webpack 配置很多东西，对一些有经验的人来说只不过麻烦些，但是对 Cesium 的初学者来说会很痛苦，因为没有使用过，也不知到要怎么配置，只能搜索网上的教程，一步一步踩坑

其实不管是有经验或是初学者，每次写项目重复配置这些东西都很麻烦

`vue-cli-plugin-cesium` 就是为了解决这个问题



github:https://github.com/isboyjc/vue-cli-plugin-cesium