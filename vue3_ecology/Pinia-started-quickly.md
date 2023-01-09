## [Pinia](https://pinia.web3doc.top/introduction.html)
<img src="../images/vue3/pinia.svg" style="zoom: 50%">

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