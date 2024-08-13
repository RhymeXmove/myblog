---
title: vue中让echarts随屏幕大小变化
date: 2021-08-08 17:12:00
tags:
- vue
- echart
categories:
- Vue
cover: https://cdn.jsdelivr.net/gh/RhymeXmove/blogimg@latest/cover/Vue.jpeg
---

====

<!--more-->

```
let myChart = this.$echarts.init(document.getElementById('myChart'))
```

给echart实体添加监听`resize()`事件

```
window.addEventListener("resize", () => {
        myChart.resize();
});    
```

