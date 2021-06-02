# vue 生命周期

![mark](http://qiniu.wind-zhou.com/blog/210427/dmCm4b3067.png?imageslim)

渲染时，都是由里向外的，先渲染子组件，再渲染父组件。



### [beforeCreate](https://cn.vuejs.org/v2/api/#beforeCreate)

- **类型**：`Function`

- **详细**：

  在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

  

### [created](https://cn.vuejs.org/v2/api/#created)

- **类型**：`Function`

- **详细**：

  在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，property 和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，`$el` property 目前尚不可用。

  

- **一般做初始数据的获取，可以在里面做异步操作（ajajx请求）。**

  >挂载数据，绑定事件等等，然后执行created函数， **这个时候已经可以使用到数据，也可以更改数据**
  >**在这里更改数据不会触发updated函数**，在这里可以在渲染前倒数第二次更改数据的机会， 不会触发
  >其他的钩子函数，一般可以在这里做**初始数据的获取**

### [beforeMount](https://cn.vuejs.org/v2/api/#beforeMount)

- **类型**：`Function`

- **详细**：

  在挂载开始之前被调用：相关的 `render` 函数首次被调用。

  **该钩子在服务器端渲染期间不被调用。**

  

- 
  >接下来开始找实例或者组件对应的模板， 编译模板为虚拟dom放入到render函数中准备渲染， 然后执
  >行beforeMount钩子函数，在这个函数中虚拟dom已经创建完成，马上就要渲染在这里也可以更改数
  >据，**不会触发updated,在这里可以在渲染前最后一次更改数据的机会**， 不会触发其他的钩子函数，
  >一般可以在这里做初始数据的获取
  >
  >**主要还是对数据的初始化**，与create作用类似。 

### [mounted](https://cn.vuejs.org/v2/api/#mounted)

- **类型**：`Function`

- **详细**：

  实例被挂载后调用，这时 `el` 被新创建的 `vm.$el` 替换了。如果根实例挂载到了一个文档内的元素上，当 `mounted` 被调用时 `vm.$el` 也在文档内。

  注意 `mounted` **不会**保证所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以在 `mounted` 内部使用 [vm.$nextTick](https://cn.vuejs.org/v2/api/#vm-nextTick)：

  ```js
  mounted: function () {
    this.$nextTick(function () {
      // Code that will run only after the
      // entire view has been rendered
    })
  }
  ```

  **该钩子在服务器端渲染期间不被调用。**

  >接下来开始render, 渲染出真实dom，然后执行mounted钩子函数， 此时，组件已经出现在页面中，
  >数据、真实dom都已经处理好了，事件都已经挂载好了，**可以在这里操作真实dom等事情...**

### [beforeUpdate](https://cn.vuejs.org/v2/api/#beforeUpdate)

- **类型**：`Function`

- **详细**：

  数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。

  **该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。**

  

  >data修改后触发触发，**注意这里面不能在修改data**，否则会陷入死循环。
  >
  >.当组件或实例的数据更改之后，会立即执行beforeUpdate, **然后vue的虚拟dom机制会重新构建虚拟**
  >**dom**与上一次的虚拟dom树利用df算法进行对比之 后重新渲染，<u>**一般不做什么事儿**</u>



>注：react里有个shouldComponentUpdata，来判断是否更新，vue里没有，因为他会自动判断。



### [updated](https://cn.vuejs.org/v2/api/#updated)

- **类型**：`Function`

- **详细**：

  由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

  当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态。如果要相应状态改变，通常最好使用[计算属性](https://cn.vuejs.org/v2/api/#computed)或 [watcher](https://cn.vuejs.org/v2/api/#watch) 取而代之。

  注意 `updated` **不会**保证所有的子组件也都一起被重绘。如果你希望等到整个视图都重绘完毕，可以在 `updated` 里使用 [vm.$nextTick](https://cn.vuejs.org/v2/api/#vm-nextTick)：

  ```
  updated: function () {
    this.$nextTick(function () {
      // Code that will run only after the
      // entire view has been re-rendered
    })
  }
  ```

  **该钩子在服务器端渲染期间不被调用。**

- **参考**：[生命周期图示](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)

### [activated](https://cn.vuejs.org/v2/api/#activated)

- **类型**：`Function`

- **详细**：

  被 keep-alive 缓存的组件激活时调用。

  **该钩子在服务器端渲染期间不被调用。**

- **参考**：

  - [构建组件 - keep-alive](https://cn.vuejs.org/v2/api/#keep-alive)
  - [动态组件 - keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#在动态组件上使用-keep-alive)

### [deactivated](https://cn.vuejs.org/v2/api/#deactivated)

- **类型**：`Function`

- **详细**：

  被 keep-alive 缓存的组件停用时调用。

  **该钩子在服务器端渲染期间不被调用。**

- **参考**：

  - [构建组件 - keep-alive](https://cn.vuejs.org/v2/api/#keep-alive)
  - [动态组件 - keep-alive](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#在动态组件上使用-keep-alive)

### [beforeDestroy](https://cn.vuejs.org/v2/api/#beforeDestroy)

- **类型**：`Function`

- **详细**：

  实例销毁之前调用。在这一步，实例仍然完全可用。

  **该钩子在服务器端渲染期间不被调用。**

  

### [destroyed](https://cn.vuejs.org/v2/api/#destroyed)

- **类型**：`Function`

- **详细**：

  实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

  **该钩子在服务器端渲染期间不被调用。**

>**beforeDestory和destroy主要做一些清理工作**，例如清除计时器，全局变量等。
>
>需手动调用。
>
>他们两个作用一样，写一个即可。
>
>组件销毁追销毁组件自身的东西。全局的不会销毁。







**vue里可以写哪些值?**



![mark](http://qiniu.wind-zhou.com/blog/210426/1BJ0Adg627.png?imageslim)





