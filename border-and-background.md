# 边框与背景

## 半透明边框

要实现完美的半透明边框效果, 需要我们对边框的工作原理有一定了解. 观察下面的代码:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/L10zcrnf/9/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

可以看出, 我们的半透明边框是存在的. 这打破了我们的设计意图: 让`body`的背景色透过边框而不是`border-box`本身的背景色. 指定`background-clip`可以调整上述行为带来的不便, 这个属性的默认值是`border-box`, 指定为`padding-box`后, 浏览器将沿着边框将目标容器的背景剪掉.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/srkez2uL/1/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 多重边框

多重边框无法像多重背景一样直接定义, 虽然可以通过`border-image`来模拟实现, 但是无法直接通过CSS灵活调整. 通常的做法是通过多个元素叠加hack. 我们可以通过更好的方法实现多重边框效果而不用额外增加无用的元素.

### box-shadow 方案

投影接受四个参数, 分别为横向偏移, 纵向偏移, 模糊值, 扩张半径. 一个正值得扩张半径加为零的模糊值和偏移量, 模拟出的投影就像实线边框.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/671wostc/15/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

投影是层层叠加的, 当需要添加多道投影时, 需要从第一层开始计算扩张半径. 并且投影不受`box-sizing`影响, 只出现在元素的外圈, 不影响元素布局而且不会触发鼠标事件. 不过我们可以给投影添加`inset`关键字, 相应的我们需要添加合适的内边距.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/dsp2359x/13/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### outline 方案

如果只需要两层边框, 通过添加`outline`可以方便的实现. 可以灵活调整边框样式, 调整与元素边缘的间距, 甚至接受负值.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/dsp2359x/17/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

可以发现, 描边不会贴合`border-radius`产生圆角, 这被CSS工作组认为是一个BUG, 不同浏览器可能会出现不同的效果, 需要额外注意.

## 灵活的背景定位

有时候我们想要针对容器的某个角进行偏移定位, 比如下面这张图: 

![](http://ipic-1253962968.file.myqcloud.com/2019-01-29-Artboard.jpg)

我们以元素的右下角为定位点, 增加了一些偏移量, 实现了内边距的效果. 在CSS2.1中, 我们只能以元素的左上角作为定位点. 对于固定尺寸的容器来说, 实现这一点需要做一些计算. 然而对于可变尺寸的元素来说, 不做一些hack几乎不可能实现. 借助现代CSS的特性, 我们有了更好的解决方案.

### background-position 的扩展语法方案

CSS3中, `background-position`属性已经得到了扩展, 它允许我们指定任意方向的偏移量, 只需要在偏移量前加关键字.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/48sqrfwj/10/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

此外, 我们还需要一个合适的回退方案. 在老旧的浏览器中, 我们的背景会直接定位到默认的左上角, 这看起来很奇怪. 我们在`background`的简写中加入`right bottom`, 在扩展语法失效的情况下, 背景将直接定位到右下角.

```css
{
    background:url(https://wsq.cool/css-secret/_assets/logo.svg)  no-repeat right bottom;
    background-size: 5em 5em;
    background-position: right 2em bottom 2em;
}
```

### background-origin 方案

当图片的偏移量等于与内边距时, 我们有更好的方案.你可能知道, 在CSS盒模型中, 每个元素身上都有三个矩形框:

![](http://ipic-1253962968.file.myqcloud.com/2019-01-30-Artboard-1.jpg)

`background-position`默认相对于`padding-box`定位.  通过指定`background-origin`属性, 可以使背景相对于`content-box`定位.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/mo5ewzg6/1/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### calc() 方案

借助新增的计算函数, 即便定位点依然在左上角, 也可以让浏览器替我们计算出合适的偏移量.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/mo5ewzg6/3/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
