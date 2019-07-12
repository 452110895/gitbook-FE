# 函数节流&函数防抖

函数防抖\(**debounce**\)

概念： 在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

生活中的实例： 如果有人进电梯（触发事件），那电梯将在10秒钟后出发（执行事件监听器），这时如果又有人进电梯了（在10秒内再次触发该事件），我们又得等10秒再出发（重新计时）。

对于函数防抖，有以下几种应用场景：

* 给按钮加函数防抖防止表单多次提交。
* 对于输入框连续输入进行AJAX验证时，用函数防抖能有效减少请求次数。
* 判断scroll是否滑到底部，滚动事件+函数防抖

函数防抖：（用到了闭包）

=============================================================

function debounce\(fn, wait\) {

  var timer = null;

  return function \(\) {

      var context = this

      var args = arguments

      if \(timer\) {

          clearTimeout\(timer\);

          timer = null;

      }

      timer = setTimeout\(function \(\) {

          fn.apply\(context, args\)

      }, wait\)

  }

}

var fn = function \(\) {

  console.log\('boom'\)

}

setInterval\(debounce\(fn,500\),1000\) // 第一次在1500ms后触发，之后每1000ms触发一次

setInterval\(debounce\(fn,2000\),1000\) // 不会触发一次（我把函数防抖看出技能读条，如果读条没完成就用技能，便会失败而且重新读条）

=============================================================

函数节流\(**throttle**\)

概念： 规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。

生活中的实例： 我们知道目前的一种说法是当 1 秒内连续播放 24 张以上的图片时，在人眼的视觉中就会形成一个连贯的动画，所以在电影的播放（以前是，现在不知道）中基本是以每秒 24 张的速度播放的，为什么不 100 张或更多是因为 24 张就可以满足人类视觉需求的时候，100 张就会显得很浪费资源。

对于函数节流，有如下几个场景：

* 游戏中的刷新率
* DOM元素拖拽
* Canvas画笔功能

函数节流：（用到了闭包）

=============================================================

function throttle\(fn, gapTime\) {

  let \_lastTime = null;

  return function \(\) {

    let \_nowTime = + new Date\(\)

    if \(\_nowTime - \_lastTime &gt; gapTime \|\| !\_lastTime\) {

      fn\(\);

      \_lastTime = \_nowTime

    }

  }

}

let fn = \(\)=&gt;{

  console.log\('boom'\)

}

setInterval\(throttle\(fn,1000\),10\)

=============================================================

参考资料：

轻松理解**JS**函数节流和函数防抖

[https://juejin.im/post/5a35ed25f265da431d3cc1b1](https://juejin.im/post/5a35ed25f265da431d3cc1b1)

