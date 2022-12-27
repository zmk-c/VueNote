# Vue3生态快速上手

vue3生态主要包含：新一代构建打包vite、vue3新语法、状态管理工具pinia、路由vue-router

vue3生态采用了大量的typescript语法，如果想学习，可以移步至👉[TypeScript快速上手](./TypeScript%E5%BF%AB%E9%80%9F%E4%B8%8A%E6%89%8B.md)

## [Vite](https://cn.vitejs.dev/guide/)
<img src="./images/vue3/vite.png" style="zoom: 30%;">

### 总览
Vite（法语意为 "快速的"，发音 /vit/，发音同 "veet"）是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：

- 一个开发服务器，它基于 原生 ES 模块 提供了 丰富的内建功能，如速度快到惊人的 模块热更新（HMR）。

- 一套构建指令，它使用 Rollup 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

Vite 意在提供开箱即用的配置，同时它的 插件 API 和 JavaScript API 带来了高度的可扩展性，并有完整的类型支持。

> ⚠️兼容性注意：
Vite 需要 Node.js 版本 14.18+，16+。然而，有些模板需要依赖更高的 Node 版本才能正常运行，当你的包管理器发出警告时，请注意升级你的 Node 版本。

### 配置环境
1. 构建vue3 vite构建（简洁）：`npm init vite@latest`
示例：
<img src="./images/vue3/vite构建项目.png" style="zoom:50%;display:block;" />

2. 构建vue3 vue-cli脚手架构建（专门为vue项目构建、全一点）：`npm init vue@latest`
示例：
<img src="./images/vue3/vue-cli脚手架构建.png" style="zoom:50%;display:block" />

### SFC 语法规范
*.vue 件都由三种类型的顶层语法块所组成：`<template>`、`<script>`、`<style>`

**`<template>`**
- 每个 *.vue 文件最多可同时包含一个顶层 `<template>` 块
- 其中的内容会被提取出来并传递给 @vue/compiler-dom，预编译为 JavaScript 的渲染函数，并附属到导出的组件上作为其 render 选项

**`<script>`**
- 每一个 *.vue 文件可以有多个 `<script>` 块 (不包括`<script setup>`)
- 该脚本将作为 ES Module 来执行
- 其默认导出的内容应该是 Vue 组件选项对象，它要么是一个普通的对象，要么是 defineComponent 的返回值

**`<script setup>`**
- 每个 *.vue 文件最多只能有一个 `<script setup>` 块 (不包括常规的 `<script>`)
- 该脚本会被预处理并作为组件的 setup() 函数使用，也就是说它会在每个组件实例中执行。`<script setup>` 的顶层绑定会自动暴露给模板。更多详情请查看 `<script setup>` 文档

**`<style>`**
- 一个 *.vue 文件可以包含多个 `<style>` 标签。
- `<style>` 标签可以通过 scoped 或 module attribute (更多详情请查看 SFC 样式特性) 将样式封装在当前组件内。多个不同封装模式的 `<style>` 标签可以在同一个组件中混

## [Vue3](https://cn.vuejs.org/guide/introduction.html)
<img src="./images/vue3/vue3.png" style="zoom: 30%">

### npm run dev 详解
在我们执行这个命令的时候会去找 package.json 的 scripts 然后执行对应的 dev 命令，在我们执行 npm run xxx，npm 会通过**软连接**查找这个软连接存在于源码目录 node_modules/vite，所以 npm run xxx 的时候，就会到 node_modules/bin 中找对应的映射文件，然后再找到相应的js文件来执行。
<img src="./images/vue3/run dev详解1.png" style="zoom: 25%; display:block">
<img src="./images/vue3/run dev详解2.png" style="zoom: 30%;display:block">
    1. 查找规则是先从当前项目的node_modlue/bin去找
    2. 找不到去全局的node_module/bin去找
    3. 再找不到 去环境变量去找

<strong style="color: red">⚠️注意：我去 node_modules/bin 找到vite.js文件后，没有可执行的文件，然后进行全局搜索发现vite.js命令在package.json文件中找到了，不知道是不是这个？</strong>
- <div style="display: flex">
    <img src="./images/vue3/run dev详解3.png" style="zoom: 25%; display:block; margin-right: 30px">
    <img src="./images/vue3/run dev详解3.png" style="zoom: 25%; display:block">
  </div>

### 模板语法&vue指令
v- 开头都是vue 的指令
```text
v-text 用来显示文本

v-html 用来展示富文本

v-if 用来控制元素的显示隐藏（切换真假DOM）

v-show 用来控制元素的显示隐藏（display: none/block Css切换）

v-else-if 表示 v-if 的“else if 块”。可以链式调用

v-else v-if条件收尾语句

v-on 简写@ 用来给元素添加事件 比如添加点击事件 v-on:click = @click

v-bind 简写:  用来绑定元素的属性Attr

v-model 双向绑定

v-for 用来遍历元素

v-on修饰符 冒泡案例 @click.stop
```

### 认识Ref全家桶
ref也可以读取dom属性

### 认识Reactive全家桶
ref 支持所以类型，而reactive支持引用类型 Array Object Map Set
ref 取值、赋值都需要加.value，而reactive 不需要加.value

### 认识to系列全家桶
toRef 只能修改响应式对象的值 非响应式视图毫无变化
toRefs 可以帮我们批量创建ref对象主要是方便我们解构使用
toRaw 将响应式对象转化为普通对象


## [Pinia](https://pinia.web3doc.top/introduction.html)
<img src="./images/vue3/pinia.svg" style="zoom: 50%">

### 总览
Pinia.js是vue3中的全局状态管理工具，用于替代vue三剑客里的vuex，有如下特点：
- 完整的 ts 的支持；
- 足够轻量，压缩后的体积只有1kb左右;
- 去除 mutations，只有 state，getters，actions；
- actions 支持同步和异步；
- 代码扁平化没有模块嵌套，只有 store 的概念，store 之间可以自由使用，每一个store都是独立的
- 无需手动添加 store，store 一旦创建便会自动添加；
- 支持Vue3 和 Vue2

### 起步安装
```shell
npm install pinia
```
### 引入注册vue3
```ts
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia' // 引入pinia
 
const store = createPinia() // 注册pinia
let app = createApp(App)
 
app.use(store)
 
app.mount('#app')
```

### 初始化仓库Store
1. 在src目录下新建一个store文件夹
2. 新建index.ts
3. 定义仓库Store
我们需要知道 Store 是使用 defineStore() 定义的，并且它需要一个唯一名称，作为第一个参数传递
```ts
import { defineStore } from 'pinia'

export const useStore = defineStore('main', {
  // other options...
})

以下这种写法也可以

const useStore = defineStore({
  id: 'main', // 唯一标识
  ...
})
export default useStore;
```
这个 name，也称为 id，是必要的，Pinia 使用它来将 store 连接到 devtools。 将返回的函数命名为 use... 是跨可组合项的约定，以使其符合你的使用习惯。

4. 定义值
State 箭头函数 返回一个对象 在对象里面定义值
```ts
import { defineStore } from "pinia";

const useStore = defineStore({
  id: 'test', // 唯一标识
  state: () => {
    return {
      current: 100,
      name: '无忧'
    }
  },

  // computed 修饰一些值
  getters: {

  },

  // methods 可以做同步 异步都可以做 提交state
  actions: {

  }
})

export default useStore;
```

### State
state 是 store 的核心部分。 我们通常从定义应用程序的状态开始。 在 Pinia 中，状态被定义为返回初始状态的函数。

1. State 是允许直接修改值的 例如current++
2. 批量修改State的值`$patch`方法 
3. 批量修改函数形式(传入参数)
推荐使用函数形式 可以自定义修改逻辑
```ts
<script setup lang="ts">
import useStore from './store'

const Test = useStore()

const change = () => {
  // 1. State 是允许直接修改值的 例如Test.current++
  // Test.current++ 

  // 2. 批量修改State的值 $patch方法
  // Test.$patch({
  //   current: 888,
  //   name: '小宝'
  // })

  // 3. 推荐使用函数形式 可以自定义修改逻辑
  Test.$patch((state) => {
    state.current = 111
    state.name = '大白'
  })
}
</script>

<template>
  <div>
    pinia: {{ Test.current }} -- {{ Test.name }}
    <button @click="change">点我+1</button>
  </div>
</template>
```
4. 通过actions修改
定义actions，在actions 中直接使用this就可以指到state里面的值

```ts
/** /src/store/index.ts */
import { defineStore } from "pinia";

const useStore = defineStore({
  id: 'test', // 唯一标识
  state: () => {
    return {
      current: 100,
      name: '无忧'
    }
  },
  // methods 可以做同步 异步都可以做 提交state
  actions: {
    setCurrent() {
      this.current = 999
    }
  }
})

export default useStore;
```
直接调用即可
```ts
<script setup lang="ts">
import useStore from './store'

const Test = useStore()

const change = () => {
  // 4. 通过actions修改 这里直接调用方法
  Test.setCurrent()
}
</script>

<template>
  <div>
    pinia: {{ Test.current }} -- {{ Test.name }}
    <button @click="change">点我+1</button>
  </div>
</template>
```

### Actions，getters
#### Actions（支持同步异步）
1. 同步 直接调用即可

```ts
/** src/store/index.ts */
import { defineStore } from "pinia";

type User = {
  name: string,
  age: number
}

let result: User = {
  name: '小白',
  age: 23
}

const useStore = defineStore({
  id: 'test', // 唯一标识
  state: () => {
    return {
      user: <User>{},
      name: '',
    }
  },
  // methods 可以做同步 异步都可以做 提交state
  actions: {
    setUser() {
      this.user = result
    }
  }
})

export default useStore;
```
直接调用actions中的setUser方法
```ts
<script setup lang="ts">
import useStore from './store'

const Test = useStore()

const change = () => {
  Test.setUser()
}
</script>

<template>
  <div>
    <p>actions-user: {{ Test.user }}</p>
    <hr />
    <p>actions-name: {{ Test.name }}</p>
    <hr />
    <button @click="change">change</button>
  </div>
</template>
```

2. 异步 可以结合async await 修饰
```ts
/** src/store/index.ts */
import { defineStore } from "pinia";

type User = {
  name: string,
  age: number
}

// 异步写法
const Login = (): Promise<User> => {
  return new Promise((resolve) => {
    setTimeout(() => {

      resolve({
        name: '小白',
        age: 23
      })
    }, 2000)
  })
}

const useStore = defineStore({
  id: 'test', // 唯一标识
  state: () => {
    return {
      user: <User>{},
      name: '无名',
    }
  },

  // computed 修饰一些值
  getters: {

  },

  // methods 可以做同步 异步都可以做 提交state
  actions: {
    async setUser() {
      const result = await Login()
      this.user = result

      this.setName('大白') // 互相调用
    },
    setName(name: string) { 
      this.name = name;
    }
  }
})

export default useStore;
```
直接调用，注意多个actions中的方法互相调用setUser、setName

#### getters
1. 使用箭头函数不能使用this，this指向已经改变指向undefined 修改值请用state
主要作用类似于computed 数据修饰并且有缓存

```ts
  getters:{
      newName:(state)=>  `$${state.user.name}`
  },
```
2. 普通函数形式可以使用this

```ts
  getters:{
      newAge ():number {
          return this.user.age
      }
  },
```
3. getters 互相调用

```ts
/** /src/store/index.ts */
import { defineStore } from "pinia";

type User = {
  name: string,
  age: number
}

// 异步写法
const Login = (): Promise<User> => {
  return new Promise((resolve) => {
    setTimeout(() => {

      resolve({
        name: '小白',
        age: 23
      })
    }, 2000)
  })
}

const useStore = defineStore({
  id: 'test', // 唯一标识
  state: () => {
    return {
      user: <User>{},
      name: '无名',
    }
  },

  // computed 修饰一些值
  getters: {
    newName(): string {
      return `${this.name} - ${this.getUserAge}`
    },
    getUserAge(): string | number {
      return this.user.age
    }
  },

  // methods 可以做同步 异步都可以做 提交state
  actions: {
    async setUser() {
      const result = await Login()
      this.user = result

      this.setName('大白')
    },
    setName(name: string) {
      this.name = name;
    }
  }
})

export default useStore;
```
```ts
<script setup lang="ts">
import useStore from './store'

const Test = useStore()

const change = () => {
  Test.setUser()
  Test.newName
}
</script>

<template>
  <div>
    <p>actions-user: {{ Test.user }}</p>
    <hr />
    <p>actions-name: {{ Test.name }}</p>
    <hr />
    <p>getters: {{ Test.newName }}</p>
    <button @click="change">change</button>
  </div>
</template>
```

## [vue-router](https://router.vuejs.org/zh/introduction.html)
<img src="./images/vue3/vue-router.png" style="zoom: 30%">

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
