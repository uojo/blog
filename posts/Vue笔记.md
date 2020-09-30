## 安装

- 官方 vue-cli 新建项目
- 独立构建和运行时构建的区别！即：写 template ，还是写 render
  - 独立构建：支持 template 选项，不能进行服务器端渲染
  - 运行时构建：不支持 template 选项，默认 npm 包导出是使用 “运行时”，改为“独立构建”需在 webpack 中配置 resolve.alias

## 开始

- 所有的组件都是拓展 Vue 后被实例化：new ( Vue.extend({...}))()
- 注意：数组的变异方法
  - 修改索引与长度将无法检测变动，需使用 set

## 使用

- new Vue(options) 中 options
  - components：{ "name":Component }
    - 组件<键/>只在父组件使用，
    - 组件<键/>在 template 标签中，可以使用 小写加-连接 来调用
  - computed：[function]
    - 值为函数，当键需要使用在 template 中使用时才会执行
    - 函数内的 this 指向 组件实例

### 指令：v- 打头，可以接受一个“参数”，可以添加“修饰符”

- v-bind:title="doSomething" ：将 DOM 中的 title 属性与 message 变量绑定，缩写为：title="doSomething"
- v-for
  - <span v-for="n in 10">{{$index}} - {{ n }}</span> 整数
  - v-for="(value, key, index) in object"
  - v-for="(item, index) in array"
  - <my-component v-for="(item, index) in items" v-bind:item="item" v-bind:index="index"></my-component> 组件的注入
  - <select v-model="selected"><option v-for="option in options" v-bind:value="option.value"></option></select>
- v-if / v-show / v-else
- v-on：绑定一个监听事件用于调用自定义方法 v-on:click.prevent="FnName"，缩写为@click="doSomething"
- v-model：将表单输入与数据建立双向绑定
  - 应用在自定义组件中时，需要组件内实现 props:["value"] ,且 有 value 变化时，触发 \$emit("input", newValue)
- v-once：一次性插值，之后改变不会更新，当有大量静态内容时
- v-html：双大括号解释为纯文本，所以输出 HTML 用

### 实例

- 属性和方法：
  - 前缀 $ 是保留的，在实例中 $data $watch $el ...
  - 在实例属性中避免使用箭头函数，因为 this 未定义

#### 实例的生命周期，四段八钩

- beforeCreate(){...}
  - 初始化事件、数据观测
- created(){...}
  - 初始化数据
- beforeMount(){...}
  - 模板编译结束，生成 render function
  - 优先级：render 函数 > template 参数 > outerHTML 模板
- mounted(){...}
  - 赋值 el
  - watcher 再执行
- beforeUpdate(){...}
- updated(){...}
  - this.\$nextTick === Vue.nextTick(function(){ ... })
- beforeDestroy(){...}
  - 实例仍然可用
- destroyed
  - 卸载 watcher、child-components、event

### 模板的语法，mustache （双大括号）

- {{}} 不能使用在模板的属性上，需要使用 v-bind:domAttr="data.field"
- {{}} 与 v-bind 中写可以是表达式（必须是单个表达式，不能是语句）
- {{}} 中可以使用 过滤器（管道符分隔），通过 filters : {...} 定义过滤器函数，过滤器可以串联，可以接受参数： filterA('arg1', arg2) "arg1"作为第二参数

### 计算属性 computed：为了清晰和简单，任何复杂逻辑，都需使用计算属性。

- Vue 检测到数据发生变动时就会执行对相应数据有引用的函数，注意和 methods 的区别
- 虽与 methods 效果相同，不过其依赖缓存，即依赖的属性不发生变化，不会重新计算（例如 Date() ）
- watch 也可以试想相同效果，不过在多依赖下，写法上更简单；
  - watch 的优势：可以很好的利用 异步进行过渡
- 提供钩子（ get 与 set）方式的定义

### class 与 style 的绑定

- <div v-bind:class="{ 'active': isActive }"></div>
- <div v-bind:class="classObject"></div> 绑定一个对象（data.classObject 或 computed.classObject ）
- <div v-bind:class="[activeClass, errorClass]"> 传数组
- <div v-bind:class="[{ active: isActive }, errorClass]"> 数组中套对象
- 应用在自定义组件上是，可以这样：<my-component class="c1 c2"></my-component>，将会追加到 my-component 组件内的 class 后
- style 与 class 使用类同
  - 如 transform 会自动添加前缀

### 事件 v-on:click / @click

- 元素 DOM 事件 $event ： @click="fn($event)"
- 修饰符，可以串联：
  - .stop / .prevent / .capture /.self / .键值（.13 .enter ...）
- 自定义事件
  - \$on(eventName) 监听事件
  - \$emit(eventName) 触发事件
- watch
  - 注意值需使用 function，非箭头函数
  - deep：true，需定义为 a:{deep:true,handler:function(val,old){}}
  - 键名，可以是 a.b.c

### 表单

- 使用 v-model 与表单控件绑定
- 复选框、多选列表可以绑定到同一个数组
- 复选框的绑定：<input type="checkbox" v-model="toggle" v-bind:true-value="a" v-bind:false-value="b" > 变量 a==toggle（当 value==true 时）
- 修饰符
  - <input v-model.lazy="express" > 在 change 下在更新 express
  - .number 类型转化
  - .trim 过滤首尾空格

### 组件

- 命名：？
- data 类型为函数，return 对象
- props：语法（字面量、动态、数据类型【监听符合数据类型的值】）
  - 单向数据流：父组件的属性变化时传导到子组件；想回传通过 自定义事件
  - <Cpt my-msg="v1"/> 申明 props:["myMsg"]
    - v1 是个值
  - :my-msg="v1" 申明 props:["myMsg"]，绑定的可以是变量，也可以是个字面量的值
    - v1 是个变量
    - 注意：组件属性 my-msg 是使用连接符，因为 HTML 标签名与属性名大小写不敏感。
  - 验证及指明类型
  - 保留字段 reserved
    - key
- 内联申明 ref，使用 `this.$ref.xxx` 取得是组件实例

#### slot 组件：备用块，可以使用 name 属性指向

#### keep-alive 组件：保留该组件的状态避免重新渲染

#### 异步组件

```javascript
// 当初始数据需要异步请求
Vue.component("async-example", function (resolve, reject) {
  setTimeout(function () {
    resolve({ template: "<div>I am async!</div>" });
  }, 1000);
});
```

#### 父子组件双向绑定（通信）

- (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 v-on 侦听器
  - <child :childHandle.sync="parentValue"></child> 更新：子组件内 this.\$emit("update:childHandle",newValue)
    - 等于 <comp :childHandle="parentValue" @update:childHandle="newValue=> parentValue= newValue"></comp>
  - <child v-on:childHandle="parentFunc"></child> 更新：子组件内 this.\$emit("childHandle")
- <child :msg="parentMsg"></child> 通过传入对象

#### mixins 混合

#### plugin

功能：

- 添加全局方法或属性
- 添加全局资源：指令、过滤器、过渡等
- 通过全局 mixin 方法添加一些组件选项
- 通过设置 Vue.prototype ，添加实例共享方法

使用

- Vue.use(myPlugin) 在 new Vue(...) 之前执行

开发

- myPlugin = {install(Vue,options){...}} 也可以直接导出一个方法， Vue.use 会直接执行

> 参考：https://cn.vuejs.org/v2/guide/plugins.html

### 参考

1. 官方文档：http://cn.vuejs.org/v2/guide/
2. 生态系统：https://github.com/vuejs/awesome-vue#libraries--plugins
3. 台湾小凡：https://github.com/bhnddowinf/vuejs2-learn
4. 经验：http://www.cnblogs.com/kidsitcn/p/5409994.html
5. UI Components Library：
   1. https://github.com/iview/iview
