# 前端Vue学习
## 前提基础（略）
**npm常用命令**
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
**Vue的特点**：
1. 采用`组件化`模式，提高代码复用率且让代码更好维护
2. `声明式`编码，让编码人员无需直接操作DOM，提高开发效率
![20220902002331](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220902002331.png)

**学习Vue之前要掌握的JavaScript基础知识**
- ES6语法规范
- ES6模块化
- 包管理器
- 原型，原型链
- 数组常用方法
- axios
- promise

**模板语法**
![20220903092805](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903092805.png)


**数据绑定**
`v-bind`属于单向绑定
![20220903093751](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903093751.png)

`v-model`属于双向绑定
![20220903094326](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903094326.png)

```html
 <!-- 如下代码是错误的，因为v-model只能应用在表单类元素（输入类元素）上 -->
<h2 v-model:x="name">你好啊</h2>
```
![20220903094820](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903094820.png)

**el与data的两种写法**
![20220903101558](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903101558.png)


**MVVM模型**
![20220903101947](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903101947.png)

![20220903102907](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903102907.png)


**数据代理**
![20220903114101](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903114101.png)

![20220903114150](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903114150.png)

vm._data中的数据做了一番小小的升级 为了做响应式 但是还是包含address和name


**事件处理**
![20220903163111](https://raw.githubusercontent.com/zmk-c/cloudImage/master/img/20220903163111.png)

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