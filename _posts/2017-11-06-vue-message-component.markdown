---
layout: post
title:  "vue消息组件"
crawlertitle: "2017-11-06-vue-note"
summary: "vue组件"
date:   20171106
categories: posts
tags: 'vue component'
author: yunlei.ke
---
vue实现消息组件

#### 1. 组件之间传递信息的方法

- vuex 所有组件之间都可以传递
- props 父子组件传递信息
- $parent 父子组件传递信息

#### 2. 基于props书写组件
    - 父组件

    <message :type="msg.type" :show="msg.show" :msg="msg.msg"  @visible-change="modalVisibleChange"></message>
    //绑定方法
    export default {
        data(){
            return {
                msg:{
                    type:'loading',
                    show:true,
                    msg:'加载中'
                }
            }
        },
        methods:{
            //set message box show/type/content/showtime
            modalVisibleChange(val,msg,type,time){
                this.msg.show = val;
                if(msg){
                    this.msg.msg = msg;
                }
                if(type){
                    this.msg.type = type;
                }
                if(time){
                    setTimeout(() => {
                        this.msg.show = false;
                    },time);
                }
            }
        }
    }

    - 子组件

    <template>
        <div v-if="show" class="message" transition="fade">
            <div class="icons-box tcenter">
                <img src="../assets/success.png" v-if="type=='success'">
                <img src="../assets/success.png" v-if="type=='fail'">
                <img src="../assets/error.png" v-if="type=='error'">
                <img src="../assets/loading.png" v-if="type=='loading'" class="loading">
            </div>
            <div class="tcenter">{{msg}}</div>
        </div>
    </template>

    export default {
        name:'message',
        props:['type','msg','show'],
        watch:{
            show:function(val){
                if(val && this.type!='loading'){
                    setTimeout(() =>{
                        this.$emit("visible-change",false);
                    },3000)
                }
            }
        }
    }


#### 3. 原理
- 子组件通过props获取父组件的属性，通过$emit方法触发父组件定义的方法，父组件在响应方法中修改属性的show/type等的值。
- 当父组件需要显示消息组件时，将show/type/msg等属性内容传递给子组件，当用户手动点击关闭子组件时，子组件利用$emit触发父组件对应的方法修改响应的show/type/msg属性并通过props传递给子组件。





