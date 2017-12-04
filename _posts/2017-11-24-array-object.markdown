---
layout: post
title:  "数组以及对象方法总结"
crawlertitle: "2017-11-24-array-object"
summary: "数组以及对象方法总结"
date:   20171124
categories: posts
tags: 'array'
author: yunlei.ke
---
数组以及对象的常用方法
#### 一、 数组
1. Array.prototype.forEach() 按升序为数组中每个元素执行一次提供的函数。  
```
arr.forEach(callback(currentValue,index,array){
    //函数接收三个参数，thisArg可选，当执行时用作this的值    
}，thisArg)
```
notice:无法中止或跳出forEach循环，除非抛出一个异常，不可链式调用。如果使用箭头函数表达式传入函数参数，thisArg会被忽略，因为箭头函数在词法上版定了this值。
2. Array.prototype.map()  创建一个新数组，其结果是该数组中每个元素都调用一个提供的函数后的结果。  
```
let numbers = [1,5,10,15];
let doubles = numbers.map(x => x**2);
//doubles is now [1,25,100,225]
let new_array = arr.map(function callback(currentValue,index,array){
    return element for new_array;
},thisArg)
```
3. Array.prototype.filter() 创建一个新数组，其包含通过所提供函数实现的测试的所有元素。
```
function isBigEnough(value){
    return value >= 10;
}
var filtered = [12,5,3,129,44].filter(isBigEnough);
//filterd is [12,129,44];
var new_array = arr.filter(callback(currentValue,index,array),thisArg);
```
4. Array.prototype.includes() 用来判断一个数组是否包含一个指定的值，返回true或false  
```
arr.includes(searchElement,fromIndex)
```
5. Array,prototype.some() | Array,prototype.every() | Array.prototype.find() 测试数组中的某些元素是否通过提供的函数的测试,返回bool 
```
const isBigEnough = (element,index,array) => {
    return element > 10;
}
arr.some(callback,thisArg)
var passed = [12,5,8,1,4].some(isBigEnough);
// passed is true
var passed = [12,5,8,1,4].every(isBigEnough);
// passed is false
如果找到一个符合条件的值，some返回true。
如果找到一个不符合条件的值，every返回false。
如果找到一个符合条件的值，find返回该值。
如果找到一个符合条件的值，findIndex返回该索引。
```
6. Array,prototype.reduce() 对累加器和数组中的元素，从左至右应用一个函数  
```
arr.reduce(callback,initialValue);
//四个参数，第一个参数表示列加气累加回调的返回值，他是上一次调用回调时返回的累计值，或initialValue
callback(accumulator,currentValue,currentIndex,array);
initialValue用作第一个调用callback的accumulator的值，如果没有提供初始值，则将使用第一个元素，在没有初始值的空数组上调用reduce将报错。
```
7. Array.prototype.copyWithin()  复制数组的一部分到同数组的另一位置  
```
arr.copyyWithin(target,start,end);
target表示索引，复制序列到该位置，如果为负数，从末尾开始计算，大于等于arr.length,将不会发生拷贝
如果end被忽略，copyWithin将会复制到arr.length;
```
8. Array.prototype.toLocaleString()  数组中的元素使用各自的toLocaleString方法转成字符串，这些字符串使用一个特定语言环境的字符串（“，”）隔开  
9. Array.from() 想要转换成数组的为伪数组对象或可迭代对象。 与map不同之处在于可以对伪数组对象或刻碟带对象进行转换。 
```
Array.from(arraylike,mapFn,thisArg)
```
10. Array.observe(arr,callback) 每次arr发生任何变化时，回调函数将被调用，调用参数为所有变化按发生顺序组成的数组。
```
callback
每当数组发生变化时，使用如下参数调用该函数：
- changes
  用于表示变化的对象数组。每个变化对象的属性如下：
    name: 变化的属性名。
    object: 变化后的数组。
    type: 用于表示变化类型的字符串。其取值为"add"、"update"、"delete"或 "splice"之一。
    oldValue: 仅用于"update"和"delete"类型。变化之前的取值。
    index: 仅用于"splice"类型。变化发生所在索引。
    removed: 仅用于"splice"类型。被删除元素组成的数组。
    addedCount: 仅用于"splice"类型。被添加的元素数量。
    
    通过Array方法如 Array.prototype.pop( ) 触发的变化将被报告成"splice"变化，长度不变但索引赋值发生变化的将被报告成"update"变化。
    
    var arr = ['a', 'b', 'c'];

    Array.observe(arr, function(changes) {
      console.log(changes);
    });
    
    arr[1] = 'B';
    // [{type: 'update', object: <arr>, name: '1', oldValue: 'b'}]
    
    arr[3] = 'd';
    // [{type: 'splice', object: <arr>, index: 3, removed: [], addedCount: 1}]
    
    arr.splice(1, 2, 'beta', 'gamma', 'delta');
    // [{type: 'splice', object: <arr>, index: 1, removed: ['B', 'c'], addedCount: 3}]
```
11. Array.of(arguments) 创建一个具有可变数量参数的新数组实例  
Array.of(7) //[7]

#### 二、对象
1. Object.keys 返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和for …… in循环便利该对象时返回的顺序一致（两者的主要区别是for in循环还会枚举其原型链上的属性）  
```
/* Array 对象 */ 
let arr = ["a", "b", "c"];
console.log(Object.keys(arr)); 
// ['0', '1', '2']
/* Object 对象 */ 
let obj = { foo: "bar", baz: 42 }, 
    keys = Object.keys(obj);
// ["foo","baz"]
/* 类数组 对象 */ 
let obj = { 0 : "a", 1 : "b", 2 : "c"};
console.log(Object.keys(obj)); 
// ['0', '1', '2']
// 类数组 对象, 随机 key 排序 
let anObj = { 100: 'a', 2: 'b', 7: 'c' }; 
console.log(Object.keys(anObj)); 
// ['2', '7', '100']
/* getFoo 是个不可枚举的属性 */ 
var my_obj = Object.create(
   {}, 
   { getFoo : { value : function () { return this.foo } } }
);
my_obj.foo = 1;
console.log(Object.keys(my_obj)); 
// ['foo']
```

2. Object.assign(target,……sources) 用于将所有可枚举属性的值从一个或多个源对象复制到目标对象，他将返回目标对象。
```
如果目标对象中的属性具有相同的键，则属性将被源中的属性覆盖。后来的源的属性将类似地覆盖早先的属性。
继承属性和不可枚举属性是不能被拷贝的
var obj = {a:1};
var copy = Object.assign({},obj);
//{a:1}
针对深拷贝，需要使用其他方法，因为Object.assign()拷贝的是属性值，如果源对象的属性值是一个指向对象的引用，他也只拷贝那个引用值。
let obj1 = {a:0,b:{c:0}};
let obj2 = Object.assign({},obj1);
console.log(JSON.stringify(obj2));
obj1.b.c = 3;
console.log(JSON.stringify(obj2));
//{a:0,b:{c:3}}
var o1 = {a:1};
var o2 = {b:2};
var 03 = {c:3};
var obj = Object.assign(o1,o2,o3);
console.log(obj);//{a:1,b:2,c:3}
console.log(o1);//{a:1,b:2,c:3}
```



