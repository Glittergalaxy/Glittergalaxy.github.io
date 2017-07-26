---
layout: post
title:  "echarts-highcharts"
crawlertitle: "2017-04-28-echarts-highcharts"
summary: "echarts-highcharts"
date:   20170428
categories: posts
tags: 'charts'
author: yunlei.ke
---
1.说明：eCharts和highcharts功能基本相同，相较而言，highcharts图表基础配色较为美观，简单应用可选，但echarts可绘制多样类型的图，使用灵活

2.引用用法差别：


- highcharts地图，中括号中为js地图文件
var world = Highcharts.geojson(Highcharts.maps['custom/world-palestine-highres']);

- highcharts饼状图等图表
$('#lang').highcharts(option);

- echarts图表，第二个选项为样式模板，图表与地图引用方式一样
var errorCol = echarts.init(document.getElementById('errorCol'),'macarons');
errorCol.setOption(option1);

3.详解echarts相关配置项

echarts的option对象中：
- series 属性(值为数组)主要包含相关数据，图表类型以及图表特殊设置等。数组中包含的对象的属性：如data,值为数组，表示有多组数据。
    - type:标识图表类型，主要有 line/scatter/map/pie等等，其中type值为map则可增加mapType属性，标识地图的区域，如值为‘china’表示中国地图，也可缩小至省份等。
    - itemStyle:可设置元素的样式。
- legend 属性表示图表的说明，标注多种类型的数据等。
- title 属性表示图表的标题。
- tooltip 表示鼠标放在某个点上时显示的内容。
- visualMap 属性表示数据筛选器，其中可设置最大最小值，以及位于图标中的位置，是否可进行过滤筛选（calculate属性）。
- dataRange 表示可拖动的数据轴线，可设置显示区间。
- toolbox 表示数据视图、切换图表type、另存为图片的控件。
- symbol 表示点的原件，可设置effect特效，以及symbolsize元件大小（可使用函数表示），

echarts设置的模板如下：
```
chinaMap.setOption({
textStyle:{
    color:'#2f323b'
},
title: {
    text: '省份分布',
    left: 'center',
    textStyle: {  
        fontWeight: 'normal',              //标题颜色  
        color: '#333'  
    },  
},
tooltip: {
    trigger: 'item'
},
legend: {
    orient: 'vertical',
    left: 'left',
    data:['设备数量']
},
visualMap: {
    min: minNum,
    max: maxNum,
    left: 'left',
    top: 'bottom',
    text: ['高','低'],           // 文本，默认为数值文本
    calculable: true
},
toolbox: {
    show: true,
    orient: 'horizontal',
    right: '20px',
    top: 'top',
    feature: {
        dataView: {show: false,readOnly: true},
        restore: {},
        saveAsImage: {}
    }
},
dataRange:{
    show:true,
    color:['#0073d6',"#3D77FF","#3DD8FF","#99FFFF",'#c7e9ff'],
    min:0,
    max:3000,
    calculable:true,
    textStyle:{
        color:'#333'
    }
},
series: [
    {
        name: '设备',
        type: 'map',
        mapType: 'china',
        roam: false,
        label: {
            normal: {
                show: true,
                textStyle: {
                    color: '#333'
                }
            },
            emphasis: {
                show: true
            }
        },
        itemStyle:{
            normal: {
                borderColor: '#eee',
                areaColor: '#c7e9ff'
            },
            emphasis: {
                areaColor: '#a4edba'
            }
        },
        data: dataArray
    }
]
})
```
            
    
    
