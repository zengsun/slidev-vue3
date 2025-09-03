
# 自定义指令

除了 Vue 内置的一系列指令 (比如 _v-model_ 或 _v-show_) 之外，
Vue 还允许你注册自定义的指令 (Custom Directives)。

<div class="flex">
  <div class="w-1/2 pr-2">

```js
const focus = {
  mounted: (el) => el.focus()
}

export default {
  directives: {
    // 在模板中启用 v-focus
    focus
  }
}
```

```html
<input v-focus />
```

  </div>
  <div class="w-1/2">

```js
const myDirective = {
  // 在绑定元素的 attribute 前或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```

  </div>
</div>

---

# 插件

插件 (Plugins) 是一种能为 Vue 添加全局功能的工具代码。

```js
import { createApp } from 'vue'

const app = createApp({})

app.use(myPlugin, {
  /* 可选的选项 */
})
```

一个插件可以是一个拥有 _install()_ 方法的对象，
也可以直接是一个安装函数本身。
安装函数会接收到安装它的应用实例和传递给 _app.use()_ 的额外选项作为参数：

```js
const myPlugin = {
  install(app, options) {
    // 配置此应用
  }
}
```

---

插件没有严格定义的使用范围，但是插件发挥作用的常见场景主要包括以下几种：
1. 通过 _app.component()_ 和 _app.directive()_ 注册一到多个全局组件或自定义指令。
2. 通过 _app.provide()_ 使一个资源可被注入进整个应用。
3. 向 _app.config.globalProperties_ 中添加一些全局实例属性或方法
4. 一个可能上述三种都包含了的功能库 (例如 _vue-router_ )。