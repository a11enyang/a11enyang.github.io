---
title: what is ajax
date: 2020-04-25 12:10:52
tags:
	- ajax
---

![image-20200430093558594](https://cdn.jsdelivr.net/gh/a11enyang/Picture/img2/image-20200430093558594.png)

<!-- more -->

### 什么是Ajax

* ajax全称asynchronous javascript and xml

* 发音诶贾克斯
* web技术，用于在后台发送和检索数据的技术



> 参考链接：https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX/Getting_Started

AJAX是异步的JavaScript和XML（**A**synchronous **J**avaScript **A**nd **X**ML）。简单点说，就是使用 `XMLHttpRequest` 对象与服务器通信。 它可以使用JSON，XML，HTML和text文本等格式发送和接收数据。AJAX最吸引人的就是它的“异步”特性，也就是说他可以在不重新刷新页面的情况下与服务器通信，交换数据，或更新页面。

你可以使用AJAX最主要的两个特性做下列事：

* 在不重新加载页面的情况下发送请求给服务器。
* 接受并使用从服务器发来的数据。



### 在没有使用异步编程的情况下，浏览器是单线程执行的

只有一个main thread

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Simple synchronous JavaScript example</title>
  </head>
  <body>
    <button>Click me</button>
    <script>
      const btn = document.querySelector('button');
      btn.addEventListener('click', () => {
        let myDate;
        for(let i = 0; i < 10000000; i++) {
          let date = new Date();
          myDate = date
        }

        console.log(myDate);

        let pElem = document.createElement('p');
        pElem.textContent = 'This is a newly-added paragraph.';
        document.body.appendChild(pElem);
      });
    </script>
  </body>
</html>

```

https://mdn.github.io/learning-area/javascript/asynchronous/introducing/simple-sync.html

因为是单线程执行的，所以每个任务必须按顺序执行，所以需要等到循环执行完成之后才能进行下一步动作



### 多线程执行

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Simple synchronous JavaScript example</title>
  </head>
  <body>
    <button>Click me</button>
    <script>
      const btn = document.querySelector('button');
      const worker = new Worker('worker.js');

      btn.addEventListener('click', () => {
        worker.postMessage('Go!');

        let pElem = document.createElement('p');
        pElem.textContent = 'This is a newly-added paragraph.';
        document.body.appendChild(pElem);
      });

      worker.onmessage = function(e) {
        console.log(e.data);
      }
    </script>
  </body>
</html>
```



### 同步javascript

顺序执行代码，严格按照代码的书写顺序进行执行





### 异步编程的两种方式

#### 异步callbacks

```js
btn.addEventListener('click', () => {
  alert('You clicked me!');

  let pElem = document.createElement('p');
  pElem.textContent = 'This is a newly-added paragraph.';
  document.body.appendChild(pElem);
});
```

第一个参数是侦听的事件类型，第二个就是事件发生时调用的回调函数。.

当我们把回调函数作为一个参数传递给另一个函数时，仅仅是把回调函数定义作为参数传递过去 — 回调函数并没有立刻执行，回调函数会在包含它的函数的某个地方异步执行，包含函数负责在合适的时候执行回调函数。