# Vuex 中 actions、mutation 的命名
实践过程中收集到的问题包括：

## 一、命名是否需要常量化？
分析如下：
- 常量后的命名难以阅读？见仁见智吧！有时是因为命名太长了，简化即可。
- 官方文档中描述，内容如下，[点击查看](https://vuex.vuejs.org/zh/guide/mutations.html#%E4%BD%BF%E7%94%A8%E5%B8%B8%E9%87%8F%E6%9B%BF%E4%BB%A3-mutation-%E4%BA%8B%E4%BB%B6%E7%B1%BB%E5%9E%8B)。
  > 使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然。

#### 如果以上能让你在 mutation 命名解决一些困惑的话，那么对于 actions 的命名是否有区别？

首先官方文档没有文字说明，其次官方的示例也没有反应，之后我在 github 上搜索一些项目，有的常量化例如 [vue-realworld-example-app](https://github.com/gothinkster/vue-realworld-example-app/blob/master/src/store/actions.type.js)，有的没有，[vue-spa](https://github.com/skyronic/vue-spa/blob/master/src/store/products.js) 。

没有结果，那么我们换一下角度，看看 [redux](https://github.com/nonoroazoro/simplest-react-redux-example/blob/master/client/actions/CounterActionCreators.js)、[redux-actions](https://redux-actions.js.org/api-reference/createaction-s)、[vuex-actions](https://www.npmjs.com/package/vuex-actions)。我们讨论的是 vuex 怎么和 redux 联系上了？
> 答： flux 是状态集管理框架，redux 是 flux 的一种实现，vuex 是基于 redux 进行改变。所以理论上是互通的，实现单向数据流，状态集统一管理。

## 二、是否需要定义 types 文件用于管理命名？
引用上个问题中官方文档描述：
> 常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然。

同样找了些项目示例，有的有，有的没。

那么这样表述是否合理：对于复杂或大型的项目，将同类、通用功能进行分类集中管理是否是“一个好习惯”。


## 三、总结
结合自己团队、项目场景等因素进行系统设计，但是前提是了解及认可各环节的合理性，然后自己参与的系统设计时进行取舍。
