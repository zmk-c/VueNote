## [Vue-Router4.x](https://router.vuejs.org/zh/introduction.html)
<img src="../images/vue3/vue-router.png" style="zoom: 30%">

因为vue是单页应用，不会有那么多html让我们跳转，所有要使用路由做页面的跳转。Vue 路由允许我们通过不同的 URL 访问不同的内容。通过 Vue 可以实现多视图的单页Web应用。

### 安装启动上手
1. 先构建前端项目，看上方vite配置环境那一节，自己搭建一个router，不用脚手架生成的vue项目目录结构，这样会了解一些
```shell
# 注意vue3版本安装对应的router4版本
npm install vue-router@4
```
2. 在src目录下面新建router 文件 然后在router 文件夹下面新建 index.ts

```ts
/** src/router/index.ts */
//引入路由对象
import { createRouter, createWebHashHistory, createWebHistory, RouteRecordRaw } from "vue-router";

// 测试两个路由
const routers: Array<RouteRecordRaw> = [
  {
    path: "/",
    component: () => import('../components/Login.vue'), // 打包时 会进行代码分割 性能优化
  },
  {
    path: "/register",
    component: () => import('../components/Register.vue'), // 打包时 会进行代码分割 性能优化
  }
]

//vue2 mode history | vue3 createWebHistory
//vue2 mode  hash   | vue3  createWebHashHistory
//vue2 mode abstact | vue3  createMemoryHistory // SSR服务端渲染

const router = createRouter({
  history: createWebHashHistory(), 
  // history: createWebHistory(), 
  routes: routers
})

// 导出路由
export default router
```
路由模式：
 - **createWebHashHistory**: hash模式 基于hash实现 路由前面有'#/' 通过window.addEventListener('hashchange',(e)=>{console.log(e)}) 这个函数监听路由变化

 - **createWebHistory**: 基于history实现 路由前面没有'#/' 通过window.addEventListener('popstate',(e)=>{console.log(e)}) 监听路由变化
 
 - **createMemoryHistory** SSR服务端渲染

3. 通过router-view内置标签展示存放路由展示

```ts
<script setup lang="ts">
</script>

<template>
  <div>
    <span>vue3</span>
    <!-- router-view 路由出口 路由匹配到的组件将渲染在这里 -->
    <div>
      <router-link to="/" style="margin-right: 10px">登录</router-link>
      <router-link to="/register">注册</router-link>
    </div>
    <hr>
    <router-view></router-view>

  </div>
</template>

```
与此，还有一个router-link内置标签
请注意，我们没有使用常规的 a 标签，而是使用一个自定义组件 router-link 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

4. 最后在main.ts挂载
运行npm run dev启动项目即可点击查看路由跳转变化

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router';

createApp(App)
  .use(router) // 挂载到main.ts中
  .mount('#app')
```

### 导航守卫
#### 全局前置守卫
**beforeEach**
```ts
router.beforeEach((to, form, next) => {
    console.log(to, form);
    next()
})
```
每个守卫方法接收三个参数
- to: Route， 即将要进入的目标路由对象；
- from: Route，当前导航正要离开的路由；
- next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。

案例 权限判断
```ts
const whileList = ['/']
 
router.beforeEach((to, from, next) => {
    let token = localStorage.getItem('token')
    //白名单 有值 或者登陆过存储了token信息可以跳转 否则就去登录页面
    if (whileList.includes(to.path) || token) {
        next()
    } else {
        next({
            path:'/'
        })
    }
})
```

#### 全局后置守卫
**afterEach**


### 动态路由
我们一般使用动态路由都是后台会返回一个路由表前端通过调接口拿到后处理(后端处理路由，权限设置)

主要使用的方法就是 **router.addRoute**

#### 添加路由
动态路由主要通过两个函数实现：**router.addRoute()** 和 **router.removeRoute()**。它们只注册一个新的路由，也就是说，如果新增加的路由与当前位置相匹配，就需要你用 router.push() 或 router.replace() 来手动导航，才能显示该新路由

```ts
router.addRoute({ path: '/about', component: About })
```
