# 模板引用​

虽然 Vue 的声明性渲染模型为你抽象了大部分对 DOM 的直接操作，但在某些情况下，我们仍然需要直接访问底层 DOM 元素。要实现这一点，我们可以使用特殊的 ref attribute：

```html
<input ref="input">
```

ref 是一个特殊的 attribute，和 v-for 章节中提到的 key 类似。
它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用。
这可能很有用，比如说在组件挂载时将焦点设置到一个 input 元素上，或在一个元素上初始化一个第三方库。

**挂接完成后**，就可以通过 <code>this.$refs.input</code> 来获得元素或组件的引用。

```js {2-4}
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
```

---

# v-for 中的模板引用

在 v3.2.25 及以上版本 <code>v-for</code> 中使用模板引用时，
相应的引用中包含的值是一个数组：

```vue
<script>
export default {
  data() {
    return {
      list: [ /* ... */ ]
    }
  },
  mounted() {
    console.log(this.$refs.items)
  }
}
</script>
<template>
  <ul>
    <li v-for="item in list" ref="items">
      {{ item }}
    </li>
  </ul>
</template>
```

---

# 函数模板引用

除了使用字符串值作名字，ref attribute 还可以绑定为一个函数，
会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：

```html
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }" />
```

注意我们这里需要使用动态的 :ref 绑定才能够传入一个函数。
当绑定的元素被卸载时，函数也会被调用一次，此时的 el 参数会是 null。
你当然也可以绑定一个组件方法而不是内联函数。