---
layout: post
title:  "vue项目文档结构"
crawlertitle: "2017-06-16-vue-structure"
summary: "内部解构"
date:   20170616
categories: posts
tags: 'vue'
author: yunlei.ke
---
解构vue2.0项目内部文档结构

```
- packageName
    - build (auto，dev和prod中配置favicon.ico,utils中配置bootstap带的fonts的publicPath)
        - utils.js 
        - webpack.base.conf.js
        - webpack.dev.conf.js
        - webpack.prod.conf.js
    - config (auto,通常需配置路径等参数)
    - dist (npm run build 后生成)
        - static
        - index.html
        - favicon.ico
    - node_modules (依赖项)
    - src (项目源文件)
        - assets (css/fonts/img)
        - components (common Components)
            - topMenu
            - leftMenu
            - pager
            - message
        - service(http请求以及常量映射)
            - http
            - api
            - commonList
        - modules (路由模块)
            - product
            - order
            - user
        - router
        - store
        - App.vue
        - main.js
    - static (静态文件)
        - js (如url配置文件，如serverurl/develop/product/prerelease)
    - favicon.ico
    - index.html
    - package.json
```

#### 1. index.html 书写规则
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>life</title>
  </head>
  <body>
    <div id="app"></div>
  </body>
</html>
包含html完整的文档结构
```
#### 2. App.vue 书写规则
```
与一般的vue组件相同，包含html/js/css
 html部分
<template>
    <div id="app">
        <alert-bar></alert-bar>
        <top-menu v-if="index"></top-menu>
        <router-view></router-view>
    </div>
</template>
通常根元素为index.html中定义的id为app的元素，且包含router-view元素（路由视图）
```
#### 3. main.js书写规则
```
1.引入项目必备
import Vue from 'vue'
import App from './App.vue'
import store from './store'
import router from './router'

2.引入公用插件以及css
import './assets/css/bootstrap.min.css'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'

Vue.use(ElementUI)

3.创建vue实例
new Vue({
    el:'#app',
    store,
    router,
    render: h => h(App)
}).$mount('#app')

```
#### 4. router.js书写规则
```
1.引入必备插件
import Vue from 'vue'
import VueRouter from 'vue-router'

2.引入路由组件
import Work from '../modules/work/work.vue'
import WorkAuditing from '../modules/work/auditing.vue'
import WorkAudited from '../modules/work/audited.vue'
import License from '../modules/work/license.vue'
import Authorize from '../modules/work/authorize.vue'

3.使用方法
Vue.use(VueRouter)
const routes = [
    {path:'/',redirect:'/login'},
    {path:'/order',component:'Order',redirect:'/order/list',children:[
        {path:'order/list',name:'orderList',component:orderList}
    ]}
]
export default new VueRouter({routes})

最后在main.js中注入
```
#### 5. store书写规则 [vuex官方文档](https://vuex.vuejs.org/zh-cn/mutations.html)
```
1.引入插件
import Vue from 'vue'
import Vuex from 'vuex'

2.使用方法
Vue.use(Vuex)
export default new Vuex.Store({
    modules:{
        
    }
})
3. modules中书写
- state
    const state = {
       current:0, 
    }
- getters
    const getters = {
        current:state => state.current
    }
- actions
    const actions = {
        setCurrent({commit},page){
            commit(SET_CURRENT,page)
        }
    }
- mutations
    const mutations = {
        [SET_CURRENT](state,page){
            state.current = page;
        }
    }
```
            
        