---
title: vue.js
date: 2020-04-23 09:20:49
tags:
	- vue.js
---

```html
<script src = "<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>"></script>
<div id="app">
  <input type="text" v-on:input="changeTitle">
  <p>
    { { title } }
  </p>
</div>

new Vue ({
	ek:"#app",
	data: {
	title: "Hello World",
},
	methods: {
		changeTitle: function(event) {
			this.title = event.target.value;
}
}
});
```

理解：html中的代码相当于是前置条件，js代码相当于是后置条件；在html代码的标签中指明要执行的动作，vue引擎会知道你想要干什么。

<!-- more -->

### 5

![image-20200423093540542](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200423093540542.png)

课程介绍



### 6

布置本地的开发环境





### 理解vue.js模板

vue（vue实例）=基于html代码生成一个模板，然后在内部存储，然后使用设个模板来生成真正的html代码，然后将它渲染成为dom。生成的模板可以访问vue实例的属性，但是js代码不可以。

```js
<script src = "<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>"></script>
<div id="app">
  <input type="text" v-on:input="changeTitle">
  <p>
    { { sayHello() } }
  </p>
</div>

new Vue ({
	el:"#app",
	data: {
	title: "Hello World",
},
	methods: {
		sayHello: function() {
			return title;
	}
}
});
```

上述的做法是不可行的



### 在vue实例中得到数据

```js
new Vue ({
	el:"#app",
	data: {
	title: "Hello World",
},
	methods: {
		sayHello: function() {
			return title;
	}
}
//这种方式是不可行的
```

vue给我们提供一些方便，可以使用`this.title`来进行访问



### 与属性链接

一般是不能在html标签的属性中使用{ {   } }，vue只能解析在标签之间的大括号

```html
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<div id="app">
  <p>
    { { title } }
  </p>
  <a v-bind:href="link">google</a> <!-- v-bind已经声明在模板语言中了，所以不需要再加大括号 -->
</div>
```

```
new Vue({
	el: "#app",
  data: {
  	title: "你好世界",
    link: "https://cn.vuejs.org/v2/guide/"
  },
  methods: {
  	sayHello: function() {
    	return this.title;
    }
  }
});
```

v-bind 动态绑定了数据

### 

### 理解和使用指令

v-bind接受一个参数，一般情况下，使用冒号将参数传递给指令

指令告诉vue绑定一些东西在我数据上，要什么东西呢，就通过冒号之后的参数来告诉



### 避免重复渲染

v-once 告诉vue该属性只会渲染一次	



### 输出html代码并渲染

v-html



### 练习

```html
new Vue({
	el: "#exercise",
  data: {
  	name: "yangweidong",
    age: 21,
    link: "https://sites.psu.edu/siowfa16/files/2016/10/YeDYzSR-10apkm4.png",
  },
  methods: {
  	ageTripple: function() {
    	return (this.age * 3);
    },
    random01: function() {
    	return Math.random();
    },
    changeName: function(event) {
    	this.name = event.target.value;
    },
    
  },
});

<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="exercise">
   <!-- 1) Fill the <p> below with your Name and Age - using Interpolation -->
    <p>VueJS is pretty cool - { { name } } ({ { age } })</p>
    <!-- 2) Output your age, multiplied by 3 -->
    <p>{ { ageTripple() } }</p>
    <!-- 3) Call a function to output a random float between 0 and 1 (Math.random()) -->
    <p v-once>{ { random01() } }</p>
    <!-- 4) Search any image on Google and output it here by binding the "src" attribute -->
    <div>
        <img style="width:100px;height:100px" v-bind:src="link">
    </div>
    <!-- 5) Pre-Populate this input with your name (set the "value" attribute) -->
    <div>
        <input type="text" v-on:input="changeName">
    </div>
</div>
```



### 监听事件

v-on 监听原HTML文件的时间，在模板文件中做出改变

v-bind 将标签属性与模板文件进行绑定，以便可以使用模板文件的属性

<img src="https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200423111258038.png" alt="image-20200423111258038" style="zoom:50%;" />

```javascript
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="app">
  <button v-on:click="increase()">click me!</button>
  <p> { { counter } }</p>
</div>
```

```html
new Vue({
	el: "#app",
  data: {
  	counter: 0,
  },
  methods: {
  	increase: function() {
    	this.counter++;
    }
  },
});
```





### 从事件中获取数据

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="app">
  <button v-on:click="increase()">click me!</button>
  <p> { { counter } }</p>
  <p v-on:mousemove="update">{ { x } } / { { y } } </p>
</div>
```

```js
new Vue({
	el: "#app",
  data: {
  	counter: 0,
    x: 0,
    y: 0,
  },
  methods: {
  	increase: function() {
    	this.counter++;
    },
    update: function(event) {
    	this.x = event.x;
      this.y = event.y;
    },
  },
});
```



### 修改一个事件

```
v-on:mousemove.stop
```



### 监听键盘事件

```js
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="app">
  <button v-on:click="increase()">click me!</button>
  <p> { { counter } }</p>
  <p v-on:mousemove="update">{ { x } } / { { y } } <span v-on:mousemove.stop>stsdfasdfasfop</span> </p>
  <input type="text" v-on:keyup.enter="alertMe">
  
</div>
```



### 在模板文件中写js

任何使用vue.js的地方都可以直接写js代码

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="app">
  <button v-on:click="counter++">click me!</button>
  <p> { { counter } }</p>
  <p v-on:mousemove="update">{ { x } } / { { y } } <span v-on:mousemove.stop>stsdfasdfasfop</span> </p>
  <input type="text" v-on:keyup.enter="alertMe">
  
</div>
```



### 数据的双向绑定

v-model

```
<input type="text" v-model="counter">
```



### computed属性

```js
<script src="https://unpkg.com/vue/dist/vue.js"></script>

<div id="app">
  <button v-on:click="counter++">增加!</button>
  <button v-on:click="counter--">减少!</button>
  <button v-on:click="secondCounter++">第二个按钮</button>
  <p>{ { counter } }</p>
  <p> { { result() } } </p>
  <p>{ { secondCounter } }</p>
</div>


new Vue({
 el: "#app",
 data: {
 		counter: 0,
    secondCounter: 0,
 },
 methods: {
 		result: function() {
    	console.log("11111111");
    	return this.counter > 5 ? "counter 大于 5" : "counter 小于 5";
    }
 },
});
```

如果是vue中的数据发生改变，并且这个数据是显示在外面的，所以vue会重新创新页面，导致vue中的方法被重新执行一次；那么如何保证定义的方法不会重新执行呢，那么就可以将该函数放置在computed中

computed中的函数使用就像是属性一样

computed相当于提供了一个缓存机制，如果想要使用缓存的话，那么使用computed；如果不想使用缓存的话，那么使用method

computed：依赖属性，如果依赖的内容没有发生改变的话，不会执行；如果依赖的内容发生了改变的话， 会执行

根据数据元数据的值进行计算然后显示在页面上

### watch

computed的属性总是同步执行的，有时候需要异步执行。当监视的属性发生改变的时候，会调用这个方法。



### 命令的简写

v-on:   ==  @

v-bind: == :



### 与CSS类的动态链接

<img src="/Users/austin/Library/Application Support/typora-user-images/image-20200423165530999.png" alt="image-20200423165530999" style="zoom:50%;" />





### v-if

v-if   v-else





### v-show



### v-for



### vue组件







### vue过滤器

```html
<div id="app">
  { { message | toupper } }  <!-- 有点类似于bash的语法 -->
</div>
```

```js
<script>
  new Vue({
  el: "#app",
  data: {
    message: "helloworld",
  },
  filter: {
    toupper: function(value) {
      return value.toUpperCase();
    }
  }
});
<script/>
```



### vue组件

![](https://cdn.jsdelivr.net/gh/a11enyang/Picture@1.0/img2/image-20200427161752238.png)

