# Class 与 Style 绑定



操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，**Vue.js 做了专门的增强**。表达式结果的类型除了字符串之外，还可以是对象或数组。

















