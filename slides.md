---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: ./background.jpeg
# apply any windi css classes to the current slide
class: "text-center"
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# Learn Vue.js

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-50">
    开始学习 <carbon:arrow-right class="inline"/>
  </span>
</div>

<style>
h1 {
  background: rgb(2,0,36);
  background: linear-gradient(90deg, rgba(21, 120, 60, 1) 0%, rgba(90,190,140,1) 35%, rgba(0,212,255,1) 100%);
  background-size: 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# What is Vue?

Vue 是由 JavaScript (TypeScript) 实现的 Web 前端框架。
Vue 的核心库只关注 _视图层_，不仅易于上手，还便于与第三方库或既有项目整合。
Vue 的实现思想深受 React 框架的影响，也是一个 **MVVM** 框架。


Vue (发音为 /vjuː/，类似 view) 是一款用于构建用户界面的 _JavaScript_ 框架。
它基于标准 HTML、CSS 和 JavaScript 构建，
并提供了一套**声明式**的、**组件化**的编程模型，帮助你高效地开发用户界面。
无论是简单还是复杂的界面，Vue 都可以胜任。

## 两个核心功能

* 声明式渲染：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系。
* 响应性：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM。

---

# Why ?

- **易于上手** - 于 MVVM 的前辈们 React、Angular 相比，更加容易使用。
- **组件化** - 得益于组件化的编程。不仅方便我们提高代码的复用，也可以方便的使用第三方组件。
- **工程化** - 使用 vue 的脚手架工具可以方便的搭建前端工程环境。
- **良好的性能** - vue 在不必精心优化的情况下，依然有比较良好的性能。
- **生态完备** - 虽然 React 具备世界上最大的生态环境，但在功能完备程度上 Vue 完全不输。

<br>

扩展阅读 ：[vue 介绍](https://cn.vuejs.org/guide/introduction.html)

<style>

</style>

---

# MVVM - 新一代的框架模型

**MVVM** 是 M（_Model_：模型）-V（_View_：视图）-VM（_ViewModel_：视图模型）的缩写形式， 
代表了一种全新的开发思想。 是第三代 Web 前端框架的标志特征！

<img src="/mvvm.png"  class="m-4 h-80 mx-auto rounded-xl shadow-2xl">

---

# MVVM - 声明式渲染

HTML 负责渲染显示 View，更加确切的讲在 Vue 中叫**模版**(template)。

```html
<div id="counter">Counter: {{ counter }}</div>
```

javascript 控制逻辑，data()中就是 Model。

```js {all|2,6|3-5|all}
const Counter = {
  data() {
    return {
      counter: 0,
    };
  },
};

Vue.createApp(Counter).mount("#counter");
```

---
layout: two-cols
---

# MVVM - 事件处理

<div class="py-4">

```html {all|3|all}
<div id="counter">
  Counter: {{ counter }}
  <button v-on:click="add(1)">+</button>
</div>
```

```js {all|7-11|9|all}
const Counter = {
  data() {
    return {
      counter: 0,
    };
  },
  methods: {
    add(val) {
      this.counter += val;
    },
  },
};

Vue.createApp(Counter).mount("#counter");
```

</div>

::right::

<div class="p-4 ">
  <h4 class="text-base mb-4">看看效果我们可是没有写更新 DOM 代码</h4>
  <Counter/>
</div>

---

# 快速上手

1. 线上尝试
2. 创建工程
3. 通过 CDN 在页面导入

```vue
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<div id="app">{{ message }}</div>

<script>
  const { createApp } = Vue
  
  createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  }).mount('#app')
</script>
```

[参考阅读](https://cn.vuejs.org/guide/quick-start.html)

---

# MVVM - 组件化应用构建

组件化编程也是重要特征。虽然组件化并不是 MVVM 的典型特征，但对于整体框架而言 也是至关重要的概念。

<img src="/components.png"  class="m-4 mx-auto shadow-2xl">

---
src: ./src/root-component.md
---

---
src: ./src/data-methods.md
---

---
src: ./src/computed.md
---

---
src: ./src/watchers.md
---

---
src: ./src/template-syntax.md
---

---
src: ./src/event-handling.md
---

---
src: ./src/form.md
---

---
src: ./src/template-refs.md
---

---
src: ./src/component-basic.md
---

---
src: ./src/components.md
---

---
src: ./src/reusability.md
---

