## 循环生成样式类

```less
// 生成字体大小
@fontSize: 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32;
.fontSizeLoop(@items, @i: 1) when(@i <=length(@items)) {
  @item: extract(@items, @i);
  .f@{item} {
    font-size: @item * 1px;
  }

  .fontSizeLoop(@items, @i + 1);
}
.fontSizeLoop(@fontSize, 1);

// 生成字号
.loop(@c)when(@c <7){
  h@{c}{
    padding: (10px * @c);
  }// 每次调用时产生的样式代码
  .loop((@c + 1));// 递归调用自身
}
.loop(1); // 调用循环 生成样式
```
