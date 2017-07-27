---
layout: post
title:  "angular1表单校验"
crawlertitle: "2017-05-02-angular1-form-validate"
summary: "angular1表单校验"
date:   20170502
categories: posts
tags: 'angular'
author: yunlei.ke
---
1. input输入框
    
    ng-model 进行双向绑定即可
2. select框 
   
     结合ng-options 和 ng-model进行绑定

3. input type = radio 单选框 
    
    value不同，name相同,ng-model绑定同一个值，该值会自动对应相应的value

4. input type = checkbox 复选框
   
    常见的是定义一个对象数组，对象中存放status属性和值属性，提交时遍历并组合相关值,获取时仍需遍历赋值相关的status,checkbox中的 ng-model 绑定相应对象的 status 属性。

5. img input type = file

    img ng-src中默认赋值一个上传图片的icon的url,上传后改变该url的值,
    ==图片是否上传需进行手动校验==。

---

    
6.关于表单校验
    
    form元素：
        name属性必须有，ng-submit为提交时调用的方法;
    
    需结合type=submit按钮：
        ng-disabled = name.$invalid;

    input type=text name=text:
        常用required；
        minlength/maxlength;
        配合help-block:
            ng-show = name.text.$dirty && name.text.$invalid;
            进行输入校验
        
    input type = url/email等特殊格式:
        配合help-block:
            ng-show = name.text.$dirty && name.text.$invalid;
            进行输入校验
            
    input type = text,手机号输入
        ng-pattern = /^1[3|5|7|8][0-9]{9}$/;
        配合help-block:
            ng-show = name.text.$dirty && name.text.$invalid;
            进行输入校验