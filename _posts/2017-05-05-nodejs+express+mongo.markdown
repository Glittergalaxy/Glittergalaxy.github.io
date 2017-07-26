---
layout: post
title:  "nodejs+express+mongo构建项目"
crawlertitle: "2017-05-05-nodejs+express"
summary: "项目全栈"
date:   20170505
categories: posts
tags: 'nodejs'
author: yunlei.ke
---
利用nodejs + express框架 + mongo数据库搭建后台环境进行前后台交互

[参考文档1](https://my.oschina.net/chenhao901007/blog/312367)
[参考文档2](http://cnodejs.org/topic/547293caa3e2aee40698df0b)
### 1.安装文件
#### 1） 安装并运行mongo
- 官网下载msi安装包，点击下一步直至安装完成。
- 安装服务：
```
安装完成后，一管理员身份运行cmd，cd到mongodb安装目录下的bin目录，然后输入：
mongod --dbpath="mongodb安装目录\data" --logpath="mongodb安装目录\log\log.txt" --install --serviceName MongoDB --serviceDisplayName MongoDB
此步骤中创建数据库目录同时创建日志文件，并将mongo安装为服务
```

- 另开一个cmd,切换到bin目录，然后输入mongo 启动数据库。
- 启动后，可输入命令行创建数据库等。
```
- show dbs
- use dbname
- db."tablename"
- db."tablename".insert({insertObj})  //插入数据
- db."tablename".find()   //查询数据
```
#### 2） 安装 express + express-generator
- 安装全局 express+express-generator即可
- 创建项目
```
express packagename -e -e 表示支持ejs模板引擎 默认是jaden
npm install 安装依赖项
npm start 启动项目
http://127.0.0.1:3000/在浏览器中查看
```

### 2.创建项目
##### 1）app.js中可修改模板引擎类型
```
app.set('view engine','ejs') 改为
app.set('view engine','html')
再进行注册
app.engine('.html',require('ejs').__express)
```
##### 2）创建login.html,index.html,center.html
- index页面创建链接，可跳转到login页面
- login页面创建form表单，可提交到center页面
- center页面可进行登出，回到index页面

##### 3）连接数据库文件
```
var mongoose = require('mongoose');
var db = mongoose.createConnection('mongodb:localhost/hello');//localhost后跟之前创建的数据库名，此处不可使用connect,
var Shema = mongoose.Schema;
var userSchema = new Schema({
    name:String,
    age:String
})//创建新的模型
exports.user = db.model('user',userSchema);//与user表关联

```
##### 4）路由配置
```
var express = require('express');
var router = express.Router();
var user = require('../models/user').user;

router.get('/',function(req,res){
    res.render('index',{title:'index'});//制定跳转后页面且指定其title
})

router.get('/login',function(req,res){
    res.render('login',{title:'login'});//制定跳转后页面且指定其title
})

router.get('/logout',function(req,res){
    res.render('logout',{title:'logout'});//制定跳转后页面且指定其title
})

//login的form表单提交为post方式
router.post('/homepage',function(req,res){
    var queryinfo = {name:req.body.name,age:req.body.age};
    (function(){
        //查询数据库中符合查询信息的数量
        user.count(queryinfo,function(err,doc){
            if(doc == 1){
                console.log(queryinfo.name + ":login success in " + new Date())
                res.render('homepage',{title:"homepage"});
                //若为1则成功跳转到主页并提示信息
            }else{
                console.log(queryinfo.name + ":failed success in " + new Date())
                res.redirect('/')
                //否则返回首页
            }
        })
    })(queryinfo);
})

modules.exports = router;

```
### 3.调试工具
node-inspector
```
全局安装
npm i -g node-inspector
安装完成后进入项目目录
node --debug ./bin/www
即可在命令行查看错误信息
```


