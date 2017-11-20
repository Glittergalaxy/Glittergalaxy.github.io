---
layout: post
title:  "IP地址定位"
crawlertitle: "2017-02-14-iplocate"
summary: "node+express+BMap"
date:   20170214
categories: posts
tags: 'BMap nodejs'
author: yunlei.ke
---
利用前端发送ip数组，node+express接收到后组装url发送请求到百度地图api并返回请求结果数组给前端

#### 前端代码部分（jquery）    
步骤：
-  引用百度地图 
-  实例化百度地图，添加相关缩放控件，设置主图 
-  重写http请求，设置contentType并对请求数据作转化为json对象处理 
-  发送请求数据，将请求结果转化成可处理对象
-  根据响应结果的经纬度进行定位，添加默认覆盖物和iplabel

```
<script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=OxLYdFmu3YS1haMUcaBmGMBK0P7PbOqb"></script>
<body>
	<div>
		<input type="text" id="ipAddress"/>
	</div>
	<div id="allmap"></div>
</body>                                                                                                   
</html>
<script type="text/javascript">
var map = new BMap.Map("allmap");
// var point = new BMap.Point(116.331398,39.897445);
// map.centerAndZoom(point,12);
map.addControl(new BMap.NavigationControl());               // 添加平移缩放控件
map.addControl(new BMap.ScaleControl());                    // 添加比例尺控件
map.addControl(new BMap.OverviewMapControl());              //添加缩略地图控件
map.enableScrollWheelZoom();                            //启用滚轮放大缩小
map.addControl(new BMap.MapTypeControl());          //添加地图类型控件
map.setMapStyle({style:'midnight'});
function http(url,method,dataObj){
	var formdata = JSON.stringify(dataObj);
	//异步操作
	var defer = $.Deferred();   
	//发送
	$.ajax({
		url : url,
		type : method,
		data : formdata,
		contentType:'application/json; charset=utf-8',
		success : function(data){
				defer.resolve(data);//执行状态从"未完成"改为"已完成"
		},
		error : function(data){
		  	defer.reject(data);
		}
	});
	return defer.promise();
}
//根据ip数组获取位置数组
var arr = ['112.18.17.7','112.118.17.7'];
var locationArr = [];

getLocate();

function getLocate(){	
	http('http://127.0.0.1:8081/map','post',arr).then(function(data){
		for (var i = 0; i < data.length; i++) {
			var temp = JSON.parse(data[i]);
			var obj = {
				'ip': arr[i],
				'x': temp.content.point.x,
				'y': temp.content.point.y,
			}
			locationArr.push(obj);
		}	
		//添加标注
		addMarker();
	});
	
}
//根据ip获取位置
// function getLocate(){	
// 	http('http://127.0.0.1:8081/map','post',{'ip':'112.13.41.2'}).then(function(data){
// 		console.log(data);
// 		var obj = {
// 			'ip': '115.3.4.2',
// 			'x': data.content.point.x,
// 			'y': data.content.point.y,
// 		};	
// 		//添加标注
// 		addMarker(obj);
// 	});
// }
// 创建多个标注
function addMarker(){
	for (var i = 0; i < locationArr.length; i ++) {
		var temp = locationArr[i];
		var point = new BMap.Point(temp.x, temp.y);
		map.centerAndZoom(point,8);
		var marker = new BMap.Marker(point);
  		map.addOverlay(marker);
  		var label = new BMap.Label(temp.ip,{offset:new BMap.Size(20,-10)});
		marker.setLabel(label);
	}
  
}	
//创建一个标注
// function addMarker(temp){
// 		console.log(temp.x);
// 		var point = new BMap.Point(temp.x, temp.y);
// 		map.centerAndZoom(point,12);
// 		var marker = new BMap.Marker(point);
//   		map.addOverlay(marker);
//   		var label = new BMap.Label(temp.ip,{offset:new BMap.Size(20,-10)});
// 		marker.setLabel(label);
  
// }	
</script>
```

#### 后端代码（nodejs）
步骤：
- 引入http模块，express模块，body-parser模块 
- 设置允许跨域请求和请求响应的数据类型
- 设置接口请求回调函数
- 请求回调函数中，取到获取的数据并遍历，向百度api发起请求
- 获取到百度的响应数据后push进数组，并对前端请求作出响应

```
var http = require('http');
var express = require('express');
var app = express();  //实例化
var key = 'OxLYdFmu3YS1haMUcaBmGMBK0P7PbOqb'; //百度api的key


var bodyParser = require('body-parser');

// 创建 application/x-www-form-urlencoded 编码解析
// var urlencodedParser = bodyParser.urlencoded({ extended: false })
app.use(bodyParser.json()); //json编码

//设置跨域访问
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With,Content-Type");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

//post请求,url/map
app.post('/map',function(req,res){
	var ipArr = req.body;
	var result = Array();
	for (var i = 0; i < ipArr.length; i++) {
		var tree = '';
		var options = {  
		    hostname: 'api.map.baidu.com',  
		    port: 80,  
		    path: '/location/ip?ak=' + key + "&coor=bd09ll&ip=" + ipArr[i],  
		    method: 'GET'  
		};  
        // 向远程服务器端发送请求
      	var getLocation = http.request(options, function(response){
		   	response.on('data', function(data) {
		      	tree += data;
		      	result.push(tree); 
		      	//避免缓存
		      	tree = ''; 	
		   	}); 
		});   
	    getLocation.end();   
	     
	}

	//延后发送请求响应
	setTimeout(function(){
		res.status(200).send(result);
	}, 500);
	

})

//监听8081接口打印请求域名和端口
var server = app.listen(8081, function () {

  var host = server.address().address
  var port = server.address().port

  console.log("应用实例，访问地址为 http://%s:%s", host, port)

})
```