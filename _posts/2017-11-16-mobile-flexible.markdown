---
layout: post
title:  "移动端适配方案"
crawlertitle: "2017-11-16-mobile-flexible-note"
summary: "移动端 适配"
date:   20171116
categories: posts
tags: 'mobile flexible'
author: yunlei.ke
---
#### 移动端适配

一、 基于flexible插件的自适应
1. 获取dpr,若已存在则使用当前dpr  
    dpr即 window.devicePixelRatio,dpr返回当前显示设备的物理像素分辨率与CSS像素分辨率的比率。简单来说，这告诉浏览器应该使用多少个屏幕的实际像素来绘制一个CSS像素。  
2. 设置viewport中的scale = 1/dpr  
    <meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scale=no">  
    
3. 根据deviceWidth以及dpr计算并设置rem大小  
    deviceWidth/dpr>540按照540计算，rem = deviceWidth*dpr/10，设计图分成10等分每一等分为rem的单位大小;

二、 基于vw的自适应  
1. vw浏览器支持  
CSS3新加属性，chrome/firefox/IE9+支持。android browser4.4+支持，chrome for android39支持。1vw等于视窗高度的1%。视窗高度指window.innerHeight包含了底部滚动条高度（如果有的话）
2. 其余相关属性  
vh:viewport height
vmin:vw 和vh中较小的那个。  
3. postcss插件  
根据vw计算原理，如果是ipone6,1vw=7.5px。那么我们在写css时只需以vw为单位写样式。利用postcss-px-to-viewport,书写样式时以px为单位，插件会编译自动计算其对应的vw值。相当于浏览器自己提供了一套适应性解决方案，和flexible原理相同，但是由于计算误差，可能会存在问题。


