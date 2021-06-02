# vue 前后台交互

## vue-resource

### 下载安装

```js
$ yarn add vue-resource
$ npm install vue-resource
```

### 语法 & API

你可以使用全局对象方式 Vue.http 或者在一个 Vue 实例的内部使用 this.$http来发起 HTTP 请求。

```js
// 基于全局Vue对象使用http
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// 在一个Vue实例内使用$http
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```



**示例：**

```js
   mounted(){
       this.$http.post("http://www.qhdlink-student.top/student/coach.php","username=zyp&userpwd=123456&userclass=64&type=4",{
           headers:{"ConTent-type":"application/x-www-form-urlencoded"},
           timeout:5000,
       }).then(function(rep){
             console.log(rep)
       },function(err){
           console.log(err+"错误")
       })
   }
```



**options对象可选参数：**

![mark](http://qiniu.wind-zhou.com/blog/210427/8mc0GjJ5G1.png?imageslim)



**返回数据的对象字段：**

![mark](http://qiniu.wind-zhou.com/blog/210427/l91a97cEiC.png?imageslim)





## axios

### 下载安装

```js
$ yarn add axios
```

### 引入

（1）注册成全局

在入口文件中的Vue对象的原型中注册一下

```js
import Vue from 'vue'
import axios from 'axios'

import MyAxios from './components/axios/axios.vue'
Vue.prototype.$axios = axios //全局注册，使用方法为:this.$axios

new Vue({
    el: '#app',
    render: h => h(MyAxios),
})
```

全局注册时使用的语法

```js
  this.$axios.post(
        "http://www.qhdlink-student.top/student/coach.php",
        "username=zyp&userpwd=123456&userclass=64&type=4"
      )
      .then(response => {
        console.log(response.data);
      });
```



(2)也可以在需要的组件中引入

```js
import axios from 'axios'
```

局部引入时的使用，直接调用即可

```js
  axios.post(
        "http://www.qhdlink-student.top/student/coach.php",
        "username=zyp&userpwd=123456&userclass=64&type=4"
      )
      .then(response => {
        console.log(response.data);
      });
```



### 调用

下面使用全局的注册方法

```vue
<template>
  <div class="one"></div>
</template>

<script>

export default {
  data: function() {
    return {
      name: "wind-zhou"
    };
  },
  mounted() {
     this.$axios.post(
        "http://www.qhdlink-student.top/student/coach.php",
        "username=zyp&userpwd=123456&userclass=64&type=4"
      )
      .then(response => {
        console.log(response.data);
      });
  }
};
</script>

<style>
</style>
```





**示例**：使用axios请求朗科的老师接口



```js
<template>
  <div class="one">
    <table>
      <tr v-for="(item,index) in teacher" :key="index">
        <td v-for="(tdItem,Citem) in item" :key="tdItem.id_coach">
          <!-- 使用for-in遍历对象 -->
          <template v-if="Citem==='path_coach'">
            <img v-bind:src="imgUrl+tdItem" />
          </template>
          <template v-else>{{tdItem}}</template>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      teacher: "",
      imgUrl:''
    };
  },
  mounted() {
    this.$axios
      .post(
        "http://www.qhdlink-student.top/student/coach.php",
        "username=zyp&userpwd=123456&userclass=64&type=4",
         {
          headers: { "Content-Type": "application/x-www-form-urlencoded" }
        }
      )
      .then(response => {
        this.teacher = response.data;
        this.imgUrl='http://www.qhdlink-student.top/'
      });
  }
};
</script>

<style>
</style>
```

## 执行多个并发请求

就是**并发**的发送多个axios请求。



```js
function getUserAccount() {
  return axios.get('/user/12345');
}
 
function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}
axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```



使用到两个api

```js
axios.all(iterable)
axios.spread(callback)
```

## 配置项

### 请求配置项

```js
{

  url: "/user",  // `url` 是用于请求的服务器 URL
  method: "get", // 默认是 get    // `method` 是创建请求时使用的方法
  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: "https://some-domain.com/api/",

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 "PUT", "POST" 和 "PATCH" 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理

    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {"X-Requested-With": "XMLHttpRequest"},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: "brackets"})
  },

  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 "PUT", "POST", 和 "PATCH"
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: "Fred"
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求花费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // 默认的

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

  // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: "janedoe",
    password: "s00pers3cret"
  },

  // `responseType` 表示服务器响应的数据类型，可以是 "arraybuffer", "blob", "document", "json", "text", "stream"
  responseType: "json", // 默认的

  // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: "XSRF-TOKEN", // default

  // `xsrfHeaderName` 是承载 xsrf token 的值的 HTTP 头的名称
  xsrfHeaderName: "X-XSRF-TOKEN", // 默认的

  // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

  // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status &gt;= 200 &amp;&amp; status &lt; 300; // 默认的
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // 默认的

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // "proxy" 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: "127.0.0.1",
    port: 9000,
    auth: : {
      username: "mikeymike",
      password: "rapunz3l"
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

### 响应配置项

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status`  HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: "OK",

  // `headers` 服务器响应的头
  headers: {},

  // `config` 是为请求提供的配置信息
  config: {}
}
```



![mark](http://qiniu.wind-zhou.com/blog/210427/eJgKJ41E6l.png?imageslim)



## 封装axios

```jsx

export default function ajax(url, data = {}, type = 'GET') { //封装一个ajax

    return new Promise((resolve, reject) => {
            // 1、执行ajax请求
            let promise;

            if (type === 'GET') { //get
                promise = axios.get(url, { //接收一下promise
                    params: data
                })
            } else { //post
                promise = axios.post(url, data)
            }

            // 2、成功后，调用reslove
            promise.then((response) => {
                resolve(response.data)

            }).catch((error) => { //请求出错也不reject，而是现实错误提示
                message.error('请求出错', error.message)

            })
        })
        // 这里新建返回了一个promise，在promise对象内内时另外一个promise（ajajx返回得出）对象返回的内容
}
```



Performing a `GET` request

```js
const axios = require('axios');

// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .then(function () {
    // always executed
  });

// Optionally the request above could also be done as
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .then(function () {
    // always executed
  });  

// Want to use async/await? Add the `async` keyword to your outer function/method.
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

> **NOTE:** `async/await` is part of ECMAScript 2017 and is not supported in Internet Explorer and older browsers, so use with caution.

Performing a `POST` request

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

## 拦截器



拦截器可以做有些公共的事件，例如给每个请求欠佳一些东西。



```js
//请求拦截器
axios.interceptors.request.use(
  function (config) {
      // 在发送请求之前做些什么
      return config;
  },
  function (error) {
      // 对请求错误做些什么
      return Promise.reject(error);
  }
);

//响应拦截器
axios.interceptors.response.use(
  function (config) {
      // 对响应数据做点什么
      return config;
  },
  function (error) {
      // 对响应错误做点什么
      return Promise.reject(error);
  }
);
```

