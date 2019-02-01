# 形状

## 自适应的椭圆

给元素添加足够大的`border-radius`可以创建一个圆形:

```css
{
    height:200px;
    width:200px;
    border-radius: 100px;
}
```

就算指定一个大于100px的值, 也会是一样的结果, 规范中指出当`border-radius`大到相互重叠时, 必须按比例减小直到不再重叠为止.

> “当任意两个相邻圆角的半径之和超过 border box 的尺寸时，用户 代理必须按比例减小各个边框半径所使用的值，直到它们不会相互重叠为止。” ——CSS 背景与边框(第三版)(http://w3.org/TR/css3- background/#corner-overlap)

大多数情况下, 我们往往不愿意指定一个元素的宽高, 我们需要它能根据内容自动调整尺寸. 在这个案例中, 我们通常希望达到如下的效果: **如果宽高相等就显示为圆, 不等则显示为椭圆.** `border-radius`允许我们分开指定水平半径和垂直半径, 需要用`/`分隔:

```css
{
    height:150px;
    width:200px;
    border-radius: 100px / 75px;
}
```
此外, `border-radius`也允许我们使用百分比尺寸, 分别对应元素本身的高度和宽度. 这样实现一个自适应的椭圆只需要我们指定:

```css
{
    border-radius: 50% / 50%;
}
```

这两个`50%`将来会被计算为不同的值, 我们也可以简写为:

```css
{
    border-radius: 50%;
}
```

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/0hwb7u4s/12/embedded/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### 半椭圆

实际上, `border-radius`是一个简写, 我们可以分别指定一个元素四个角的圆角半径.

* border-top-left-radius
* border-top-right-radius
* border-bottom-right-radius
* border-bottom-left-radius

与通常的CSS简写相同, 我们也可以一次指定`border-radius`的四个值, 这四个值分别对应元素左上角顺时针旋转的四个圆角值.  指定三个则第四个与第二个相同, 指定两个则第一个与第三个相同, 第二个与第四个相同. 这与其他CSS简写的工作原理类似.

另外, 我们也可以分别指定四个角的水平圆角半径和垂直圆角半径. 只要在`/`前后分别指定四个值即可.

<iframe width="100%" height="500" src="//jsfiddle.net/myWsq/0hwb7u4s/37/embedded/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 平行四边形

在视觉设计中, 平行四边形可以传达一些动感. 通过形变函数`skew()`对普通矩形进行拉伸即可生成平行四边形.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/jp9371zd/3/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

我们观察到, 不仅按钮发生了形变, 它的内容同样也发生了形变, 这很不美观而且难以阅读. 有没有什么办法只让容器形变而不影响到它的内容呢? 

### 嵌套元素方案

这很简单, 主要思路是对内层的元素进行反向的形变. 我们不得不新增额外的元素来实现这一效果.

```css
button{
    transform:skew(-30deg);
}
button > span{
    transform:skew(30deg);
}
```

### 伪元素方案

另一种思路是把所有的样式用在伪元素上, 包括背景等, 而控制元素尺寸的属性依然用在宿主元素上. 然后再对伪元素进行形变. 我们希望伪元素具有良好的可维护性, 成为DRY的伪元素, 它的尺寸由宿主元素决定. 一个简单的办法是使用**绝对定位**, 将伪元素拉伸至与宿主元素相同的尺寸.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/jp9371zd/10/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

**这个方案还适用于其他形变形式, 或者说还适用于任何我们希望改变容器样式而不改变内容样式的情况.**