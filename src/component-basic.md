# 组件基础​
组件允许我们将 UI 划分为独立的、可重用的部分，并且可以对每个部分进行单独的思考。在实际应用中，组件常常被组织成层层嵌套的树状结构：

<img src="/components.basic.png" class="m-4 h-70 mx-auto rounded-xl shadow-2xl">

这和我们嵌套 HTML 元素的方式类似，Vue 实现了自己的组件模型，使我们可以在每个组件内封装自定义内容与逻辑。

---

# 定义组件

使用单文件组件（SFC）是最简单的方式：
```vue {all|1-9|10-12}
<script>
export default {
  data() {
    return {
      count: 0
    }
  }
}
</script>
<template>
  <button @click="count++">You clicked me {{ count }} times.</button>
</template>
```

---

# 使用组件

要使用一个子组件，我们需要在父组件中导入它。
假设我们把计数器组件放在了一个叫做 ButtonCounter.vue 的文件中，
这个组件将会以默认导出的形式被暴露给外部。

```vue {all|2,5-7|13}
<script>
import ButtonCounter from './ButtonCounter.vue'

export default {
  components: {
    ButtonCounter
  }
}
</script>

<template>
  <h1>Here is a child component!</h1>
  <ButtonCounter />
  <button-counter />
</template>
```

<v-clicks>
<click-times />
<click-times />
</v-clicks>

---

# 传递 props

Props 是一种特别的 attributes，你可以在组件上声明注册。

```vue {3|8}
<script>
export default {
  props: ['title']
}
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```

当一个 prop 被注册后，可以像这样以自定义 attribute 的形式传递数据给它：

```html
<BlogPost title="My journey with Vue" />
```

一个组件可以有任意多的 props，默认情况下，所有 prop 都接受任意类型的值。

---

# 监听事件

一个组件除了属性以外，还需要可以处理事件监听（和普通的 HTML 元素一样）！
这个模式是：在父组件中对子组件注册监听（事件处理器）。当子组件发生事件时，则父组件的事件监听被执行！

比如：之前的按钮的 @click 代码，当然原生的 HTML 元素的事件触发是浏览器已经处理了！
```html
<button @click="count++"> plus one </button>
```

而自定义组件的事件触发，则可以通过 this.$emit 来触发！

```js
// 子组件中调用 $emit
this.$emit("click")
```

```html
<!-- 父组件 -->
<子组件 @click="事件处理器" />
```

_演练一下_ 

<event-parent />

---

# 定义事件

我们可以通过 emits 选项来声明需要抛出的事件：

```vue {4}
<script>
export default {
  props: ['title'],
  emits: ['enlarge-text']
}
</script>

```

这声明了一个组件可能触发的所有事件，还可以对事件的参数进行验证。
同时，这还可以让 Vue 避免将它们作为原生事件监听器隐式地应用于子组件的根元素。


---

# 通过插槽来分配内容

一些情况下我们会希望能和 HTML 元素一样向组件中传递内容（标签体）：
```html
<AlertBox>
  Something bad happened.
</AlertBox>
```

效果：

<AlertBox>
  Something bad happened.
</AlertBox>

```vue {4}
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot />
  </div>
</template>
```
