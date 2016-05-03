# 执行流程

```
new Vue()
this._init()
  this._updateRef()
  this._callHook('init')
  this._initState()
  this._initEvents()
  this._callHook('created')
  this.$mount(options.el)
```

## `this._callHook('init')`: `instance/internal/events.js`

用来触发对应钩子的方法

[选项-生命周期钩子](http://vuejs.org.cn/api/#选项-生命周期钩子)

## `this._initState()`: `instance/internal/state.js`

```
this._initProps()
this._initMeta()
this._initMethods()
this._initData()
this._initComputed()
```

## [`this.$mount()`](https://github.com/vuejs/vue/blob/v1.0.15/src/instance/api/lifecycle.js#L16): `instance/api/lifecycle.js`

获取正确的 `el`  节点，如果没有，则创建一个空 `div` 节点，然后调用 `this._compile(el)` 解析该对象。

`this.$el` 则是在 `this._compile()` 执行之后产生的。

`this._compile()` 调用了 `transclue(el)` 方法，该方法位于文件 `src/compiler/transclude.js` 中，允许在 `Vue` 实例创建之前，处理 `node/fragment`。

`this._initElement(el)` 这个方法中主要处理 `this.$el`, 如果 `el` 是 `DocumentFragment` 节点，则获取其 `firstChild` 属性。


## compileTextNode(node, options)


## `processTextToken(token, options)`

处理单个的文本记号(token)

这个方法中产生了 `descriptor` 对象，主要处理类型 `html` 和 `text` 类型的指令(Directive)。`descriptor` 对象中包含 `def` 成员，用来存储指令的处理方法。

```
{
  descriptor: {
    def: {
     bind: function() {},
     update: function() {}
    },
    exprssion: "message",
    filters: "",
    name: "text"
  },
  html: false,
  oneTime: false,
  tag: true,
  value: "message"
}
```


取得 `el.childNodes`


## 编译

[`this.$mount()`](https://github.com/vuejs/vue/blob/v1.0.15/src/instance/api/lifecycle.js) 是编译模版的入口方法，在 `this._init()` 方法中被调用。

```
this.$mount(options.el)

  > this._compile(el)

    > compileRoot(el, options, contextOptions)

    > compile(el, options, partial)

      > compileNodeList(el.childNodes, options)

        > compileNode(node, options)

        > compileElement(node, options)

          > checkTerminalDirectives

          > compileDirectives(el.attributes, options) 编译元素所有的指令，返回一个 linker

            > makeNodeLinkFn 最终调用 vm._bindDir，将指令压入到 this._directives 中

        > compileTextNode(node, options)

          > parseText(node.wholeText)

            >  processTextToken(token, options)

            >  makeTextNodeLinkFn(tokens, frag, options)

            > vm._bindDir(token.descriptor, node, host, scope)

最后把指令 `push` 到 `this._directives` 数组

vm._bindDir 中调用了 Directive 对象

this._directives 是存放 Directive 对象的数组

linkAndCapture 迭代 vm._directives 对象，调用 Directive 实例的 _bind() 方法
```

## 模版

`Vue.js` 把 `html` 当作模版

常见的 `js` 模版一般使用正则匹配的方式，生成 `html` 字符串，然后插入到页面中，细小的变动就要重新渲染有关的 `DOM` 节点。

## 数据

初始化数据 [`this._initData`](https://github.com/vuejs/vue/blob/v1.0.15/src/instance/internal/state.js#L79)

```
数据初始化 (this._initData) ->

  监听数据

  -> this._proxy(key)

  -> observe(data, this) -> defineReactive(obj, key, val)

解析模版 (this.$mount)
  > 收集指令
  > 链接模版和指令

```


## 指令


`default function Directive (descriptor, vm, el, host, scope, frag) {`

指令的构造器 `Directive`，位于 [`src/directive.js`](https://github.com/vuejs/vue/blob/v1.0.20/src/directive.js)

descriptor是指令的描述信息
