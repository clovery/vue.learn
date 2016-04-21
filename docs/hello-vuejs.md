官方提供的 [10 秒种看懂 Vue.js](http://vuejs.org.cn/)。

~~~html
<div id="demo">
  <p>{{message}}</p> 2
  <input v-model="message"> 3
</div>
~~~

~~~javascript
var demo = new Vue({
  el: '#demo', // 1
  data: { // 2
    message: 'Hello Vue.js!'
  }
})
~~~

这块代码的功能：实时显示 `input` 输入框的内容。虽然只是简短的代码，却几乎包含了整个 Vue 的运转机制。下面一步步拆分其功能。

先看 `html` 中出现的标记:

1. `id="demo"`
2. `{{message}}`
3. `v-model="message"`

2 处是 Vue 的 `v-text` 指令的简写方式，同样可以写成:

~~~
<p v-text="message"></p>
~~~

3 处是 Vue `v-model` 指令，其值是一个字符串，用来告诉 Vue 需要双向绑定的数据的名字。

再看 `js` 中的代码:

1: `el: 'demo'` 是告诉 Vue 对象需要处理的节点，`el` 允许 [`CSS selectors`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector), 内部使用 `document.querySelector` 处理。

2: `data` 是存 `js` 对象，告诉 Vue 需要监听的数据成员。


以上没有很难理解的地方。
