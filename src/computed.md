
# 计算选项

computed 是一个包含方法的对象。基本上依赖 data 选项，
所以叫计算属性实在贴切。

**计算属性**会基于其响应式依赖被缓存。和其他实现方式
相比，会有较高的性能表现！

建议：可以使用计算属性完成的功能，尽量使用计算属性完成。

```js {all|12-17}
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
  computed: {
    double() {
      return this.counter * 2;
    }
  }
};
```
