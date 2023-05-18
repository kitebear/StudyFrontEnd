实现 grid 布局的水平居中和垂直居中可以使用以下两种方式：
·
1.  使用 align-items 和 justify-items 属性

可以将 grid 容器设置为网格项目在水平和垂直方向都居中，方法是将 align-items 和 justify-items 属性都设置为 center。例如：

```css
复制.container {
  display: grid;
  align-items: center;
  justify-items: center;
}
```

2.  使用 grid-template-areas 属性

可以使用 grid-template-areas 属性，将所有的网格项目都放置在 grid 容器的中心区域，从而实现水平居中和垂直居中。例如：

```css
复制.container {
  display: grid;
  grid-template-areas: " center " ;
  justify-content: center;
  align-content: center;
}

.center {
  grid-area: center;
}
```

在这个示例中，grid-template-areas 属性设置了只有一个区域 center，而其他网格项目则被隐藏在边缘区域中；justify-content 和 align-content 属性将 center 区域放置于 grid 容器的中心。

以上两种方式都可以实现 grid 布局的水平居中和垂直居中，具体应该根据实际情况选择。
