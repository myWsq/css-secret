# 风格与技巧

编写实际项目的原则是平衡可维护性与代码复杂度. 

有时候**代码易维护和代码量少不可兼得**, 如何做出取舍是一个优秀的程序员面临的最大挑战.

## 贯彻落实DRY原则

> DRY 是 Don’t Repeat Yourself 的首字母缩写，意思是不应该重复你已经做过的事。

所谓DRY, 即尽量减少改动时要编辑的地方, 并不是代码量越少越好.

我们给下面这个按钮添加一些效果
<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/rpnL54qv/27/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

这段代码存在一些可维护性问题, 首先是字号相关的问题. 如果我们想要改变字号, 就要同时调整行高. 这两个属性都写成了绝对值. 更麻烦的是我们需要做一些算数, 算出行高应该是多少.

> 当某些值相互依赖时, 应该把他们的相互关系用代码表达出来.

这段代码应该改成这样会更易维护:

```css
{
    font-size:16px;
    line-height:1.5;
}
```

一般情况下, 我们的button字号往往与父元素或根元素`<html>`相关. 如果采用绝对值控制字号, 虽然很简单, 但是在我们修改父元素字号`em`或根元素字号`rem`时, 不得不修改所有按钮的字号. 如果改用百分比就好多了:

```css
{
    font-size:1em;
    line-height:1.5
}
```

现在我们修改父元素字号时, 按钮的尺寸会随之变化, 此时还有一些尺寸是用绝对值表示的, 所以看起来会有一些不协调. **此时就需要重新审视到底哪些效果应该跟着按钮一起放大, 而哪些效果是保持不变的.** 比如在这个例子中, 我们希望按钮的边框粗细保持在1px, 不受按钮尺寸的影响.

```css
{
    padding: .375em 1em;
    border-radius: .25em;
    border: 1px solid #fff;
}
```

除了按钮的尺寸, 颜色是另一个重要的变数. 在例子中, 除主色调外, 我们还需要推算出变亮变暗的其他三个版本. 事实上, 只要将一个半透明的遮罩叠加在主色调上, 即可产生不同的变体:

```css
button{
    background: #2895f0 linear-gradient(rgba(0,0,0,.2), transparent);
}
button:active{
    background: #2895f0 linear-gradient(rgba(0,0,0,.05), rgba(0,0,0,.1));
}
```
接下来, 我们只要简单的改变字号大小与主色调即可产生不同按钮变体:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/rpnL54qv/43/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

### 更多DRY技巧

**1. 不要一味追求代码量少.**

来看看下面这段代码, 我们要给元素添加10px宽的边框, 但左侧不加边框.

```css
{
    border-width:10px 10px 10px 0;
}
```
只要一条声明就足够了, 但是日后如果需要改动边框宽度, 我们需要同时修改三处. 如果将这条声明拆开写, 就方便多了.

```css
{
    border-width: 10px;
    border-left-width: 0;
}
```

**2. currentcolor**

这是一个特殊的关键字, 一直被解析为当前元素的文本颜色`color`. 这可能是CSS有史以来第一个**变量**. 假如我们想让`hr`标签的颜色与文本颜色相同, 只需这样写:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/fr3okw1h/13/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

发现了吗? 似乎很多已有的属性也具有类似的行为. **currentcolor本身是很多CSS颜色属性的初始值.** 比如`border-color`,`outline-color`以及`text-shadow`,`box-shadow`的颜色值, 等等.

**3. 继承**

`inherit`是一个很容易被遗忘的关键字. 它可以作用于任何CSS属性中并绑定父元素的计算值(对伪元素来说, 则会取生成改伪元素的宿主元素). 举例来说, 要将表单元素的字体设置为与页面其他元素相同, 我们只需利用inherit的特性而不是重新指定字体:

```css
input, select, button { font: inherit; }
```

这个技巧在编写气泡等依赖性较强的元素时, 非常有用:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/anujxw60/31/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>