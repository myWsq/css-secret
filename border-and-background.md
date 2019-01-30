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


## 边框内圆角

有时我们需要一个容器, 内部是圆角, 但边框或描边保持直角, 使用两个容器嵌套可以轻松实现: 

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/7/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

这很简单, 不过要求我们使用两个元素实现. 还记得多重边框使用过的`outline`属性吗? 刚好它有不贴合圆角边框的特性. 如果我们用`box-shadow`将那些因为不贴合而产生的缝隙"堵住"的话, 就可以实现相同的效果.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/16/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## 条纹背景

条纹背景很常见, 通常的做法是生成一个独立的条纹位图文件作为背景图片. 对于每次改动, 我们都需要用图形编辑器来修改它. 即便是使用svg, 也需要一个独立文件. 如果能直接利用CSS生成条纹背景那就太棒了. 事实上, 真的可以:

<iframe width="100%" height="380" src="//jsfiddle.net/myWsq/7t1dybmc/27/embedded/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

观察下面的代码, 随着渐图案色标的拉近, 颜色过渡区域越来越小, 直到最后几乎消失. 

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/31/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>



> “如果多个色标具有相同的位置，它们会产生一个无限小的过渡区域， 过渡的起止色分别是第一个和最后一个指定值。从效果上看，颜色会在那个位置突然变化，而不是一个平滑的渐变过程。” ——CSS 图像(第三版)(http://w3.org/TR/css3-images)

现在我们已经创建了两条巨大的水平条纹. 因为渐变也是图案的一种, 所以我们也能通过`background-size`调整其尺寸, CSS中, 背景图案默认是平铺的, 所以我们会得到下面的效果:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/27/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### 不等宽条纹

只需要调整色标的位置, 保持两个色标位置相同即可创建不等宽条纹.

```css
{
    background: linear-gradient(#2196F3 30%,#B8E986 30%);
}
```

为了避免每次改动都需要修改两个数字, 让我们的代码更加DRY. 我们从规范里找到了捷径: 

> “如果某个色标的位置值比整个列表中在它之前的色标的位置值都要小，则该色标的位置值会被设置为它前面所有色标位置值的最大值。” ——CSS 图像(第三版)(http://w3.org/TR/css3-images)
 
 这意味着如果我们把第二个色标设置为0, 它会默认和前一个色标保持一致:

 ```css
{
    background: linear-gradient(#2196F3 30%,#B8E986 0);
}
```

### 多色条纹

我们发现, 要想实现条纹图案, 必须将渐变图案中两种颜色的色标置为相同的值. 这意味着渐变图案的颜色总是**成对出现的**, 如果我们想创建第三种颜色的条纹, 应该怎么做呢?

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/39/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

只要将第二对颜色的第一种颜色置为第一对颜色的第二种, 他们之间就不会产生过渡效果. 对应的色标即为第二对颜色的交界处.

### 垂直条纹

用代码写水平条纹是最简单的, 垂直条纹只需要我们旋转每个贴片, 并修改`background-size`属性.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/42/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### 斜向条纹

注意, 斜向的条纹略有不同. 如果直接旋转45度的话, 会得到崩坏的条纹图案. 我们回想一下实现条纹图案的原理: **通过贴片重复**. 就像下面的图, 斜向团的贴片略有不同:

![](http://ipic-1253962968.file.myqcloud.com/2019-01-30-111134.png)

结合我们刚才说到的多色条纹, 实现四种颜色的贴片. 通过贴片重复, 就可以构建出斜向条纹的图案了. 

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/54/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### 更灵活的条纹

假如我们需要的不是45度的条纹, 而是60度, 或者30度. 直接改变贴片角度明显行不通. 幸运的是我们有更好的方法创建条纹. 一个鲜为人知的真
相是 `linear-gradient()` 和 `radial-gradient()` 还各有一个循环式的加强版:`repeating-linear-gradient()` 和 `repeating-radial-gradient()`

他们的工作方式与前两者类似, 不同的是色标是无限循环的.

> 更详细的说明请转到 [repeating-linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/repeating-linear-gradient)

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/66/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

如果直接看代码有写难以理解的话, 我们可以尝试展开这个循环函数:

```css
{
    background: repeating-linear-gradient(60deg,#2196F3,#2196F3 25px,#B8E986 0,#B8E986 50px);
}
```
```css
{
    background: linear-gradient(60deg,
        #2196F3      ,#2196F3 25px,#B8E986 0,#B8E986 50px,
        #2196F3 50px ,#2196F3 75px,#B8E986 0,#B8E986 100px,
        #2196F3 100px ,#2196F3 125px,#B8E986 0,#B8E986 150px,
        ...
    );
}
```
现在, 我们可以随心所欲地改变条纹角度了, 这样做还有一个好处是我们可以灵活控制条纹的宽度而不用使用勾股定理计算每个贴片的大小.  不过必须制定四个色标, 而且代码更难理解一些.

### 更加DRY的条纹

在大多数情况下, 我们想要的条纹颜色组合差异并不大, 只是在明度方面有轻微的差异. 还记得我们的DRY按钮吗? 这里同样适用, 只要指定一个主色调作为背景色, 叠加明度不一的条纹即可达到效果.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/h6k2otcn/69/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

这样做还为我们提供了一个合理的回退方案, 在老旧的浏览器上将直接显示背景色. 

