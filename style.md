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