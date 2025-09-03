
# data 和 methods 选项

**data** Property 是一个函数选项，返回一个对象（Model）。

<hr class="m-2" />

**methods** 是包含方法的一个对象，提供模板中要使用的函数。

```js {all|2-6|7-11|all}
const options = {
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
```

<style>

</style>
