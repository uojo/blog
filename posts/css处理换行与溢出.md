## 文本换行

white-space 设置换行符（手动回车）、空格符（手动空格）是否有效。

- normal 换行符（无效），空格符（无效），自动换行
- nowrap 换行符（无效），空格符（无效），自动换行（否）
- pre 换行符（有效），空格符（有效），自动换行（否）
- pre-wrap 换行符（有效），空格符（有效），自动换行
- pre-line 换行符（有效），空格符（无效），自动换行

word-break 设置单词如何被拆分换行。

- normal 使用默认换行规则
- keep-all 保证所有单词显示在一行内，可能会引发长单词超出容器宽度
- break-all 无视单词完整性，到行末直接换行
- break-word 综合 keep-all 与 break-all，单词能显示一行就显示一行，不能的直接换行。

overflow-wrap (别名：word-wrap) 设置溢出一行的单词如何换行。

- normal 换行时，确保单词显示完整，即使溢出容器。
- break-word 只有当单词一行放不下才会拆分换行。

## 文本溢出

### 单行显示省略号

```css
width: 100px;
overflow: hidden;
white-space: no-wrap;
text-overflow: ellipsis;
```

### 多行显示省略号

```css
width: 100px;
overflow: hidden;
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 2;
```
