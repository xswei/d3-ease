# d3-ease

*Easing* 是一种通过扭曲时间来控制动画中的表现形式的方法。通常被用来 [slow-in, slow-out](https://en.wikipedia.org/wiki/12_basic_principles_of_animation#Slow_In_and_Slow_Out)。通过对时间的缓动，[animated transitions](https://github.com/d3/d3-transition) 会更平滑且运动过程也更合理。

此模块中的缓动类型实现了 [ease method](#ease_ease)，传递一个归一化的时间 *t* 会返回一个经过缓动处理之后的时间 *tʹ*。两者通常都处于 [0,1] 之间，其中 `0` 表示动画的开始而 `1` 表示的是动画的结束；有一些缓动类型比如 [elastic](#easeElastic) 返回的值可能会超出这个范围。一个好的缓动类型应该在 *t* 等于 `0` 时候返回 `0` 并且 *t* 等于 `1` 时返回 `1`。参考 [easing explorer](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4) 来直观的感受 *t* 和 *tʹ* 之间的映射关系。

这些缓动类型大多数都是基于 [Robert Penner](http://robertpenner.com/easing/) 的作品.

## Installing

`NPM` 安装: `npm install d3-ease`. 此外还可以下载 [latest release](https://github.com/d3/d3-ease/releases/latest). 可以直接从 [d3js.org](https://d3js.org) 以 [standalone library(单独的标准库)](https://d3js.org/d3-ease.v1.min.js) 或作为 [D3 4.0](https://github.com/d3/d3) 的一部分直接载入. 支持 `AMD`, `CommonJS`, 以及基础的标签引入形式。如果使用标签引入则会暴露 `d3` 全局变量:

```html
<script src="https://d3js.org/d3-ease.v1.min.js"></script>
<script>

var ease = d3.easeCubic;

</script>
```

[在浏览器中测试 d3-ease ](https://tonicdev.com/npm/d3-ease)

## API Reference

<a name="_ease" href="#_ease">#</a> <i>ease</i>(<i>t</i>)

给定归一化的时间 *t*，通常在 [0, 1] 之间，然后返回对应缓动的时间 *tʹ*, 返回值通常也在 [0, 1] 之间。`0` 表示的是动画开始而 `1` 表示的是动画结束。一个好的缓动设计会在 *t* = 0 时返回 `0` 而 *t* = 1 时返回 `1`。参考 [easing explorer](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4)。例如应用 [cubic](#easeCubic) 缓动:

```js
var te = d3.easeCubic(t);
```

同理，应用自定义的 [elastic](#easeElastic) 缓动:

```js
// Before the animation starts, create your easing function.
var customElastic = d3.easeElastic.period(0.4);

// During the animation, apply the easing function.
var te = customElastic(t);
```

<a name="easeLinear" href="#easeLinear">#</a> d3.<b>easeLinear</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/linear.js "Source")

`Linear(线性)` 缓动。恒等函数，*linear*(*t*) 返回 *t*。

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/linear.png" alt="linear" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#linear)

<a name="easePolyIn" href="#easePolyIn">#</a> d3.<b>easePolyIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/poly.js#L3 "Source")

`Polynomial(多项式)` 缓动; 将 *t* 进行 [exponent](#poly_exponent) 运算. 如果指数没有指定则默认为 `3`, 等价于 [cubicIn](#easeCubicIn).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/polyIn.png" alt="polyIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#polyIn)

<a name="easePolyOut" href="#easePolyOut">#</a> d3.<b>easePolyOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/poly.js#L15 "Source")

反转 `polynomial` 缓动; 等价于 1 - [polyIn](#easePolyIn)(1 - *t*). 如果 [exponent](#poly_exponent) 没有被指定则默认为 `3`, 等价于 [cubicOut](#easeCubicOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/polyOut.png" alt="polyOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#polyOut)

<a name="easePoly" href="#easePoly">#</a> d3.<b>easePoly</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/poly.js "Source")
<br><a name="easePolyInOut" href="#easePolyInOut">#</a> d3.<b>easePolyInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/poly.js#L27 "Source")

`Symmetric polynomial(对称多项式)` 缓动; 在 *t* 处于 [0, 0.5] 时使用 [polyIn](#easePolyIn) 而 *t* 处于 [0.5, 1] 时 使用 [polyOut](#easePolyOut). 如果没有指定 [exponent](#poly_exponent) 则默认为 `3`. 等价于 [cubic](#easeCubic).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/polyInOut.png" alt="polyInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#polyInOut)

<a name="poly_exponent" href="#poly_exponent">#</a> <i>poly</i>.<b>exponent</b>(<i>e</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/poly.js#L1 "Source")

根据指定的指数 *e* 返回一个新的多项式缓动函数. 例如分别创建 [linear](#easeLinear), [quad](#easeQuad), 和 [cubic](#easeCubic) 缓动函数:

```js
var linear = d3.easePoly.exponent(1),
    quad = d3.easePoly.exponent(2),
    cubic = d3.easePoly.exponent(3);
```

<a name="easeQuadIn" href="#easeQuadIn">#</a> d3.<b>easeQuadIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/quad.js#L1 "Source")

`Quadratic(二次缓动)`; 等价于 [polyIn](#easePolyIn).[exponent](#poly_exponent)(2).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/quadIn.png" alt="quadIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#quadIn)

<a name="easeQuadOut" href="#easeQuadOut">#</a> d3.<b>easeQuadOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/quad.js#L5 "Source")

反转 `quadratic` ; 等价于 1 - [quadIn](#easeQuadIn)(1 - *t*). 也等价于 [polyOut](#easePolyOut).[exponent](#poly_exponent)(2).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/quadOut.png" alt="quadOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#quadOut)

<a name="easeQuad" href="#easeQuad">#</a> d3.<b>easeQuad</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/quad.js "Source")
<br><a name="easeQuadInOut" href="#easeQuadInOut">#</a> d3.<b>easeQuadInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/quad.js#L9 "Source")

`Symmetric quadratic(对称二次缓动)` ; *t* 处于[0, 0.5] 时使用 [quadIn](#easeQuadIn) 而 *t* 处于 [0.5, 1] 时使用 [quadOut](#easeQuadOut).  也等价于 [poly](#easePoly).[exponent](#poly_exponent)(2).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/quadInOut.png" alt="quadInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#quadInOut)

<a name="easeCubicIn" href="#easeCubicIn">#</a> d3.<b>easeCubicIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/cubic.js#L1 "Source")

`Cubic(三次缓动)`；等价于 [polyIn](#easePolyIn).[exponent](#poly_exponent)(3)

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/cubicIn.png" alt="cubicIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#cubicIn)

<a name="easeCubicOut" href="#easeCubicOut">#</a> d3.<b>easeCubicOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/cubic.js#L5 "Source")

反转 `cubic(三次缓动)`; 等价于 1 - [cubicIn](#easeCubicIn)(1 - *t*). 也等价于 [polyOut](#easePolyOut).[exponent](#poly_exponent)(3).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/cubicOut.png" alt="cubicOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#cubicOut)

<a name="easeCubic" href="#easeCubic">#</a> d3.<b>easeCubic</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/cubic.js "Source")
<br><a name="easeCubicInOut" href="#easeCubicInOut">#</a> d3.<b>easeCubicInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/cubic.js#L9 "Source")

`Symmetric cubic(对称三次缓动)`; *t* 处于 [0, 0.5] 时使用 [cubicIn](#easeCubicIn) 而 *t* 处于 [0.5, 1] 时使用 [cubicOut](#easeCubicOut)。也等价于 [poly](#easePoly).[exponent](#poly_exponent)(3).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/cubicInOut.png" alt="cubicInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#cubicInOut)

<a name="easeSinIn" href="#easeSinIn">#</a> d3.<b>easeSinIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/sin.js#L4 "Source")

`Sinusoidal(正弦缓动)`; 返回 `sin(*t*)`.

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/sinIn.png" alt="sinIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#sinIn)

<a name="easeSinOut" href="#easeSinOut">#</a> d3.<b>easeSinOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/sin.js#L8 "Source")

反转 `sinusoidal(正弦缓动)`; 等价于 1 - [sinIn](#easeSinIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/sinOut.png" alt="sinOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#sinOut)

<a name="easeSin" href="#easeSin">#</a> d3.<b>easeSin</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/sin.js "Source")
<br><a name="easeSinInOut" href="#easeSinInOut">#</a> d3.<b>easeSinInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/sin.js#L12 "Source")

对称 `sinusoidal(缓动)`; *t* 处于 [0, 0.5] 时使用 [sinIn](#easeSinIn) 而 *t* 处于 [0.5, 1] 时使用 [sinOut](#easeSinOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/sinInOut.png" alt="sinInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#sinInOut)

<a name="easeExpIn" href="#easeExpIn">#</a> d3.<b>easeExpIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/exp.js#L1 "Source")

`Exponential(指数)` 缓动; 映射为 2 的 10 \* (*t* - 1) 次方.

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/expIn.png" alt="expIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#expIn)

<a name="easeExpOut" href="#easeExpOut">#</a> d3.<b>easeExpOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/exp.js#L5 "Source")

反转 `exponential(指数缓动)`; 等价于 1 - [expIn](#easeExpIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/expOut.png" alt="expOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#expOut)

<a name="easeExp" href="#easeExp">#</a> d3.<b>easeExp</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/exp.js "Source")
<br><a name="easeExpInOut" href="#easeExpInOut">#</a> d3.<b>easeExpInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/exp.js#L9 "Source")

对称 `exponential(指数缓动)`; *t* 处于 [0, 0.5] 时使用 [expIn](#easeExpIn) 而 *t* 处于 [0.5, 1] 时使用 [expOut](#easeExpOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/expInOut.png" alt="expInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#expInOut)

<a name="easeCircleIn" href="#easeCircleIn">#</a> d3.<b>easeCircleIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/circle.js#L1 "Source")

`Circular(环形缓动)`.

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/circleIn.png" alt="circleIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#circleIn)

<a name="easeCircleOut" href="#easeCircleOut">#</a> d3.<b>easeCircleOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/circle.js#L5 "Source")

反转 `Circular(环形缓动)`; 等价于 1 - [circleIn](#easeCircleIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/circleOut.png" alt="circleOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#circleOut)

<a name="easeCircle" href="#easeCircle">#</a> d3.<b>easeCircle</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/circle.js "Source")
<br><a name="easeCircleInOut" href="#easeCircleInOut">#</a> d3.<b>easeCircleInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/circle.js#L9 "Source")

反转 `circular(缓动)`; *t* 处于 [0, 0.5] 时使用 [circleIn](#easeCircleIn) 而 *t* 处于 [0.5, 1] 时使用 [circleOut](#easeCircleOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/circleInOut.png" alt="circleInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#circleInOut)

<a name="easeElasticIn" href="#easeElasticIn">#</a> d3.<b>easeElasticIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js#L5 "Source")

`Elastic(震荡缓动)`, 像一个震荡的橡皮筋. 震荡的 [amplitude(振幅)](#elastic_amplitude) 和 [period(周期)](#elastic_period) 是可以配置的; 如果没有指定则分别默认为 `1` 和 `0.3`.

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/elasticIn.png" alt="elasticIn" width="100%" height="360">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#elasticIn)

<a name="easeElastic" href="#easeElastic">#</a> d3.<b>easeElastic</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js "Source")
<br><a name="easeElasticOut" href="#easeElasticOut">#</a> d3.<b>easeElasticOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js#L18 "Source")

反转 `elastic(震荡缓动)` 等价于 1 - [elasticIn](#easeElasticIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/elasticOut.png" alt="elasticOut" width="100%" height="360">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#elasticOut)

<a name="easeElasticInOut" href="#easeElasticInOut">#</a> d3.<b>easeElasticInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js#L31 "Source")

对称 `elastic(震荡缓动)`; *t* 处于 [0, 0.5] 时使用 [elasticIn](#easeElasticIn) 而 *t* 处于 [0.5, 1] 时使用 [elasticOut](#easeElasticOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/elasticInOut.png" alt="elasticInOut" width="100%" height="360">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#elasticInOut)

<a name="elastic_amplitude" href="#elastic_amplitude">#</a> <i>elastic</i>.<b>amplitude</b>(<i>a</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js#L40 "Source")

根据指定的振幅 *a* 返回一个新的震荡缓动.

<a name="elastic_period" href="#elastic_period">#</a> <i>elastic</i>.<b>period</b>(<i>p</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/elastic.js#L41 "Source")

根据指定的周期 *p* 返回一个新的震荡缓动.

<a name="easeBackIn" href="#easeBackIn">#</a> d3.<b>easeBackIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/back.js#L3 "Source")

[Anticipatory](https://en.wikipedia.org/wiki/12_basic_principles_of_animation#Anticipation) 缓动, 就像是一个舞者在跳跃之前先将自己的膝盖弯曲。 超调量 [overshoot](#back_overshoot) 是可配置的。如果没有指定的话默认为 `1.70158`。

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/backIn.png" alt="backIn" width="100%" height="300">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#backIn)

<a name="easeBackOut" href="#easeBackOut">#</a> d3.<b>easeBackOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/back.js#L15 "Source")

反转 `anticipatory` 缓动; 等价于 1 - [backIn](#easeBackIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/backOut.png" alt="backOut" width="100%" height="300">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#backOut)

<a name="easeBack" href="#easeBack">#</a> d3.<b>easeBack</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/back.js "Source")
<br><a name="easeBackInOut" href="#easeBackInOut">#</a> d3.<b>easeBackInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/back.js#L27 "Source")

对称 `anticipatory` 缓动; 在 *t* 处于 [0, 0.5] 时使用 [backIn](#easeBackIn) 而 *t* 处于 [0.5, 1] 时使用 [backOut](#easeBackOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/backInOut.png" alt="backInOut" width="100%" height="300">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#backInOut)

<a name="back_overshoot" href="#back_overshoot">#</a> <i>back</i>.<b>overshoot</b>(<i>s</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/back.js#L1 "Source")

根据指定的超调量 *s* 返回一个新的 `anticipatory` 缓动.

<a name="easeBounceIn" href="#easeBounceIn">#</a> d3.<b>easeBounceIn</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/bounce.js#L12 "Source")

`Bounce(弹跳缓动)`, 就像是一个橡皮球.

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/bounceIn.png" alt="bounceIn" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#bounceIn)

<a name="easeBounce" href="#easeBounce">#</a> d3.<b>easeBounce</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/bounce.js "Source")
<br><a name="easeBounceOut" href="#easeBounceOut">#</a> d3.<b>easeBounceOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/bounce.js#L16 "Source")

反转 `Bounce(弹跳缓动)` 等价于 1 - [bounceIn](#easeBounceIn)(1 - *t*).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/bounceOut.png" alt="bounceOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#bounceOut)

<a name="easeBounceInOut" href="#easeBounceInOut">#</a> d3.<b>easeBounceInOut</b>(<i>t</i>) [<>](https://github.com/d3/d3-ease/blob/master/src/bounce.js#L20 "Source")

对称 `Bounce(弹跳缓动)`; 在 *t* 处于 [0, 0.5] 时使用 [bounceIn](#easeBounceIn) 而 *t* in [0.5, 1] 时使用 [bounceOut](#easeBounceOut).

[<img src="https://raw.githubusercontent.com/d3/d3-ease/master/img/bounceInOut.png" alt="bounceInOut" width="100%" height="240">](http://bl.ocks.org/mbostock/248bac3b8e354a9103c4/#bounceInOut)
