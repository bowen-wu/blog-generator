---
title: Tween.js
date: 2018-01-11 19:58:39
tags: Plugin
---
tweenjs 是使用 JavaScript 中的一个简单的补间动画库，支持数字、对象的属性和 CSS 样式属性的赋值。

tweenjs 以平滑的方式修改元素的属性值，需要传递给 tween 要修改的值、动画结束时的最终值和动画花费时间（duration），之后 tween 引擎就可以计算从开始动画点到结束动画点之间值，从而产生平滑的动画效果。
# 使用
- tweenjs CDN 
    ```
    https://cdnjs.cloudflare.com/ajax/libs/tween.js/17.1.1/Tween.min.js
    ```
- tweenjs [下载到本地](https://pan.baidu.com/s/1nvoXev3)，密码是 ` ywoq `。

- [tweenjs github](https://github.com/tweenjs/tween.js)

- tweenjs 实例
    ```
    var box = document.createElement('div');
    box.style.setProperty('background-color', '#008800');
    box.style.setProperty('width', '100px');
    box.style.setProperty('height', '100px');
    document.body.appendChild(box);
    
    function animate(time) {
        requestAnimationFrame(animate);
        TWEEN.update(time);
    }
    requestAnimationFrame(animate);

    var coords = { x: 0, y: 0 };
    var tween = new TWEEN.Tween(coords) 
            .to({ x: 300, y: 200 }, 1000) 
            .easing(TWEEN.Easing.Quadratic.Out) 
            .onUpdate(function() { 
                box.style.setProperty('transform', 'translate(' + coords.x + 'px, '     + coords.y + 'px)');
            })
            .start(); 
    ```

# 示例说明
1. 假设有一个对象 position ，它的坐标为 x 和 y
```
var position = { x: 100, y: 0 }
```
2. 改变 x 的值从 100 到 200 ，持续时间为 1s
```
var tween = new TWEEN.Tween(position)
tween.to({x: 200}, 1000)
```
3. 创建 tween 对象后，激活它，从而让它开始动画
```
tween.start();
```
4. 为了平滑的动画效果，需要在同一个循环动画中调用 TWEEN.update 方法
```
animate();
function animate(){
    requestAnimationFrame(animate);
    TWEEN.update();
}
```
这个动作将会更新所有被激活的 tweens ，在 1s 内 position.x 将变为 200 。
5. 可以使用 onUpdate 回调函数将结果打印到控制台上
```
tween.onUpdate(function(){
    console.log( this.x );
})
```
这个函数在每次 tween 被更新时都会被调用

# tweenjs 动画
Tween.js 本身不会运行，你需要通过 update 方法明确告诉它什么时候开始运行，推荐在动画主循环中使用该动画，可以调用 ` requestAnimationFrame ` 方法来获得良好的图像性能

使用无参数的调用方法，update 将明确当前时间。也可以为 update 方法法明确一个时间。
```
TWEEN.update(100);
```
update 的时间为 100毫秒 ，可以使用这种方法来明确代码中所有随时间变化的函数。

# 控制 tween 动画
- ` start ` 和 ` stop ` ==> ` Tween.start ` 和 ` Tween.stop ` 分别用来控制 tween 动画的开始和结束
对于已经结束和没有开始的动画，Tween.stop 方法不起作用。 Tween.start 可以方法接收一个时间参数，若使用了该参数，tween 动画将在延迟该时间数后才开始动画，否则他将立刻开始动画。

- ` update ` ==> 通过 ` TWEEN.update ` 方法执行动画的更新

- ` chain ` ==> 制作多个多行，例如一个动画在另一个动画结束后开始，可以通过 ` chain ` 来实现
    ```
    tweenA.chain(tweenB);  //tweenB 在 tweenA 之后开始动画，故可以制作一个无线循环的动画
    tweenB.chain(tweenA);
    ```
- ` repeat ` ==> 制作循环动画，优于 ` chain `，接收一个用于描述循环次数的参数
    ```
    tween.repeat(10);
    tween.repeat(infinity);
    ```

- ` delay ` ==> 用于控制动画之间的延迟
    ```
    tween.delay(1000);
    tween.start()
    ```

# 回调函数
可以在每次 tween 循环周期的指定时间点调用自定义的函数
- ` onStart ` ==> tween 动画开始前的回调函数
- ` onStop ` ==> tween 动画结束后的回调函数
- ` onUpdate ` ==> 在 tween 动画每次更新后执行
- ` onComplete ` ==> 在 tween 动画全部结束后执行

```
var tween = new TWEEN.Tween({

}).to({

}).onStart(function(){

}).onUpdate(function(){

})
```

# 动画运动算法（缓动函数）easing函数
- Linear ==> 线性匀速运动效果
- Quadratic ==> 二次方的缓动（t^2）
- Cubic ==> 三次方的缓动（）
- Quartic ==> 四次方的缓动（）
- Quintic ==> 五次方的缓动
- Sinusoidal ==> 正弦曲线的缓动（）
- Exponential ==> 指数曲线的缓动（）
- Circular ==> 圆形曲线的缓动（）
- Elastic ==> 指数衰减的正弦曲线缓动（）
- Back ==> 超过范围的三次方的缓动
- Bounce ==> 指数衰减的反弹缓动

# 缓动方式（效果）easing类型
- easeIn（In） ==> 加速，先慢后快
- easeOut（Out） ==> 减速，先快后慢
- easeInOut（InOut） ==> 前半段加速，后半段减速

# 使用公式
 ```
 .easing(TWEEN.Easing.easing函数.easing类型)
```

# tweenjs 源码
![](http://upload-images.jianshu.io/upload_images/9617841-82e90227e99697b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![控制动画方法](http://upload-images.jianshu.io/upload_images/9617841-8846f1fe98dec1d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![缓动函数](http://upload-images.jianshu.io/upload_images/9617841-5db818a1c9c6e0f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/9617841-c9678e2fab0b0c59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)