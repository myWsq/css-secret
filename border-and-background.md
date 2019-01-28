# 边框与背景

## 半透明边框

要实现完美的半透明边框效果, 需要我们对边框的工作原理有一定了解. 观察下面的代码:

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/L10zcrnf/9/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

可以看出, 我们的半透明边框是存在的. 这打破了我们的设计意图: 让`body`的背景色透过边框而不是`border-box`本身的背景色. 指定`background-clip`可以调整上述行为带来的不便, 这个属性的默认值是`border-box`, 指定为`padding-box`后, 浏览器将沿着边框将目标容器的背景剪掉.

<iframe width="100%" height="300" src="//jsfiddle.net/myWsq/srkez2uL/1/embedded/html,css,result/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>