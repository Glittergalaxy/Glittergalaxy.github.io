---
layout: post
title:  "vue项目相关插件"
crawlertitle: "2017-06-14-vueproject"
summary: "vue项目相关插件"
date:   20170614
categories: posts
tags: 'vue'
author: yunlei.ke
---
vue项目搭建过程中所需插件

**所有插件在项目中使用大前提为** npm install --save 在项目运行时所需用到的插件

##### 1. axios 用于发送http请求

- 引入方式
```
import axios from 'axios'
```
- 所需设置
```
//设置传输数据类型
axios.defaults.headers.post['Content-Type'] = 'application/json;charset=utf-8';
//设置服务器url
axios.defaults.baseURL = serviceUrl;
//设置headers
axios.defaults.headers.common = {
    UserId:Cookies.get('UserId')
}
//配置请求方法，以下为发送文件上传请求示例
export default {
    file: function(url,data){
        return new Promise((resolve,reject) => {
            let form = new FormData() //实例化一个formdata类型
            form.append(file,data.file) //将所传文件放入formdata类型数据中
            axios.post(url,form,{
                method:'post',
                headers:{'Content-Type':'multiple/form-data'} //设置传输文件格式为form-data类型
            })
            .then((response) => {
                resolve(response.data)
            })
            .catch((error) => {
                reject(error)
            })
        })
    }
}
```

##### 2. 关于ES6中promise对象的使用

[阮一峰的说明](http://es6.ruanyifeng.com/?search=promise&x=0&y=0#docs/promise)
  
同jquery中定义的promise对象类似，prototype中有.then()方法和.catch方法
```
- .then()方法可以有两个参数，分别为resolve和reject时的回调方法。
- .catch()方法即为.then(null,reject)的别名，用于指定发生错误时的回调函数，同时.then()方法中制定的回调函数，若异步过程产生错误，也会被catch方法捕获
- 一般来说，不要在then方法里面定义reject状告台的回调函数，总是使用catch方法,因为可以捕获到前面then方法执行的错误。
```

##### 3. js-cookie插件
  
cookie使用有两处 1> http请求头部获取cookie 2>登陆后设置cookie  
- 引入方式 
```
import * as Cookies from 'js-cookie';
```
- 使用方法
```
设置cookie Cookies.set('userid','1234',{expires:1(单位天)})
获取cookie Cookies.get('userid')
```

##### 4. crypto-js插件

- 引入方式
```
import SHA1 from 'crypto-js/sha1'
或
import CryptoJS from 'crypto-js'
```
- 使用方法
```
SHA1(psw).toString()
或
CryptoJS.SHA1(psw).toString()
```

##### 5. bootstrap样式

- 引入方式
```
在main.js中
import './assets/css/bootstrap.min.css'
```

##### 6. element-ui插件及样式

- 引入方式

```
在main.js中
import ElementUI from 'elemnt-ui'
import 'element-ui/lib/theme-default/index.css'

Vue.use(ElementUI)
```
- 使用方式

```
在component中使用elemnt-ui中的元素，如
<el-form></el-form> form表单中带相关校验
<el-table></el-table> table带远程排序、选择某行、添加序号、隐藏内容等功能
```