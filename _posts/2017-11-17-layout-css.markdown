---
layout: post
title:  "左右布局"
crawlertitle: "2017-11-17-layout-css-note"
summary: "左右布局"
date:   20171117
categories: posts
tags: 'css layout'
author: yunlei.ke
---
#### 实现左边固定宽度，右边自适应布局  
```
.wrapper{
    height:800px;
    width:100%;
}
.left{
    width:200px;
}
.right{
    height:100%;
}
```
1. float方法或绝对定位方法
- 左边元素 float:left;或 position:absolute;
- 右边元素 margin-left:200px;  
2. flex布局  
- 父元素 display:flex;
- 右边 flex:1;  
3. table布局  
- 父元素 display:table;
- 左边、右边 display:table-cell;  

4. display:table-cell可实现以下功能  
- 不定高元素垂直居中
- 等高实现
- 左侧定宽，右侧自适应布局