# vue.js语法

## 简介

​	Vue.js 使用了基于 HTML 的模板语法，**允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据**。所有 Vue.js 的模	板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

​	**在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数**。结合响应系统，Vue 能够智能地计算出最少需要重新渲染	多少组件，并把 DOM 操作次数减到最少。

​	如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，[直接写渲染 (render) 函数](https://cn.vuejs.org/v2/guide/render-function.html)，使用可选	的 JSX 语法。

## 插值

>**所谓的插值就可以简单理解将组件中存储的数据，填充到html相应位置中去。**
>
>相当于学习react的jsx语法。

### 文本

**语法：**数据绑定最常见的形式就是使用**“Mustache”语法 (双大括号) 的文本插值**



```html
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` property 的值。无论何时，绑定的数据对象上 `msg` property 发生了改变，插值处的内容都会更新。

通过使用 [v-once 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

**示例：**

```vue
<template>
  <div id="container">
    <h1>{{msg}}</h1>   
  </div>
</template>



<script>
export default {
  name: "test",
  data: function() {
    return {
      msg: "vue 初体验"
    };
  }
};
</script>

<style scoped>
#container{
    text-align: center
}
</style>
```

>**vue的数据存储位置：**
>
>不同于react把数据存储在state中，vue把数据存储在一个`data`的字段里，插值的时候，直接使用**双大括号**读取对应值即可。
>
>**另外，vue的data里的数据是以函数的形式存储，为什么呢？**
>
>主要是为了隔离作用域，防止组件复用时，一个组件修改值，所有的组件的值都发生变化
>
>data是一个对象，它们之间的地址空间是互通，如果是函数，它是一个私密的空间。函数是有自己的函数作用域



### 原始HTML

插值时，不仅能插入纯文本，还能插入标签元素。

**语法：**双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html`

```vue
<template>
  <div id="container">
    <h1>{{msg}}</h1>
     <h2 v-html="msg"></h2>
  </div>
</template>



<script>
export default {
  name: "test",
  data: function() {
    return {
      msg: " <p>vue 初体验</p>"
    };
  }
};
</script>

<style scoped>
#container{
    text-align: center
}


p{
    color: rebeccapurple;
}

</style>
```



![mark](http://qiniu.wind-zhou.com/blog/210420/15dFeF4kFA.png?imageslim)

>**遇到问题**： v-html添加的内容，css样式不起作用
>
>原因：**scoped属性在搞鬼。**
>
> 因为如果添加了scoped，该属性表示，css样式仅用于当前组件（隔离css的作用域）。那是因为，v-html相当于引入外部组件内容。所以无法应用该样式。
>
>解决方案：去掉scoped

### Attribute

Mustache 语法不能作用在 HTML attribute 上，**但有时需要给属性名赋值一个变量**。

遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)

**语法：**

```html
<div v-bind:id="dynamicId"></div>
```

对于布尔 attribute (它们只要存在就意味着值为 `true`)，`v-bind` 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```

如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` attribute 甚至不会被包含在渲染出来的 `<button>` 元素中。



**示例：**

```vue
<template>
  <div id="container">
    <h1 v-bind:class="color">{{msg}}</h1>
  </div>
</template>

<script>
export default {
  name: "test",
  data: function() {
    return {
      msg: " vue 初体验",
      color:'red'
    };
  }
};
</script>

<style  >
#container {
  text-align: center;
}

.red{
    color: red;
}
</style>
```

![mark](http://qiniu.wind-zhou.com/blog/210420/EmLdBBCa7i.png?imageslim)



### js表达式

插值时还可以插入js的表达式。

**示例：**

```vue
<template>
  <div id="container">
    <!-- 下面在属性插值时。插入了js表达式 -->
    <h1 v-bind:class=" false?bg1:bg2">{{msg}}</h1>
  </div>
</template>

<script>
export default {
  name: "test",
  data: function() {
    return {
      msg: " vue 初体验",
      bg1: "red",
      bg2: "green"
    };
  }
};
</script>

<style  >
#container {
  text-align: center;
}

.red {
  color: red;
}

.green {
  color: green;
}
</style>
```

<img src="http://qiniu.wind-zhou.com/blog/210420/eAcaEBFDHE.png?imageslim" alt="mark" style="zoom:25%;" />

>同理。在文本插值时。也可以在`{{ }}`中插入js表达式。



## 指令

指令 (Directives) 是**带有 `v-` 前缀的特殊 attribute**。

指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。

**指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM**。

指令其实就是用js写的。

### [参数](https://cn.vuejs.org/v2/guide/syntax.html#参数)

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
```

> **在这里 `href` 是参数**，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

在这里参数是监听的事件名。我们也会更详细地讨论事件处理。

### [修饰符](https://cn.vuejs.org/v2/guide/syntax.html#修饰符)

>作用：增加功能。

修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

**例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：**

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下来对 [`v-on`](https://cn.vuejs.org/v2/guide/events.html#事件修饰符) 和 [`v-for`](https://cn.vuejs.org/v2/guide/forms.html#修饰符) 等功能的探索中，你会看到修饰符的其它例子。



## [缩写](https://cn.vuejs.org/v2/guide/syntax.html#缩写)

### [`v-bind` 缩写](https://cn.vuejs.org/v2/guide/syntax.html#v-bind-缩写)

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### [`v-on` 缩写](https://cn.vuejs.org/v2/guide/syntax.html#v-on-缩写)

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

它们看起来可能与普通的 HTML 略有不同，但 `:` 与 `@` 对于 attribute 名来说都是合法字符，在所有支持 Vue 的浏览器都能被正确地解析。而且，它们不会出现在最终渲染的标记中。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。

**示例：**

```vue
<template>
  <div id="container">
    <!-- 下面在属性插值时。使用了简写形式 -->
    <h1 :class=" false?bg1:bg2">{{msg}}</h1>
  </div>
</template>
```





## 过滤器

>其实就可以理解为对输入的内容进行一步预处理。

**语法：**

Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式** (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：

```vue
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

**管道符左侧为被处理的数据，管道符右侧为一个函数，用于对左侧数据做处理并返回。**



你可以在一个组件的选项中定义本地的过滤器：

```js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```



**示例：**将左侧的字符串都转换成大写（感觉相当于数组的forEach和filter方法）

```vue
<template>
  <div id="container">
    <!-- 过滤器filter -->
    <p>{{meassage|translate}}</p>
  </div>
</template>

<script>
export default {
  name: "test",
  data: function() {
    return {
      meassage: "jighiwphghwrpihgiprwhpighrwpgh"
    };
  },
  filters:{
      translate:function( value){
          return value.toUpperCase()
      } 
  }
};
</script>

<style  >
#container {
  text-align: center;
}

p {
  color: green;
}
</style>
```

![mark](http://qiniu.wind-zhou.com/blog/210420/kiDBe90FbA.png?imageslim)

>注：所有的过滤器都要写在一个`filters`的字段下,该字段的值是一个过滤器函数组成的对象。