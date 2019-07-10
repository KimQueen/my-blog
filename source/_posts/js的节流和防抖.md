---
title: js的节流和防抖
categories: 性能
tag:
  - JavaScript
  - 基础
toc: true
thumbnail: "/images/train.jpg"
---
#### 概念
- 防抖： 任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行。
- 节流：指定时间间隔内只会执行一次任务。
<!--more-->
#### 实现方式
防抖的实现方式：
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>Page Title</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
  </head>
  <body>
    <button id="dobounce">点我防抖</button>
    <button id="throttle">点我节流！</button>
    <script>
      window.onload = function() {
        // 进行 防抖
        var myDebounce = document.getElementById("dobounce");
        myDebounce.addEventListener("click", dobounce(saydoDounce));
        // 进行 节流
        var myThrottle = document.getElementById("throttle");
        myThrottle.addEventListener("click", throttle);
      };

      // 防抖
      function dobounce(fn) {
        let timeout = null;
        return function() {
          clearTimeout(timeout);
          timeout = setTimeout(() => {
            // fn();
            // console.log(this)
            fn.call(this);
          }, 1000);
        };
      }
      function saydoDounce() {
        console.log(this);
        console.log("防抖成功");
      }

      // 方法一
      // 节流
      // function throttle(fn){
      //     let canRun = true;
      //     return function(){
      //         if(!canRun){ // 在函数开头判断标志是否为 true，不为 true 则中断函数
      //             return;
      //         }
      //         canRun = false;
      //         setTimeout(()=>{
      //             fn.call(this);
      //             canRun = true;
      //         },2000)
      //     }
      // }


      // 方法二
      let canRun = true;

      function throttle() {
        //   console.log(canRun)
          if (!canRun) {
            // 在函数开头判断标志是否为 true，不为 true 则中断函数
            return;
          }
          canRun = false;
          setTimeout(() => {
            sayThrottle();
            canRun = true;
          }, 5000);
      }

      function sayThrottle() {
        console.log("节流成功");
      }
    </script>
  </body>
</html>

```
防抖的代码：这件事儿需要等待，如果你反复催促，我就重新计时！
节流的代码：固定时间内只是执行一次

#### 应用场景

*debounce*

- search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
- window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次


*throttle*

- 鼠标不断点击触发，mousedown(单位时间内只触发一次)
- 监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断
