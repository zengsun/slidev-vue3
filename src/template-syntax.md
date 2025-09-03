# template 选项

Vue 通过 template 选项来指定模板（就是普通字符串）。
当然，这个字符串必须符合特定语法！

```js
const Hello = {
  template: "<h1>{{s}}</h1>",
  data() {
    return {
      s: "hello world",
    };
  },
};
Vue.createApp(Hello).mount("#app");
```

## 单文件组件

Vue 的单文件组件 (即 \*.vue 文件，英文 Single-File Component，简称 SFC)
是一种特殊的文件格式，使我们能够将一个 Vue
组件的模板、逻辑与样式封装在单个文件中。

[参考阅读](https://cn.vuejs.org/guide/scaling-up/sfc.html)

---

# 模版语法

> Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM
> 绑定至底层组件实例的数据。所有 Vue.js 的模板都是合法的
> HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

<div class="mt-4 text-green-500" grid="~ cols-2 gap-4">
  <div>

### 插值

1. 文本

```html
<div class="counter">Count: {{ counter }}</div>
```

2. HTML

```html
<p>使用模版语法: {{ html }}</p>
<p>使用v-html指令: <span v-html="html"></span></p>
```

3. Attribute (属性)

```html
<div v-bind:id="dynamicId"></div>
```

属性无法直接绑定，需要通过指令来进行绑定。

</div>
  <div>

### 指令

指令 (Directives) 是带有 v- 前缀的特殊 attribute。 指令 attribute 的值预期是单个
JavaScript 表达式。 当然咯，v-for 和 v-on 是例外情况

1. 内置指令和自定义指令
2. 指令名称 + 参数(动态) + 修饰符

```html
<a v-bind:id="url" v-on:click.prevent="onClick">...</a>
```

3. 缩写

```html
<a :id="url" @click.prevent="onClick">...</a>
```

</div>
</div>

---

# Class 与 Style 绑定

数据绑定的一个常见需求场景是操纵元素的 CSS _class_ 列表和内联样式。 因为 class
和 style 都是 _attribute_， 我们可以和其他 _attribute_ 一样使用 v-bind
将它们和动态的字符串绑定。

然而，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。 因此，Vue
专门为 class 和 style 的 v-bind 用法提供了**特殊的功能增强**。
除了字符串外，表达式的值也可以是**对象或数组**。

<div class="flex">
  <div class="h-80 overflow-y-scroll">
```vue
<template>
  <div>
    <div :class="{ active: isActive, error: hasError }">Active</div>
    <button @click="isActive = !isActive">isActive</button>
    <button @click="hasError = !hasError">hasError</button>
</div>
</template>
<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: false
    }
  }
}
</script>
<style>
.active {
  color: blue;
}
.error {
  background: red;
}
</style>
```
  </div>
  <div data-theme="light" class="px-4">
    <StyleBind />

[参考阅读](https://cn.vuejs.org/guide/essentials/class-and-style.html)

</div>
</div>

---

# 条件渲染

## v-if \ v-else \ v-else-if 指令

v-if
指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回真值时才被渲染。
v-else 为 v-if 添加一个“else 区块”。 
v-else-if 提供的是相应于 v-if 的“else if 区块”。它可以连续多次重复使用。

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```

---

# 内置特殊元素\<template>上使用 v-if

因为 v-if
是一个指令，他必须依附于某个元素。但如果我们想要切换不止一个元素呢？在这种情况下我们可以在一个
\<template> 元素上使用
v-if，这只是一个不可见的包装器元素，最后渲染的结果并不会包含这个 \<template>
元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

v-else 和 v-else-if 也可以在 \<template> 上使用。

## v-show 指令

v-show 指令和 v-if 指令使用完全相同。不同之处在于 v-show 会在 DOM
渲染中保留该元素；v-show 仅切换了该元素上名为 display 的 CSS 属性。

---

# 列表渲染

## v-for 指令

可以使用 v-for 指令基于一个数组来渲染一个列表。 v-for 指令的值需要使用 **item in
items** 形式的特殊语法，其中 items 是源数据的数组。

```js
data() {
  return {
    items: [{ message: 'Foo' }, { message: 'Bar' }]
  }
}
```

```html
<li v-for="item in items">{{ item.message }}</li>
```

[参考阅读](https://cn.vuejs.org/guide/essentials/list.html)

---

# v-for 指令需要注意的点

<div class="flex">
  <div class="w-1/2 mr-8">

1. 不仅可以迭代数组，也可以是对象或者数值。
2. 和 v-if 一样可以使用在\<template>元素上
3. 同时使用 v-if 和 v-for 是不推荐的，因为这样二者的优先级不明显。
4. 强烈建议使用 key 属性！
5. 在使用计算属性时，不要直接使用 reverse 和 sort 方法！

</div>
  <div class="w-1/2">
    <Todos class="w-full" />

[参考阅读](https://cn.vuejs.org/guide/essentials/list.html#v-for-with-a-component)

</div>
</div>
