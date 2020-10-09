Block Formatting Contexts 块状格式化上下文

背景：元素的布局常见的方案包括普通流（normal flow）、浮动（float）、绝对定位（absolute positioning）。

BFC 是 CSS2.1 规范中的一个概念。属于普通流，主要特点在于：“内部是一个独立封闭的空间，不对外部产生影响”。

### BFC 的特征（规则）

1. BFC 内部不会影响外部。
2. 同一个 BFC 容器内的两个相邻盒子元素的 margin 会重叠。
3. BFC 区域不会与浮动区域重叠。
4. 计算 BFC 容器高度时，容器内的浮动元素会参与计算。

### 触发 BFC 的方式

- 根元素(html 标签)
- float 值不为 none
- position 值为 absolute、fixed。
- display 值为 inline-block、table-cell、flex ……
- overflow：hidden、auto、scroll

### BFC 的应用

#### 示例 1：margin 重叠。

根据[规则 2]所述，两个 `.box` 都是同一个 BFC 容器（html 标签）内的两个相邻元素，所以上下 margin 会重叠。

> 注意：是容器内，不限定是子元素还是孙元素。

```html
<style>
  .box {
    width: 100px;
    height: 100px;
    background: red;
    margin: 100px;
  }
</style>
<body>
  <div class="wrap">
    <div class="box"></div>
    <div class="box"></div>
  </div>
</body>
```

解决办法：

- 将 .box 元素外层增加一层嵌套标签，并触发嵌套元素的 BFC，使每个 .box 元素在不同的 BFC 容器内。

#### 示例 2：包含浮动元素的容器，高度塌陷。

```html
<!-- 父容器仅有一个浮动元素时，父容器的高度只有 2px。 -->
<div style="border: 1px solid #000;">
  <div style="width: 100px; height: 100px; float: left;"></div>
</div>
```

解决办法：

- 父容器添加样式 overflow:hidden

#### 示例 3：浮动元素与非 BFC 元素会重叠显示

```html
<div style="height: 100px;border: 4px solid #000;">
  <div style="background-color: green; width: 100px; float: left;">aside</div>
  <div style="background-color: blue; overflow: hidden;">
    BFC区域不会与浮动元素重叠，否则重叠
  </div>
</div>
```

## 参考

- https://zhuanlan.zhihu.com/p/25321647
- https://www.jianshu.com/p/8d5f2a35f5a7
