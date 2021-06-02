# vue-router

Vue Router 是 [Vue.js](http://cn.vuejs.org/) 官方的路由管理器。它和 Vue.js 的核心深度集成，让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

# 动态路由匹配

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 `User` 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。

> 渲染的是同一个路由，不过根据传入值不同，渲染的也不同。
>
> **就是使用通配符实现。**

>对应于react的路由传值。
>
>**同样，也都包含两个过程:**
>
>（1）传值
>
>（2）接收



## 使用params传值

(1) 传值

```js
<!-- 使用params传值-->   
<router-link :to="'/about'+name">About</router-link>
```

（2）声明接收

```js
const routes: Array<RouteConfig> = [
  {
    //每个对象就是路由的规则，相当于react的route，注册路由
    path: "/",
    name: "Home",
    component: Home
  },
  {
    path: "/about:name",
    name: "About",
    component: () =>
      import(/* webpackChunkName: "about" */ "../views/About.vue") //组件的异步加载
  }
];
```

(3)  接收

```js
<div class="about">
    <h1>This is an about page {{$route.params.name}}</h1>
</div>
```

## 使用query传值

（1）传值

```js
<!-- 使用query传值-->  
    <router-link to="/?id=123456">Home</router-link>
```

（2）接收

```js
<div class="home">
    <h1>大自然真的自然吗？{{ $route.query.id }}</h1>
</div>
```



## 响应路由参数的变化

提醒一下，当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。

>面试题：
>
>（1）watch 监听变化
>
>（2）导航守卫

复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) `$route` 对象：

# 嵌套路由



（1）设置导航路由

```vue
<template>
  <div id="app">
    <div id="nav">
      <input type="text" v-model="name" />
      <router-link to="/?id=123456">Home</router-link>|
      <!-- 使用query传值-->
      <router-link :to="'/about'+name">About</router-link>
      <!-- 使用params传值-->
    </div>
    <router-view />
    <!-- 视图组件，路由组件显示在这里-->
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      name: "wind-zhou"
    };
  }
};
</script>>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```



(2) 路由注册

在路由配置里添加children字段

```js
import Vue from "vue";
import VueRouter, { RouteConfig } from "vue-router";
import Home from "../views/Home.vue";
import Address from "../components/address.vue";
import Tel from "../components/tel.vue";

Vue.use(VueRouter);

const routes: Array<RouteConfig> = [
  {
    //每个对象就是路由的规则，相当于react的route，注册路由
    path: "/",
    name: "Home",
    component: Home,
    children: [                  //注册二级路由
      {
        path: "/Home/",
        component: Address
      },
      {
        path: "/Home/tel",
        component: Tel
      }
    ]
  },
  {
    path: "/about:name",
    name: "About",
    component: () =>
      import(/* webpackChunkName: "about" */ "../views/About.vue") //组件的异步加载
  }
];

const router = new VueRouter({   //生成一个路由组件
  mode: "history", //也可使选择hash模式
  base: process.env.BASE_URL,
  routes
});

export default router;
```

（3）在要渲染组建的位置插入视图组件

```vue
<router-view />
```





























