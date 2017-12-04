---
layout: post
title:  "异步请求方法"
crawlertitle: "2017-11-22-promise-async"
summary: "异步请求方法"
date:   20171122
categories: posts
tags: 'promise'
author: yunlei.ke
---
由vue项目看常用异步请求方法语法以及ES6异步请求方法
#### 一、Promise  
1. Promise对象是一个构造函数，用来生成Promise实例,接受一个函数作为参数，而该函数的两个参数分别是resolve和reject。他们是两个函数，由js引擎提供，不用自己部署。 
```
const promise = new Promise(function(resolve,reject){
    if(){
        resolve(value);
    }else{
        reject(error);
    }
})
```
2. Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
3. 如果调用resolve和reject函数时带有参数，那么他们的参数会被传递给回调函数。
4. resolve函数的参数除了是正常的值以外，还可能是另一个Promise实例,  
```
const p1 = new Promise(function(resolve,reject){
   // ... 
});
const p2 = new Promise(function(resolve,reject){
    // ...
    resolve(p1);
})
```
5. 上面代码中，p1和p2都是Promise实例，但是p2的resolve方法将p1作为参数，即一个异步操作的结果是返回另一个异步操作。
注意，这里p1的状态就会传递给p2，也就是说，p1的状态决定了p2的状态，如果p1的状态是pending,那么p2的回调函数就会等待p1的状态改变。
6. Promise实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。他的作用式微Promise实例添加状态改变时的回调函数。then方法的第一个参数是resolved状态的回调函数，第二个参数是rejected状态的回调函数。then方法返回的是一个新的Promise实例，因此可以采用链式写法，即then方法后面再调用另一个then方法。  
```
getJSON("/post.json").then(function(json){
    return json.post;
}).then(function(post){
    //...
})
```
上面的代码使用then方法，依次指定了两个回调函数，第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。第一个then方法指定的回调函数，返回的是另一个promise对象，这时，第二个then方法指定的回调函数，就会等待这个新的Promise对象状态发生变化再调用相应方法。  
7. catch方法是.then(null,rejection)的别名，用于指定发生错误时的回调函数。一般来说，不要在then方法中定义reject状态的回调函数（即then的第二个参数），总是使用catch方法。理由是catch方法可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch）。因此，建议总是使用catch方法，而不是用then 方法中的第二个参数。  
```
promise.then(function(data){
    // success
}).catch(function(data){
    // error
})
```
#### 二、fetch  
1. 浏览器兼容：firefox 39以上，chrome42以上
2. 原理：基于Promise实现，无论如何，都会返回一个promise实例
3. 当接收到一个代表错误的http状态码时，从fetch()返回的Promise不会被标记为reject,及时状态码是404或500，相反她会将promise状态标记为resolve，仅当网络故障或请求被阻止时，才会标记为reject。
4. 默认情况下,fetch不会从服务端发送或接受任何cookies,如果站点依赖于用户session,则会导致未经认证的请求。（要发送cookies,必须设置credentials选项）  
5. fetch用法以及相关api
```
if (window.fetch) {
    let myInit = {
        credentials: 'include',//设置发送请求时自动发送cookies
        method: type,
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },//对象或包含 ByteString 值的对象字面量。
        mode: "cors",// 可选值为cors/no-cors/same-origin
        cache: "force-cache"//强制缓存
    }

    //将body转为json对象，也可能是Blob,BufferSource,FormData、URLSearchParams或者USVString对象。注意get或者head方法的请求不能包含body信息。
    if (type == 'POST') {
        Object.defineProperty(myInit, 'body', {
            value: JSON.stringify(data)
        })
    }
    
    try {
        //返回response
        const response = await fetch(url, myInit);
        //response对象可以处理多种格式，json()/blob()/arrayBuffer()/formData()/text()
        //response格式处理完后返回的是一个Promise对象，需要.then或者 await后才能使用
        const responseJson = await response.json();
        return responseJson
    } catch (error) {
        throw new Error(error)
    }
}
```
6. get请求示例  
```
//html
fetch('user.html').then(function(response){
    return response.text();
}).then(function(body){
    document.body.innerHTML = body;
})
//图片
var myImage = document.querySelector('img');
fetch('flower.jpg').then(function(response){
    return response.blob();
})
.then(function(myBlob){
    var objectURL = URL.createObjectURL(myBlob);
    myImage.src = objectURL;
})
//JSON
fetch(url)
.then(function(response){
    return response.json();
}).then(function(data){
    console.log(data);
}).catch(function(error){
    console.log("oops,error");
})
```
#### 三、await/async  
1. async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
2. async函数的返回值是promise对象，可以用then方法指定下一步的操作。同时也可以作为await命令的参数。
3. async函数返回的Promise对象，必须等到内部所有的await命令后面的Promise对象执行完，才会发生状态改变。
```
async function timeout(ms){
    await new Promise((resolve) => {
        setTimeout(resolve,ms);
    })
}
async function asyncPrint(value,ms){
    await timeout(ms);
    console.log(value);
}
```




