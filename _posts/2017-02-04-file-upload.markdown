---
layout: post
title:  "file-upload相关"
crawlertitle: "2017-02-04-file-upload"
summary: "file-upload相关"
date:   20170204
categories: posts
tags: 'js'
author: yunlei.ke
---
文件上传相关注意事项
#### 不需要读取文件或图片大小情况
    1. 根据input[type=file]元素的id获取到对应element并确保不为空。
    
    2.该element对应的files属性中存放着已上传的文件内容，由于h5中支持多文件上传，因此只需获取element.files[0];
    
    3.element.files[0]保存着上传文件的多个属性，包括name,value等等，value中包含文件上传的路径，可对其进行判断是否符合类型要求，name即文件名。
    
    4.上传时将element.files[0]包装成对象如下：{'file':fileObj};
    
    5.在上传的ajax中需修改属性Content-Type为undefined，在transformRequest 参数中，new一个 FormData的对象,并将file文件放至其file属性中。

#### 需要验证文件大小情况

    1.与上一种方法相同，先获取到已上传的文件内容并验证其格式以及大小是否符合要求。
    
    2.创建一个reader对象，读取文件内容并规定对应事件触发的方法。
    var reader = new FileReader();
    reader.readAsDataURL(elem.files[0]); 
    reader.onload = function(e) { 
        var image = new Image();
        image.src = e.target.result;
        image.onload = function(){
			if(image.width != 126 || image.height != 78) {
				hideDiv();
				Msg("图片的尺寸要求为126*78", 3000);
			 	return false;
			};
			//上传文件
		}
    };

    //数据读取出错时触发
    reader.onerror = function(e) {
    	alert("文件读取失败");
    	//显示等待条
        hideDiv();

    } 
    //数据读取中断时触发
    reader.onabort = function(e) {
    	alert("文件读取中断");
    	//显示等待条
		hideDiv();
    }

#### 关于上传文件方法等
[参考文档](www.zhangxinxu.com/wordpress/2015/11/html-input-type-file/) 
1. 上传文件元素样式改造
- 将file元素透明度设置为0，覆盖在对应按钮上方，点击时相当于点击file元素,缺点在于尺寸不好规定     
- 经典做法：使用label元素，将其for属性指到对应file元素，并且隐藏该file元素
```
<label for="id"></label>
<input type="file" id="id"/>
```
- 使用按钮或者链接点击事件触发上传
```
<input style="display:none" type="file"  onchange="angular.element(this).scope().uploadFile('testReport')" />
```
另：angular中file元素上传文件方法如上，元素自带的change事件优先于angular的onchange方法。
    
2.  关于file元素详解
- html中的每个file元素，都会对应创建一个FileUpload对象，获取它的方法就是用document.getElementById()或者$().[0]
```
FileUpload对象的属性：
accept ：文件传输的MIME类型列表
accessKey: 访问的快捷键
name : 对象的名称
type : 文件类型
value : 文件路径
size ： 文件大小
```
- 另上传文件时配合ajax进行上传
    1. 对应的数据传输格式限定为：
```
Content-Type: undefined 
```  
上传时浏览器会自动带上content-type:form-data,并附上对应的formboundry分隔符,此为提交表单数据的一个分隔符，不同浏览器对应不同的分隔符。
    2. 在transformRequest 参数中，new一个 FormData的对象,并将file文件放至其file属性中。

#### 前端读取文件并创建文件下载链接
    if(navigator.appVersion.toString().indexOf(".NET")>0){
        window.navigator.msSaveBlob(myFile,"SunnyChuan.xlsx");
    }
    else{
        var a = document.createElement("a");
        a.href =window.URL.createObjectURL(myFile); 
        a.download = "SunnyChuan.xlsx"; 
        a.click(); 
        document.body.appendChild(a);
    }
[微软官方文档](https://technet.microsoft.com/ZH-CN/Library/hh779016.aspx)