当然可以，这里提供一个基本的 CSS Grid 布局的案例。

HTML 部分：

```html
复制<div class="container">
  <div class="item item1"></div>
  <div class="item item2"></div>
  <div class="item item3"></div>
  <div class="item item4"></div>
  <div class="item item5"></div>
  <div class="item item6"></div>
  <div class="item item7"></div>
  <div class="item item8"></div>
  <div class="item item9"></div>
</div>
```

CSS 部分：

```css
复制.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 三列，每列的宽度平均分，也可以使用像素值或百分比来定义 */
  grid-template-rows: repeat(3, 100px); /* 三行，每行的高度都是100像素，同样也可以使用像素值或百分比来定义 */
  gap: 10px; /* 每个网格之间的间隙为10像素 */
}

.item {
  background-color: #eee;
  padding: 10px;
  text-align: center;
}

.item1 {
  grid-column: 1 / 3; /* 定义item1所跨越的列为第1和第2列 */
  grid-row: 1; /* 定义item1所在的行为第1行 */
}

.item2 {
  grid-column: 3; /* 定义item2所在的列为第3列 */
  grid-row: 1 / 3; /* 定义item2所跨越的行为第1和第2行 */
}

.item3 {
  grid-column: 1; /* 定义item3所在的列为第1列 */
  grid-row: 2 / 4; /* 定义item3所跨越的行为第2、3行 */
}

.item4 {
  grid-column: 2; /* 定义item4所在的列为第2列 */
  grid-row: 3; /* 定义item4所在的行为第3行 */
}

.item5 {
  grid-column: 3; /* 定义item5所在的列为第3列 */
  grid-row: 4; /* 定义item5所在的行为第4行 */
}

.item6 {
  grid-column: 1; /* 定义item6所在的列为第1列 */
  grid-row: 4; /* 定义item6所在的行为第4行 */
}

.item7 {
  grid-column: 2 / 4; /* 定义item7所跨越的列为第2、3列 */
  grid-row: 5; /* 定义item7所在的行为第5行 */
}

.item8 {
  grid-column: 1; /* 定义item8所在的列为第1列 */
  grid-row: 6; /* 定义item8所在的行为第6行 */
}

.item9 {
  grid-column: 2; /* 定义item9所在的列为第2列 */
  grid-row: 6; /* 定义item9所在的行为第6行 */
}
```

这个案例展示了一个 3*3 的网格，其中每个网格的大小都是 100px，每个网格之间有 10px 的间隙，使用 `grid-column` 和 `grid-row` 定义了每个网格所占的位置。