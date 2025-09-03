# Vue 应用与根组件

由于应用程序是由组件构建，第一个被构建出来的就是根组件。
在 vue3 中可以通过以下代码进行创建：

```js
const app = Vue.createApp({
  /* 根组件选项：框架约定的属性用法。 */
});
```

由于是第一个组件，所以需要使用 mount 方法装载到 html 页面的某个 dom 节点上。

```js
const RootComponent = {
  /* 选项 */
};
const app = Vue.createApp(RootComponent);
const vm = app.mount("#app");
```

> 和 component、directive、use 等方法不同。
> mount 方法不返回 app，而是根组件实例本身（也就是无法再进行链式调用）。

[根组件实例](https://v3.cn.vuejs.org/guide/instance.html)

---

# 组件的生命周期

<div grid="~ cols-2 gap-4">
  <div>
    <p><strong>生命周期</strong>对组件化编程是非常重要的概念。
    之所以重要，是因为我们有许多任务需要在恰当的时间点执行。
    </P>
    <hr mx="2rem" class="text-accent-amber">
    <p>
    而在 MVVM 框架中，组件的创建和渲染细节都被框架接管。
    如果没有<strong class="text-red-500">生命周期钩子函数</strong>，我们难以在合适的时间点开始任务。
    </p>

```js {5-9}
Vue.createApp({
  data() {
    return { counter: 1 };
  },
  // created 生命周期钩子函数
  created() {
    // `this` 指向 vm 实例
    this.counter;
  },
});
```

[生命周期钩子](https://cn.vuejs.org/guide/essentials/lifecycle.html)

  </div>
  <div>
  <img src="/lifecycle.svg"  class="m-4 h-100 mx-auto shadow-2xl border rounded border-amber-500">
  </div>
</div>
