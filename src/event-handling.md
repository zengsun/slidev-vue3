# 事件处理

在 _DOM_ 上注册事件监听，并在事件触发时执行对应的函数我们已经熟悉！
在 **Vue** 中，我们可以使用 v-on 指令 (简写为 @) 来监听 _DOM_ 事件。

> 用法：v-on:click="handler" 或 @click="handler"。

事件处理器 (handler) 的值可以是：

1. 内联事件处理器：事件被触发时执行的内联 JavaScript 语句 (与 onclick 类似)。
2. 方法事件处理器：一个指向组件上定义的方法的属性名或是路径。

---

# 内联事件处理器

内联事件处理器通常用于简单场景，例如：

```js
data() {
  return {
    count: 0
  }
}
```

```html {1}
<button @click="count++">Add 1</button>
<p>Count is: {{ count }}</p>
```

当处理逻辑比较简单时，我们就不必定义处理函数，为命名发愁了！

---

# 方法事件处理器​

随着事件处理器的逻辑变得愈发复杂，内联代码方式变得不够灵活。
因此 v-on 也可以接受一个方法名或对某个方法的调用。

```js
data() {
  return {
    name: 'Vue.js'
  }
},
methods: {
  greet(event) {
    // 方法中的 `this` 指向当前活跃的组件实例
    alert(`Hello ${this.name}!`)
    // `event` 是 DOM 原生事件
    if (event) {
      alert(event.target.tagName)
    }
  }
}
```

```html
<!-- `greet` 就是定义过的方法名，直接绑定就可以！ -->
<button @click="greet">Greet</button>
```

[参考阅读](https://cn.vuejs.org/guide/essentials/event-handling.html)

---

# 事件修饰符

在处理事件时调用 event.preventDefault() 或 event.stopPropagation() 是很常见的。
尽管我们可以直接在方法内调用，但如果方法能更专注于数据逻辑而不用去处理 DOM 事件的细节会更好。

Vue 为 v-on 提供了以下事件修饰符：

* .stop    停止事件冒泡
* .prevent 阻止默认事件
* .self    只处理自己触发的事件
* .capture 捕获事件
* .once    只触发一次
* .passive 滚动事件

---

# 按键修饰符​

在监听键盘事件时，我们经常需要检查特定的按键。Vue 允许在 v-on 或 @ 监听按键事件时添加按键修饰符。

```html
<!-- 仅在 `key` 为 `Enter` 时调用 `submit` -->
<input @keyup.enter="submit" />
<!-- 你可以直接使用 KeyboardEvent.key 暴露的按键名称作为修饰符，但需要转为 kebab-case 形式。 -->
<input @keyup.page-down="onPageDown" />
```

## 按键别名

> .enter .tab .delete .esc .space .up .down .left .right

## 系统按键修饰符

> .ctrl .alt .shift .meta

```html
<!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />
<!-- Ctrl + 点击 -->
<div @click.ctrl="doSomething">Do something</div>
```

## .exact 修饰符​
.exact 修饰符允许控制触发一个事件所需的确定组合的系统按键修饰符。
