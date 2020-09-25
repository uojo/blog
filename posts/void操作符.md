# void 操作符

用法：void(expression) 括号可以省略。

作用：计算表达式，并确保返回值是 undefined。

应用场景

```html
<!-- 点击后，不执行跳转超链接，让超链接去执行JS -->
<a href="javascript:void(0);">foo</a>
<!-- 点击后，实现表单提交 -->
<a href="javascript:void(document.form.submit())">单此处提交表单</a>
<!-- 点击后，先执行方法 foo，再执行 javascript:void(0)，如果foo后添加 return false，将不会执行-->
<a href="javascript:void(0)" onclick="foo()">点我</a>
```

JS 的几种跳转方式

方式一

```javascript
window.open();
```

方式二

```html
<script>
  function foo(id, el) {
    el.target = "_blank";
    el.href = "//xxx.com?id=" + id;
    el.click();
  }
</script>
<a href="javascript:;" onclick="foo(1,this)">foo</a>
```

方式三

```javascript
window.location.herf = "http://www.xxx.com";
```
