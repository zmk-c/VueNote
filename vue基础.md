# 前端Vue学习
**学习Vue之前要掌握的JavaScript基础知识**
- ES6语法规范
- ES6模块化
- 包管理器
- 原型，原型链
- 数组常用方法
- promise
- axios
- ...

**npm常用命令（安装自行百度）**
```bash
# 如果速度慢可以使用淘宝镜像 cnpm 首先安装cnpm)
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install vue-cli -g
# 或者配置源为淘宝仓库
npm config get registry  // 查看npm当前镜像源
npm config set registry https://registry.npm.taobao.org

# 扩展
Vue cli = Vue + 一堆的js插件。
SpringCloud = SpringBoot + 一堆第三方组件。

# 查看npm相关安装
zmk@zmk ~ % npm ls -g --depth=0
/opt/homebrew/lib
├── @vue/cli@5.0.8
├── cnpm@8.3.0
├── npm@8.16.0
├── webpack@5.74.0
└── webpack-cli@4.10.0

# npm查看安装插件的所有版本（以webpack为例）
npm view webpack versions
```

**yarn和npm**

npm 是按照队列执行每个package，也就是说必须要等到当前package 安装完成之后，才能继续后面的安装。 而yarn 是同步执行所有任务，提高了性能。
![20220809145656](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220809145656.png)

## Vue2学习
### 第一章Vue核心
**1.1 Vue的特点**：
1. 采用<strong style="color:#DD5145">组件化</strong>模式，提高代码复用率且让代码更好维护
2. <strong style="color:#DD5145">声明式</strong>编码，让编码人员无需直接操作DOM，提高开发效率
![20220902002331](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220902002331.png)

**1.2 模板语法**

Vue模板语法有2大类：
1. **插值语法**：
    功能：用于解析标签体内容
    写法：{{xxx}}, 其中xxx是js表达式，且可以直接读取到data中的所有属性（不用this指向）
2. **指令语法**：
    功能：用于解析标签（包括：标签属性，标签体内容，绑定事件...）
    举例：v-bind:href="xxx" 或 简写为 :href="xxx"，其中xxx同样要写js表达式，
    且可以直接读取到data中的所有属性
    备注⚠️：Vue中有很多指令，且形式都是：v-???，此处只是拿v-bind举例


```html
<!-- 举例 -->
<div id = "root">
    <h1>插值语法（用于解析标签体内容）</h1>
    <h3>你好, {{name}}</h3>
    <hr>
    <h1>指令语法（用于解析标签，包括标签属性 标签体内容 绑定事件）</h1>
    <!-- v-bind:href  => :href  v-bind绑定 将url当成 js表达式 给标签里任何一个属性动态绑定值 -->
    <a :href="school.url">点击去{{school.name}}官网</a> 
    <br>
    <a :href="school.url.toUpperCase()">点击去尚硅谷官网</a> 
    <br>
    <a :href="Date.now()">点击去尚硅谷官网</a> 
    <hr>
</div>
```

**1.3 数据绑定**

Vue中有两种数据绑定的方式：
1. **单向绑定（v-bind）**: 数据只能从data留下页面
2. **双向绑定（v-model）**: 数据不仅能从data留下页面，还可以从页面留下data

备注⚠️：
1. 双向绑定v-model一般应用在表单元素上（如input select等）
2. v-model:value 可以简写为v-model，因为v-model默认收集的就是value值


`v-bind`属于单向绑定
![20220903093751](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903093751.png)

`v-model`属于双向绑定
![20220903094326](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903094326.png)

```html
 <!-- 如下代码是错误的，因为v-model只能应用在表单类元素（输入类元素）上 -->
<h2 v-model:x="name">你好啊</h2>
```

**1.4 el与data的两种写法**

data与el的两种写法
1. el有两种写法
    （1）new Vue的时候配置el属性
    （2）先创建vue实例，随后再通过vm.$mount('#xxx')指定el的值

```js
举例：

// el的两种写法
const vm = new Vue({
    el: '#root',  // 第一种写法
    data: {
        name: '尚硅谷'
    }
})

vm.$mount('#root') // 第二种写法 比较灵活 例如 定时2秒在执行 实例
setTimeout(() => {
    vm.$mount('#root') // 比较灵活
},2000)
```

2. data有两种写法
    （1）对象式
    （2）函数式
    如何选择？ 目前学习时哪种写法都可以，以后学到组件时，data必须使用函数式，否则会报错

```js
举例：

// data的两种写法
new Vue({
    el: '#root',
    data: {     // data的第一种写法：对象式
        name: '尚硅谷' 
    }

    data() {    // data的第二种写法：函数式  后面组件化 都采用这种
        console.log('@@@@',this) // 此处的this是Vue实例对象 但是注意这里的data函数不能写成箭头函数 因为箭头函数没有自己的this 向外查找就是window对象
        return {
            name: '尚硅谷'
        }
    }
})
```

3. 一个重要原则！！！⚠️
    <strong style="color:#DD5145">由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了</strong>


**1.5 MVVM模型**
MVVM模型
1. M: 模型（Model）, new Vue实例data中的数据
2. V: 视图（View）, 模板代码
3. VM: 视图模型（ViewModel）, Vue实例

观察发现
1. data中的所有属性最后都出现在了vm实例上
2. vm身上所有的属性及Vue原型所有属性，在Vue模板中都可以直接使用

![20221012175844](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20221012175844.png)

**1.6 数据代理**
1. Vue中的数据代理：
    通过 vm对象 来 代理data对象 中属性的操作（读/写）
2. Vue中数据代理的好处：
    更加方便的操作data中的数据
3. 基本原理：
    <strong style="color:#DD5145">通过Object.defineProperty()把data对象中所有属性添加到vm上</strong>
    为每一个添加到vm上的属性，都指定一个getter/setter
    在getter/setter内部去操作（读/写）data中对应的属性

![20220903114101](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903114101.png)

```js
vue2数据代理原理Object.defineProperty举例：

let person = {
    name: '章三',
    sex: '男',
}

// Object.defineProperty 定义新属性 或 修改原有的属性  默认定义的属性不可枚举
Object.defineProperty(person,'age',{
    value: 18,
})

Object.defineProperty(person,'age',{
    value: 18,
    enumerable: true, // 控制属性是否可枚举 默认false
    writable: true, // 控制属性是否可以被修改 默认false
    configurable:true, // 控制属性是否可以被删除 默认false

    // 当有人读取person的age属性时 get函数(getter)就会被调用 且返回值就是age的值
    get() {
        console.log('有人读取age属性了')
        return number
    },

    // 当有人修改person的age属性时 set函数(setter)就会被调用 且会收到修改的具体值
    set(value) {
        console.log('有人修改age属性值了，且值是 ' + value)
        number = value;
    }
})
```

vm._data中的数据做了一番小小的升级 为了做响应式 但是还是包含address和name

**事件处理**
事件的基本使用：
1. 使用v-on:xxx 或 @xxx绑定事件，其中xxx是事件名
2. 事件的回调需要配置在methods对象中，最终会在vm上
3. methods中配置的函数，不要使用箭头函数！否则this指向就不是vm了
4. methods中配置的函数，都是被Vue所管理的，this的指向是**vm或组件实例对象**
5. @click="demo"和@click="demo($event)"效果一致，但后者可以传参

```js
const vm = new Vue({
    el: '#root',
    data() {  // 只有配置在data中的数据 才会做数据劫持/数据代理
        return {
            name: '尚硅谷',
        }
    },
    data: { // 其实方法也可以写在data中，但是这样会让vue很累  做了一遍没有意义的数据代理 
        name: '尚硅谷',
        /* showInfo2(event,param) {
            console.log(event,param)
        } */
    },
    methods: {
        showInfo1(event) {
            // console.log(this)  // 此处的this是vm  改成箭头函数this指向为window
            alert('欢迎你 新同学1')
        },
        // showInfo2(event,param) {
        //     console.log(event,param)
        // }
    }
})
```

![20220903163643](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903163643.png)

键盘事件
![20220903171258](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903171258.png)


**计算属性**
![20220903181148](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903181148.png)


**监视属性**
![20220904162605](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904162605.png)

![20220904164251](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904164251.png)


**绑定样式**
![20220904173024](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904173024.png)


**条件渲染**
![20220904200045](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904200045.png)

v-if 可以移除添加节点 操作DOM 而v-show未移出节点 仅仅是将节点隐藏

template仅可与v-if使用 不能与v-show使用


**列表渲染**
![20220904202209](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904202209.png)

`key`详解 虚拟DOM对比算法(Diff算法)  打乱顺序了

以index所以作为key 如果不写key属性 那么vue默认使用遍历的index索引作为key
![20220904203751](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904203751.png)

![20220904203819](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904203819.png)

以唯一标识作为key  效率高 增加了复用 
![20220904204226](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904204226.png)

![20220904204438](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220904204438.png)


**收集表单数据**
![20220905151842](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905151842.png)


**内置指令**
![20220905153020](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905153020.png)


**自定义指令**
![20220905164955](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905164955.png)


**生命周期**
![20220905173934](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905173934.png)

![20220905174958](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905174958.png)

![20220905185812](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905185812.png)

### 第二章 Vue组件化编程

**对组件的理解**
![20220905192045](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905192045.png)

`封装`
![20220905192511](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905192511.png)

组件的定义：实现应用中`局部`功能`代码`和`资源`的集合
![20220905192640](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905192640.png)


**非单文件组件**
非单文件组件：一个文件中包含有n个组件
![20220905201323](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905201323.png)

![20220905202617](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220905202617.png)

关于VueComponent
![20220906091648](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220906091648.png)

一个重要的内置关系
![20220906105845](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220906105845.png)

![20220906112213](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220906112213.png)

**单文件组件**
单文件组件：一个文件中只包含有1个组件
![20220906143417](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220906143417.png)

### 第三章 使用Vue脚手架
Vue脚手架是Vue官方提高的标准化开发工具（开发平台）
```bash
# 全局安装 vue 脚手架
npm install -g @vue/cli

# 创建一个新项目
vue create 项目名称

# 启动项目
npm run serve
```
![20220906214247](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220906214247.png)

**备注**
1. Vue脚手架隐藏了所有webpack相关的配置，若想查看具体的webpack配置，请执行：`vue inspect > output.js`


### 第四章 Vue中的ajax


### 第五章 vuex


### 第六章 vue-router
vue router 是vue.js官方的路由管理器，它和vue.js的核心深度集成，让构建单页面应用（SPA）变得易如反掌，包含的功能有：
- 嵌套的路由/视图表
- 模块化的 基于组件的路由配置
- 路由参数，查询，通配符
- 基于vue.js过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的css class链接
- h5历史模式或hash模式，在IE9中自动降级
- 自定义的滚动条行为

```bash
# 安装vue router
npm install vue-router@4
npm install vue-router --save-dev
```

### 第七章 Vue UI组件库

```bash
# 安装element-ui
npm i element-ui -S  
# 在main.js中引入
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
```