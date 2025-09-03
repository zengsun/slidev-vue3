# 组件 注册

一个 Vue 组件在使用前需要先被“注册”，这样 Vue 才能在渲染模板时找到其对应的实现。
组件注册有两种方式：全局注册和局部注册。

<div class="flex flex-row">
<div class="w-1/2 pr-2">

## 全局注册

我们可以使用 Vue 应用实例的 app.component() 方法，让组件在当前 Vue
应用中全局可用。

```js {all|1|3|5}
import MyComponent from "./App.vue";

app
  .component("MyComponent", MyComponent)
  // 可以链式调用
  .component("ComponentA", ComponentA);
```

</div>
<div class="w-1/2">

## 局部注册

局部注册需要使用 components 选项：

```vue {all|2,5-7|12}
<script>
import ComponentA from "./ComponentA.vue";

export default {
  components: {
    ComponentA,
  },
};
</script>

<template>
  <ComponentA />
</template>
```

</div>
</div>

---

# 组件 props

一个组件需要显式声明它所接受的 props，这样 Vue 才能知道外部传入的哪些是 props，
哪些是透传 attribute（关于 _透传 attribute_ ，以后会讨论）。

props 需要使用 _props_ 选项来定义：

```js
export default {
  props: {
    title: String,
    likes: Number,
  },
};
```

props 除了可以接受数值以外，也可以接受对象（实际上这才是常用的）。

---

## 静态 vs. 动态 Prop

至此，你已经见过了很多像这样的静态值形式的 props：

```html
<BlogPost title="My journey with Vue" />
```

相应地，还有使用 v-bind 或缩写 : 来进行动态绑定的 props：

```html
<!-- 根据一个变量的值动态传入 -->
<BlogPost :title="post.title" />

<!-- 根据一个更复杂表达式的值动态传入 -->
<BlogPost :title="post.title + ' by ' + post.author.name" />
```

<AlertBox v-click>

需要注意除了 String 可以使用静态传值， 其他类型都不可以（不能省略 **v-bind
及其缩写** ）！

</AlertBox>

<div v-after>

### 使用一个对象绑定多个 prop

```html
<BlogPost v-bind="post" />
等价于
<BlogPost :id="post.id" :title="post.title" />
```

</div>

---

# 单向数据流

所有的 props 都遵循着单向绑定原则，props 因父组件的更新而变化，
**自然地将新的状态向下流往**子组件，而不会逆向传递。
这避免了子组件意外修改父组件的状态的情况，不然应用的数据流将很容易变得混乱而难以理解。

**即数据流永远自上（父组件）而下（子组件）！**

<AlertBox v-click>

基于单向数据流的原则，你永远不应该在组件内部修改任何 props 的值！
**如果一定要更改，则遵守谁提供，谁修改的原则！** props, provide/inject,
全局状态都在此列！

</AlertBox>

<div v-after>

_演练一下_

</div>

---

## props 校验

Vue 组件可以更细致地声明对传入的 _props_ 的校验要求。

<div class="h-80 overflow-y-scroll">
```js
export default {
  props: {
    // 基础类型检查
    //（给出 `null` 和 `undefined` 值则会跳过任何类型检查）
    propA: Number,
    // 多种可能的类型
    propB: [String, Number],
    // 必传，且为 String 类型
    propC: {
      type: String,
      required: true
    },
    // Number 类型的默认值
    propD: {
      type: Number,
      default: 100
    },
    // 对象类型的默认值
    propE: {
      type: Object,
      // 对象或者数组应当用工厂函数返回。
      // 工厂函数会收到组件所接收的原始 props
      // 作为参数
      default(rawProps) {
        return { message: 'hello' }
      }
    },
    // 自定义类型校验函数
    propF: {
      validator(value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].includes(value)
      }
    },
    // 函数类型的默认值
    propG: {
      type: Function,
      // 不像对象或数组的默认，这不是一个
      // 工厂函数。这会是一个用来作为默认值的函数
      default() {
        return 'Default function'
      }
    }
  }
}
```
</div>
---

# 组件 事件

事件的触发与监听： 回顾一下之前组件基础的内容！

事件模型：父组件通过 _v-on_ 来注册事件处理器，子组件通过 _this.$emit("事件名",
参数)_ 来触发事件。

组件的事件监听器也支持 _.once_ 修饰符：

```html
<MyComponent @some-event.once="callback" />
```

<AlertBox v-click>和原生 DOM
事件不一样，组件触发的事件没有冒泡机制。你只能监听直接子组件触发的事件。</AlertBox>

---

# 事件：参数、声明及校验

<div class="flex">
<div class="w-1/2 pr-2">

## 事件参数

```html
<button @click="$emit('increaseBy', 1)">Increase by 1</button>
```

父组件中监听事件，写一个内联的箭头函数作为监听器，此函数会接收到事件附带的参数：

```html
<button @click="$emit('increaseBy', 1)">Increase by 1</button>
```

</div>
<div class="w-1/2" v-click>

## 声明触发的事件

```js
export default {
  emits: ["inFocus", "submit"],
};
```

这个 emits 选项还支持对象语法，它允许我们对触发事件的参数进行验证：

```js
export default {
  emits: {
    submit(payload) {
      // 通过返回值为 `true` 还是为 `false` 来判断
      // 验证是否通过
    },
  },
};
```

</div>
</div>

[参考阅读](https://cn.vuejs.org/guide/components/events.html)

---

# 组件 v-model

v-model 可以在组件上使用以实现**双向绑定效果**。v-model
只是一个语法糖。其展开形式：

```html {1-4|6-9|all}
<input :value="searchText" @input="searchText = $event.target.value" />

<CustomInput
  :modelValue="searchText"
  @update:modelValue="newValue => searchText = newValue"
/>
```

要让这个例子实际工作起来，\<CustomInput> 组件内部需要做两件事：

1. 将内部原生 \<input> 元素的 _value_ attribute 绑定到 _modelValue_ prop
2. 当原生的 input 事件触发时，触发一个携带了新值的 _update:modelValue_
   自定义事件

_演练一下_

---

## v-model 的参数

默认情况下，v-model 在组件上都是使用 modelValue 作为 prop，并以
update:modelValue 作为对应的事件。 我们可以通过给 v-model
指定一个参数来更改这些名字：

```html
<MyComponent v-model:title="bookTitle" />
```

在这个例子中，子组件应声明一个 _title_ prop，并通过触发 _update:title_
事件更新父组件值：

```vue
<script>
export default {
  props: ["title"],
  emits: ["update:title"],
};
</script>
<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>
```

上面的情况，可以推广到多个属性的绑定。

---

# 透传 Attributes

我们来创建一个自定义的修饰符 capitalize，它会自动将 v-model
绑定输入的字符串值第一个字母转为大写：

<AlertBox>

Attributes 继承：​ _“透传 attribute”_ 指的是传递给一个组件，却没有被该组件声明为
props 或 emits 的 attribute 或者 v-on 事件监听器。

</AlertBox>

最常见的例子就是 class、style 和 id。 当组件渲染时，透传的 _attribute_
会自动被添加到根元素上。
这是非常有用的特性，对于封装组件可以大大简化我们的工作。

_演练一下_

<AlertBox v-click>

自动的 _attributes_ 穿透需要你的组件本身是有唯一根元素的情况下才生效。
因此，如果不是特殊情况，建议每个组件都有唯一根元素（虽然在 vue3 中不是必须的）！

</AlertBox>

---

# 禁用 Attributes 继承 ​

如果你不想要一个组件自动地继承 attribute，你可以在组件选项中设置 _inheritAttrs:
false_ 。

最常见的需要禁用 attribute 继承的场景就是 attribute
需要应用在根节点以外的其他元素上。 通过设置 inheritAttrs 选项为
false，你可以完全控制透传进来的 attribute 被如何使用。

<v-click>

这些透传进来的 attribute 可以在模板的表达式中直接用 _$attrs_ 访问到。

```html
<span>Fallthrough attribute: {{ $attrs }}</span>
```

</v-click>
<v-click>

这个 $attrs 对象包含了除组件所声明的 props 和 emits 之外的所有其他
attribute，例如 class，style，v-on 监听器等等。

有几点需要注意：

1. 和 props 有所不同，透传 attributes 在 JavaScript 中保留了它们原始的大小写，
   所以像 foo-bar 这样的一个 attribute 需要通过 _$attrs['foo-bar']_ 来访问。
2. 像 @click 这样的一个 _v-on_ 事件监听器将在此对象下被暴露为一个函数
   _$attrs.onClick_ 。

</v-click>
<v-click>

[参考阅读](https://cn.vuejs.org/guide/components/attrs.html)

</v-click>

---

# 插槽 Slots

_回顾一下_：组件基础的插槽（粗略的理解：**标签体**）！

\<slot> 元素是一个插槽出口 (slot outlet)，标示了父元素提供的插槽内容 (slot
content) 将在哪里被渲染。

<img src="/slots.png" class="m-4 h-60 mx-auto rounded-xl shadow-2xl">

<AlertBox>

\<slot> 就是一个占位符，用于标注以后父组件给出内容在组件中的渲染位置！

</AlertBox>

---

# 渲染作用域

插槽内容可以访问到父组件的数据作用域，因为插槽内容本身是在父组件模板中定义的。

```html
<span>{{ message }}</span> <FancyButton>{{ message }}</FancyButton>
```

<AlertBox>

父组件模板中的表达式只能访问父组件的作用域；子组件模板中的表达式只能访问子组件的作用域。

</AlertBox>

---

# 作用域插槽

然而在某些场景下插槽的内容可能想要同时使用父组件域内和子组件域内的数据。
要做到这一点，我们需要一种方法来让子组件在渲染时将一部分数据提供给插槽。
可以像对组件传递 props 那样，向一个插槽的出口上传递 attributes：

<div class="flex">
<div class="w-1/2">

```html {2}
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
```

```html {all|1|2}
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>
```

也可以在 v-slot 中使用解构：

```html
<MyComponent v-slot="{ text, count }"> {{ text }} {{ count }} </MyComponent>
```

</div>
<div class="w-1/2">
  <img src="/scoped-slots.svg" class="ml-2 rounded-xl shadow-2xl" />
</div>
</div>

---

# 具名插槽

在一个组件中包含多个插槽出口，比如：

```html {all|3|6|9|3,6,9}
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

\<slot> 元素可以有一个特殊的 attribute _name_， 给各个插槽分配唯一的
ID，以确定每一处要渲染的内容。 带 _name_ 的插槽被称为 **具名插槽 (named
slots)**。 没有提供 _name_ 的 \<slot> 出口会隐式地命名为 _“default”_。

```html
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```

---

## v-slot 缩写

_v-slot_ 有对应的简写 _#_，因此 _\<template v-slot:header>_ 可以简写为
_\<template #header>_。

<div class="flex">
  <div class="w-1/2">
    <img src="/named-slots.png" class="h-40" />
  </div>
  <div class="w-1/2">

```html
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>
```

</div>
</div>

---

## 具名作用域插槽

具名作用域插槽的工作方式也是类似的， 插槽 props 可以作为 v-slot
指令的值被访问到：_v-slot:[name]="slotProps"_

```html
<MyComponent>
  <template #header="headerProps"> {{ headerProps }} </template>

  <template #default="defaultProps"> {{ defaultProps }} </template>

  <template #footer="footerProps"> {{ footerProps }} </template>
</MyComponent>
```

<AlertBox>

插槽上的 _name_ 是一个 Vue 特别保留的 attribute，不会作为 props 传递给插槽。

</AlertBox>

---

## 动态插槽名

动态指令参数在 v-slot 上也是有效的，即可以定义下面这样的动态插槽名：

```html
<base-layout>
  <template v-slot:[dynamicSlotName]> ... </template>

  <!-- 缩写为 -->
  <template #[dynamicSlotName]> ... </template>
</base-layout>
```

---

# 依赖注入 provide / inject

通常情况下，当我们需要从父组件向子组件传递数据时，会使用 _props_。
但如果组件树深度较大，逐级传递非常麻烦！

_provide_ 和 _inject_ 可以帮助我们解决这一问题。
一个父组件相对于其所有的后代组件，会作为**依赖提供者**。
任何后代的组件树，无论层级有多深，都可以**注入**由父组件提供给整条链路的依赖。

<img src="/provide-inject.png" class="mx-auto h-80">

---

## provide

要为组件后代提供数据，需要使用到 provide 选项：

```js {2-4|3}
export default {
  provide: {
    message: "hello!",
  },
};
```

对于 _provide_ 对象上的每一个属性，后代组件用其 _key_ 为注入名查找期望注入的值，
属性的值就是要提供的数据。 如果需要依赖当前组件实例的状态，则以函数形式使用
_provide_：

```js {all|7-12}
export default {
  data() {
    return {
      message: "hello!",
    };
  },
  provide() {
    return {
      message: this.message, // 使用函数的形式，可以访问到 `this`
    };
  },
};
```

---

## 应用层 Provide

除了在一个组件中提供依赖，我们还可以在整个应用层面提供依赖：

```js
import { createApp } from "vue";

const app = createApp({});

app.provide(/* 注入名 */ "message", /* 值 */ "hello!");
```

在应用级别提供的数据在该应用内的**所有组件**中都可以 _注入_。
这在你编写插件时会特别有用，因为插件一般都不会使用组件形式来提供值。

---

# inject

要注入上层组件提供的数据，需使用 _inject_ 选项来声明：

```js
export default {
  inject: ["message"],
  created() {
    console.log(this.message); // injected value
  },
};
```

注入会在组件自身的状态**之前**被解析，因此你可以在 _data()_ 中访问到注入的属性：

```js
export default {
  inject: ["message"],
  data() {
    return {
      // 基于注入值的初始数据
      fullMessage: this.message,
    };
  },
};
```

---

## 注入默认值

默认情况下，_inject_ 假设传入的注入名会被某个祖先链上的组件提供。
如果该注入名的确没有任何组件提供，则会抛出一个运行时警告。

如果在注入一个值时不要求必须有提供者，那么我们应该声明一个默认值，和 _props_
类似：

```js {all|7,12}
export default {
  // 当声明注入的默认值时
  // 必须使用对象形式
  inject: {
    message: {
      from: "message", // 当与原注入名同名时，这个属性是可选的
      default: "default value",
    },
    user: {
      // 对于非基础类型数据，如果创建开销比较大，或是需要确保每个组件实例
      // 需要独立数据的，请使用工厂函数
      default: () => ({ name: "John" }),
    },
  },
};
```

---

## 使用 Symbol 作注入名

至此，我们已经了解了如何使用字符串作为注入名。
但如果你正在构建大型的应用，包含非常多的依赖提供，
或者你正在编写提供给其他开发者使用的组件库， 建议最好使用 _Symbol_
来作为注入名以避免潜在的冲突。

```js
// keys.js
export const myInjectionKey = Symbol();
// 在供给方组件中
import { myInjectionKey } from "./keys.js";
export default {
  provide() {
    return {
      [myInjectionKey]: {
        /* 要提供的数据 */
      },
    };
  },
};
// 注入方组件
import { myInjectionKey } from "./keys.js";
export default {
  inject: {
    injected: { from: myInjectionKey },
  },
};
```

---

# 异步组件 ​

**异步组件**是指没有被打包到 _主包_（ _main.js_）中的组件。 因此没有和 _主包_
**同步**下载到客户端， 如果要使用，则需要从服务器加载（**异步**）组件代码！

Vue 提供了 defineAsyncComponent 方法来实现异步组件：

```js
import { defineAsyncComponent } from "vue";

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...从服务器获取组件
    // 如果发生错误
    if (error) return reject(error);
    resolve(); /* 获取到的组件 */
  });
});
// ... 像使用其他一般组件一样使用 `AsyncComp`
```

---

ES 模块动态导入也会返回一个 **Promise**，所以多数情况下我们会将它和
_defineAsyncComponent_ 搭配使用。 类似 _Vite_ 和 _Webpack_
这样的构建工具也支持此语法 (并且会将它们作为打包时的代码分割点)，
因此我们也可以用它来导入 Vue 单文件组件：

```js
import { defineAsyncComponent } from "vue";

export default defineAsyncComponent(() =>
  import("./components/MyComponent.vue")
);
```

这个组件和其他普通组件一样使用（进行全局或者局部注册）！
