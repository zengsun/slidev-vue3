# 表单输入绑定​

在前端处理表单时，我们常常需要将表单输入框的内容同步给 JavaScript 中相应的变量。
手动连接值绑定和更改事件监听器可能会很麻烦：

```html
<input
  :value="text"
  @input="event => text = event.target.value">
```

v-model 指令简化了这个步骤

```html
<input v-model="text">
```

另外，v-model 还可以用于各种不同类型的输入，\<textarea>、\<select> 元素。它会根据所使用的元素自动使用对应的 DOM 属性和事件组合：

* 文本类型的 \<input> 和 \<textarea> 元素会绑定 value property 并侦听 input 事件；
* \<input type="checkbox"> 和 \<input type="radio"> 会绑定 checked property 并侦听 change 事件；
* \<select> 会绑定 value property 并侦听 change 事件。

---

# v-model 修饰符

.lazy​

默认情况下，v-model 会在每次 input 事件后更新数据 (IME 拼字阶段的状态例外)。
你可以添加 lazy 修饰符来改为在每次 change 事件后更新数据：

```html
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />
```

.number​

如果你想让用户输入自动转换为数字，你可以在 v-model 后添加 .number 修饰符来管理输入：

```html
<input v-model.number="age" />
```

.trim​

如果你想要默认自动去除用户输入内容中两端的空格，你可以在 v-model 后添加 .trim 修饰符：

```html
<input v-model.trim="msg" />
```