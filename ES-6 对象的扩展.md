# lesson-6 对象的扩展

对象（object）是 JavaScript 最重要的数据结构。ES6 对它进行了重大升级，本章介绍数据结构本身的改变，下一章介绍`Object`对象的新增方法。

## 属性的简洁表示法

**简介表示法**：ES6 **允许在大括号里面，直接写入变量和函数作为对象的属性和方法**。这样的书写更加简洁。

**属性：**

属性这有以下两种写法：



```javascript
<script>
    let foo = 'bar';

    let obj = {
        foo
    }
    console.log(obj)
</script>


//或者
<script>
    let foo = 'bar';

    let obj = {
        x: foo
    }
    console.log(obj)
</script>
```

两者输出分别为：![mark](http://qiniu.wind-zhou.com/blog/210329/iijHge2cKK.png?imageslim)

![mark](http://qiniu.wind-zhou.com/blog/210329/jm8imme1f4.png?imageslim)

**就是说如果第一种方法直接将foo写入对象，obj对象会默认将变量名作为key值。**

**方法：**

除了属性简写，方法也可以简写。



```javascript
<script>
    //es5方法写法
    var obj1 = {
        name: '周正',
        sayName: function() {
            console.log(this.name)
        }
    }
 
  //es6方法写法
    var obj2 = {
        age: 24,
        sayAge() {
            console.log(`my age is ${this.age}`)
        }
    }

    obj1.sayName()
    obj2.sayAge()
</script>
```

输出：![mark](http://qiniu.wind-zhou.com/blog/210329/A3H21bb5Cc.png?imageslim)

**相比于es5的方法声明方法，es6的方法声明方法更简洁，省略了中间function。**



这种写法用于函数的返回值，将会非常方便。

```javascript
function getPoint() {
  const x = 1;
  const y = 10;
  return {x, y};
}

getPoint()
// {x:1, y:10}
```



## 属性名表达式

**核心：es6允许使用表达式定义对象的属性名和方法名。**



JavaScript 定义对象的属性，有两种方法。

```javascript
// 方法一
obj.foo = true;

// 方法二
obj['a' + 'bc'] = 123;
```

上面代码的**方法一是直接用标识符作为属性名**，**方法二是用表达式作为属性名**，这时要将表达式放在方括号之内。



但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。

```javascript
var obj = {
  foo: true,
  abc: 123
};
```

**ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内。**

```javascript
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
```



下面是另一个例子。

```javascript
let lastWord = 'last word';

const a = {
  'first word': 'hello',
  [lastWord]: 'world'
};

a['first word'] // "hello"
a[lastWord] // "world"
a['last word'] // "world"
```

**表达式还可以用于定义方法名。**

```javascript
let obj = {
  ['h' + 'ello']() {
    return 'hi';
  }
};

obj.hello() // hi
```

注意，属性名表达式与简洁表示法，不能同时使用，会报错。

```javascript
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };

// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};
```

注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串`[object Object]`，这一点要特别小心。

```javascript
const keyA = {a: 1};
const keyB = {b: 2};

const myObject = {
  [keyA]: 'valueA',
  [keyB]: 'valueB'
};

myObject // Object {[object Object]: "valueB"}
```

上面代码中，`[keyA]`和`[keyB]`得到的都是`[object Object]`，所以`[keyB]`会把`[keyA]`覆盖掉，而`myObject`最后只有一个`[object Object]`属性。

## 方法的 name 属性

**核心**：函数内 有一个`name`属性，返回函数名。

对象方法也是函数，因此也有`name`属性。

```javascript
const person = {
  sayName() {
    console.log('hello!');
  },
};

person.sayName.name   // "sayName"
```

上面代码中，方法的`name`属性返回函数名（即方法名）。

## 属性的可枚举性和遍历

### 可枚举性

**作用**：判断**属性**是否可被遍历。



**对象的每个属性都有一个描述对象（Descriptor）**，用来控制该属性的行为。`Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。



```javascript
<script>
    let obj = {
        foo: '123',
        bar: 'wind'
    };
    var x = Object.getOwnPropertyDescriptor(obj, 'foo')
    console.log(x)
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210329/eIdhiICD1A.png?imageslim)



描述对象的`enumerable`属性，称为“**可枚举性**”，如果该属性为`false`，**就表示某些操作会忽略当前属性。**

目前，有四个操作会忽略`enumerable`为`false`的属性。

- `for...in`循环：**只遍历对象自身的和继承的可枚举的属性**。
- `Object.keys()`：返回对象自身的所有**可枚举**的属性的键名。
- `JSON.stringify()`：只串行化对象自身的**可枚举**的属性。
- `Object.assign()`： 忽略`enumerable`为`false`的属性，只拷贝对象自身的**可枚举**的属性。

这四个操作之中，前三个是 ES5 就有的，最后一个`Object.assign()`是 ES6 新增的。其中，**只有`for...in`会返回继承的属性**，其他三个方法都会忽略继承的属性，只处理对象自身的属性。

>**关于for in 只能遍历可枚举性的举例：**
>
>我们都知道for in可以循环数组，但是数组也是个对象，里面有 length属性，但是每次遍历时都没有遍历出length属性，因此我们猜测他的enumerable的值为false，下面验证一下。
>
>![mark](http://qiniu.wind-zhou.com/blog/210329/k16eL6dg0A.png?imageslim)
>
>
>
>for in不能遍历dom节点集合，我们可以验证一下。
>
>```js
><script>
>    var divLIst = document.getElementsByClassName("one");
>
>    for (var i in divLIst) {
>        console.log(i)
>    }
></script>
>```
>
>
>
>![mark](http://qiniu.wind-zhou.com/blog/210329/68cH9jBC7D.png?imageslim)
>
>





实际上，引入“可枚举”（`enumerable`）这个概念的最初目的，就是让某些属性可以规避掉`for...in`操作，不然所有内部属性和方法都会被遍历到。比如，对象原型的`toString`方法，以及数组的`length`属性，就通过“可枚举性”，从而避免被`for...in`遍历到。

```javascript
Object.getOwnPropertyDescriptor(Object.prototype, 'toString').enumerable
// false

Object.getOwnPropertyDescriptor([], 'length').enumerable
// false
```

上面代码中，`toString`和`length`属性的`enumerable`都是`false`，因此`for...in`不会遍历到这两个继承自原型的属性。



**另外，ES6 规定，所有 Class 的原型的方法都是不可枚举的。**

```javascript
Object.getOwnPropertyDescriptor(class {foo() {}}.prototype, 'foo').enumerable
// false
```

总的来说，操作中引入继承的属性会让问题复杂化，大多数时候，我们只关心对象自身的属性。所以，尽量不要用`for...in`循环，而用`Object.keys()`代替。



### 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

**（1）for...in**

`for...in`**循环遍历对象自身的和继承的可枚举属性**（不含 Symbol 属性）。

**（2）Object.keys(obj)**

`Object.keys`返回一个数组，**包括对象自身的（不含继承的）**所有可枚举属性（不含 Symbol 属性）的键名。

**（3）Object.getOwnPropertyNames(obj)**

`Object.getOwnPropertyNames`返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

**（4）Object.getOwnPropertySymbols(obj)**

`Object.getOwnPropertySymbols`返回一个数组，包含对象自身的所有 Symbol 属性的键名。

**（5）Reflect.ownKeys(obj)**

`Reflect.ownKeys`返回一个数组，包含对象自身的（不含继承的）所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。

以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。

```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```

上面代码中，`Reflect.ownKeys`方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性`2`和`10`，其次是字符串属性`b`和`a`，最后是 Symbol 属性。



**示例**: 简单实用Objec.keys()遍历对象

```js
<script>
    var list = {
        name: 'zhouzheng',
        age: 13,
        hobby: 'swim'
    }


    for (var i in list) {
        console.log(list[i])
    }
    var keyArr = Object.keys(list)
    var valueArr = Object.values(list)
    console.log(keyArr)
    console.log(valueArr)
</script>
```

![mark](http://qiniu.wind-zhou.com/blog/210329/6jaAfbk70j.png?imageslim)

## super 关键字

**核心**：super指向了当前对象的原型对象。



我们知道，`this`关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字`super`，**指向当前对象的原型对象。**

```javascript
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
    return super.foo;
  }
};

Object.setPrototypeOf(obj, proto);  //这句是实现了继承关系，将proto对象作为obj对象的原型。
obj.find() // "hello"
```

上面代码中，对象`obj.find()`方法之中，通过`super.foo`引用了原型对象`proto`的`foo`属性。



>在对象中：super就是`this._ _proto_ _`的简写。

**注意**，`super`关键字表示原型对象时，**只能用在对象的方法之中**，用在其他地方都会报错。

```javascript
// 报错
const obj = {
  foo: super.foo
}

// 报错
const obj = {
  foo: () => super.foo
}

// 报错
const obj = {
  foo: function () {
    return super.foo
  }
}
```

上面三种`super`的用法都会报错，因为对于 JavaScript 引擎来说，这里的`super`都没有用在对象的方法之中。第一种写法是`super`用在属性里面，第二种和第三种写法是`super`用在一个函数里面，然后赋值给`foo`属性。目前，只有对象方法的简写法可以让 JavaScript 引擎确认，定义的是对象的方法。

JavaScript 引擎内部，`super.foo`等同于`Object.getPrototypeOf(this).foo`（属性）或`Object.getPrototypeOf(this).foo.call(this)`（方法）。

```javascript
const proto = {
  x: 'hello',
  foo() {
    console.log(this.x);
  },
};

const obj = {
  x: 'world',
  foo() {
    super.foo();
  }
}

Object.setPrototypeOf(obj, proto);

obj.foo() // "world"
```

上面代码中，`super.foo`指向原型对象`proto`的`foo`方法，但是绑定的`this`却还是当前对象`obj`，因此输出的就是`world`。

## 对象的扩展运算符

《数组的扩展》一章中，已经介绍过扩展运算符（`...`）。ES2018 将这个运算符[引入](https://github.com/sebmarkbage/ecmascript-rest-spread)了对象。

### 解构赋值

**解构赋值的特点：**

- 解构赋值是将传过来的剩余的**可遍历**但**未被读取的属性**接受过来，并**组合成一个对象**
- **解构赋值必须是最后一个参数**
- **解构赋值的拷贝是浅拷贝**
- 解构赋值，**不能复制继承自原型对象的属性**

对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。



```javascript
<script>
    let {
        x,
        y,
        ...z
    } = {
        x: 1,
        y: 2,
        a: 3,
        b: 4
    };
    console.log(x)
    console.log(y)
    console.log(z)
</script>
```

![mark](http://qiniu.wind-zhou.com/blog/210329/h3DBGF98eH.png?imageslim)



上面代码中，变量`z`是解构赋值所在的对象。**它获取等号右边的所有尚未读取的键（`a`和`b`），将它们连同值一起拷贝过来。**



由于解构赋值要求等号右边是一个对象，所以如果等号右边是`undefined`或`null`，就会报错，因为它们无法转为对象。

```javascript
let { ...z } = null; // 运行时错误
let { ...z } = undefined; // 运行时错误
```

**解构赋值必须是最后一个参数**，否则会报错。

```javascript
let { ...x, y, z } = someObject; // 句法错误
let { x, ...y, ...z } = someObject; // 句法错误
```

上面代码中，解构赋值不是最后一个参数，所以会报错。



注意，**解构赋值的拷贝是浅拷贝**，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的引用，而不是这个值的副本。

```javascript
let obj = { a: { b: 1 } };
let { ...x } = obj;
obj.a.b = 2;
x.a.b // 2
```

上面代码中，`x`是解构赋值所在的对象，拷贝了对象`obj`的`a`属性。`a`属性引用了一个对象，修改这个对象的值，会影响到解构赋值对它的引用。



另外，**扩展运算符的解构赋值，不能复制继承自原型对象的属性。**

```javascript
let o1 = { a: 1 };
let o2 = { b: 2 };
o2.__proto__ = o1;
let { ...o3 } = o2;
o3 // { b: 2 }
o3.a // undefined
```

上面代码中，对象`o3`复制了`o2`，但是只复制了`o2`自身的属性，没有复制它的原型对象`o1`的属性。

下面是另一个例子。

```javascript
const o = Object.create({ x: 1, y: 2 });
o.z = 3;

let { x, ...newObj } = o;
let { y, z } = newObj;
x // 1
y // undefined
z // 3
```

上面代码中，变量`x`是单纯的解构赋值，所以可以读取对象`o`继承的属性；变量`y`和`z`是扩展运算符的解构赋值，只能读取对象`o`自身的属性，所以变量`z`可以赋值成功，变量`y`取不到值。ES6 规定，变量声明语句之中，如果使用解构赋值，扩展运算符后面必须是一个变量名，而不能是一个解构赋值表达式，所以上面代码引入了中间变量`newObj`，如果写成下面这样会报错。

```javascript
let { x, ...{ y, z } } = o;
// SyntaxError: ... must be followed by an identifier in declaration contexts
```

解构赋值的一个用处，是扩展某个函数的参数，引入其他操作。

```javascript
function baseFunction({ a, b }) {
  // ...
}
function wrapperFunction({ x, y, ...restConfig }) {
  // 使用 x 和 y 参数进行操作
  // 其余参数传给原始函数
  return baseFunction(restConfig);
}
```

上面代码中，原始函数`baseFunction`接受`a`和`b`作为参数，函数`wrapperFunction`在`baseFunction`的基础上进行了扩展，能够接受多余的参数，并且保留原始函数的行为。

### 扩展运算符

**作用**：对象的扩展运算符（`...`）用**于取出参数对象的所有可遍历属性，拷贝到当前对象之中**。



```javascript
let z = { a: 3, b: 4 };
let n = { ...z };
n // { a: 3, b: 4 }
```

**由于数组是特殊的对象，所以对象的扩展运算符也可以用于数组。**

```javascript
let foo = { ...['a', 'b', 'c'] };
foo
// {0: "a", 1: "b", 2: "c"}
```

**如果扩展运算符后面是一个空对象，则没有任何效果。**

```javascript
{...{}, a: 1}
// { a: 1 }
```

**如果扩展运算符后面不是对象，则会自动将其转为对象。**

```javascript
// 等同于 {...Object(1)}
{...1} // {}
```

上面代码中，扩展运算符后面是整数`1`，会自动转为数值的包装对象`Number{1}`。由于该对象没有自身属性，所以返回一个空对象。



**注：使用扩展运算符复制的对象属于深拷贝**  (重要)

```js
<script>
    let z = {
        a: 3,
        b: 4
    };
    let n = {...z
    };
    z.a = 10
    console.log(n)
</script>

```

![mark](http://qiniu.wind-zhou.com/blog/210329/7DAhbkF85C.png?imageslim)



下面的例子都是类似的道理。

```javascript
// 等同于 {...Object(true)}
{...true} // {}

// 等同于 {...Object(undefined)}
{...undefined} // {}

// 等同于 {...Object(null)}
{...null} // {}
```

但是，**如果扩展运算符后面是字符串，它会自动转成一个类似数组的对象，因此返回的不是空对象。**

```javascript
{...'hello'}
// {0: "h", 1: "e", 2: "l", 3: "l", 4: "o"}
```

**对象的扩展运算符等同于使用`Object.assign()`方法。**

```javascript
let aClone = { ...a };
// 等同于
let aClone = Object.assign({}, a);
```

上面的例子**只是拷贝了对象实例的属性**，如果想完整克隆一个对象，还拷贝对象原型的属性，可以采用下面的写法。

```javascript
// 写法一
const clone1 = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};

// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);

// 写法三
const clone3 = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)
```

上面代码中，写法一的`__proto__`属性在非浏览器的环境不一定部署，因此推荐使用写法二和写法三。

扩展运算符可以用于合并两个对象。

```javascript
let ab = { ...a, ...b };
// 等同于
let ab = Object.assign({}, a, b);
```

如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉。

```javascript
let aWithOverrides = { ...a, x: 1, y: 2 };
// 等同于
let aWithOverrides = { ...a, ...{ x: 1, y: 2 } };
// 等同于
let x = 1, y = 2, aWithOverrides = { ...a, x, y };
// 等同于
let aWithOverrides = Object.assign({}, a, { x: 1, y: 2 });
```

上面代码中，`a`对象的`x`属性和`y`属性，拷贝到新对象后会被覆盖掉。

**这用来修改现有对象部分的属性就很方便了。**

```javascript
let newVersion = {
  ...previousVersion,
  name: 'New Name' // Override the name property
};
```

上面代码中，`newVersion`对象自定义了`name`属性，其他属性全部复制自`previousVersion`对象。

如果把自定义属性放在扩展运算符前面，就变成了设置新对象的默认属性值。

```javascript
let aWithDefaults = { x: 1, y: 2, ...a };
// 等同于
let aWithDefaults = Object.assign({}, { x: 1, y: 2 }, a);
// 等同于
let aWithDefaults = Object.assign({ x: 1, y: 2 }, a);
```

与数组的扩展运算符一样，对象的扩展运算符后面可以跟表达式。

```javascript
const obj = {
  ...(x > 1 ? {a: 1} : {}),
  b: 2,
};
```

扩展运算符的参数对象之中，如果有取值函数`get`，这个函数是会执行的。

```javascript
let a = {
  get x() {
    throw new Error('not throw yet');
  }
}

let aWithXGetter = { ...a }; // 报错
```

上面例子中，取值函数`get`在扩展`a`对象时会自动执行，导致报错。

<hr>
## Object.is()

**作用**：判断是否相等



ES5比较两个值是否相等，只有两个运算符：相等运算符（`==`）和严格相等运算符（`===`）。它们都有缺点，前者会自动转换数据类型，后者的`NaN`不等于自身，以及`+0`等于`-0`。



JavaScript缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。

ES6提出“Same-value equality”（同值相等）算法，用来解决这个问题。`Object.is`就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

```javascript
Object.is('foo', 'foo')
// true
Object.is({}, {})
// false
```

​     

> Object.js()与‘===’的区别：
>
> **不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。**



```javascript
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

ES5可以通过下面的代码，部署`Object.is`。

```javascript
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
```

## Object.assign()

### 基本用法

**作用**：`Object.assign`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。



```javascript
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

**注意：**

- **出现同名属性会发生覆盖**
- 由于 合并时tartget会被改变，因此为了每个对象不被改变，我们可以在target前加一个空对象`{}`



**示例：**

```js
<script>
    var obj1 = {
        'name': 'zhou'
    };
    var obj2 = {
        age: 12
    }
    var obj3 = {
        hobby: 'swim'
    }

    var newObj = Object.assign(obj1, obj2, obj3)

    console.log(newObj)
    console.log(obj1)
    console.log(obj2)
    console.log(obj3)
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/4J25F6l4lE.png?imageslim)

前面增加{ }之后

```js
    var newObj = Object.assign({}, obj1, obj2, obj3)
```



![mark](http://qiniu.wind-zhou.com/blog/210330/LLd3d43kLD.png?imageslim)

## Object.getOwnPropertyDescriptors()

**作用**：输出所有属性以及他们的descriptor对象。

ES5有一个`Object.getOwnPropertyDescriptors`方法，返回某个对象所有属性的描述对象（descriptor）。

>`Object.getOwnPropertyDescriptor`只返回某一个，而Object.getOwnPropertyDescriptors()返回的是所有属性的描述对象，并且语法有点不同。

**示例：**

```js
<script>
    var obj = {
        name: 'wind-zhou',
        age: 24,
        sayName() {
            console.log(this.name)
        }
    }

    console.log(Object.getOwnPropertyDescriptors(obj))

    console.log(Object.getOwnPropertyDescriptor(obj, 'age'))
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/f5d9m3ab8a.png?imageslim)





## `__proto__`属性，Object.setPrototypeOf()，Object.getPrototypeOf()

**作用**：读取和设置对象的原型。

`_ _proto_ _`并不是公开的API，最好使用Object.setPrototypeOf()和Object.getPrototypeOf()用来设置和读取原型对象。

**示例：**

```js
<script>
    var obj1 = {
        name: 'wind-zhou'
    }

    var obj2 = {
        age: 24,
        hobby: 'swim'
    }


    Object.setPrototypeOf(obj1, obj2);   //设置obj1的原型为obj2 ，实现了原型链继承
    console.log(Object.getPrototypeOf(obj1)) //读取
    console.log(obj1.age)  //检测是否实现了继承
</script>

```

输出：![mark](http://qiniu.wind-zhou.com/blog/210330/LHliAFH50a.png?imageslim)



`__proto__`属性（前后各两个下划线），用来读取或设置当前对象的`prototype`对象。目前，所有浏览器（包括 IE11）都部署了这个属性。



## Object.keys()，Object.values()，Object.entries()

### Object.keys()

ES5 引入了`Object.keys`方法，**返回一个数组**，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。

**示例：**

```js
<script>
    var obj = {
        name: 'wind-zhou',
        age: 24,
        sayName() {
            console.log(this.name)
        }
    }
    console.log(Object.keys(obj))
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/CIb8FhJ8Lj.png?imageslim)





ES2017 [引入](https://github.com/tc39/proposal-object-values-entries)了跟`Object.keys`配套的`Object.values`和`Object.entries`，作为遍历一个对象的补充手段，供`for...of`循环使用。



**示例：**

```js
<script>
    var obj = {
        name: 'wind-zhou',
        age: 24,
        sayName() {
            console.log(this.name)
        }
    }
    console.log(Object.keys(obj))
    console.log(Object.values(obj))
    console.log(Object.entries(obj))
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/b57Kh2ACdE.png?imageslim)



>for...of用来遍历可遍历的对象（拥有iterator接口），他和for...in使用方法基本相同，只是比for...in更简洁一些。
>
>**所谓的可遍历是指拥有iterable接口的对象**，例如数组有这个接口，因此是可遍历的，json则是不可遍历的，使用for...of遍历时会报错。



**示例**：使用for...of遍历 object.entries()

```js
<script>
    var obj = {
            name: 'wind-zhou',
            age: 24,
            sayName() {
                console.log(this.name)
            }
        }
        // console.log(Object.keys(obj))
        // console.log(Object.values(obj))

    for (let i of Object.entries(obj)) {
        console.log(i)

    }

    for (let [i, v] of Object.entries(obj)) { //遍历出数组后，并解构赋值
        console.log(i, v)
    }
</script>
```



输出:

![mark](http://qiniu.wind-zhou.com/blog/210330/CDj42gDCDE.png?imageslim)



### Object.values()

`Object.values`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。

```javascript
const obj = { foo: 'bar', baz: 42 };
Object.values(obj)
// ["bar", 42]
```



如果`Object.values`方法的参数是一个字符串，会返回各个字符组成的一个数组。

```javascript
Object.values('foo')
// ['f', 'o', 'o']
```

上面代码中，**字符串会先转成一个类似数组的对象**。字符串的每个字符，就是该对象的一个属性。因此，`Object.values`返回每个属性的键值，就是各个字符组成的一个数组。

**如果参数不是对象，`Object.values`会先将其转为对象。**由于数值和布尔值的包装对象，都不会为实例添加非继承的属性。所以，`Object.values`会返回空数组。

```javascript
Object.values(42) // []
Object.values(true) // []
```

### Object.entries()

`Object.entries()`方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。

这里不再演示，前面已有示例。

**如果原对象的属性名是一个 Symbol 值，该属性会被忽略。**

```javascript
Object.entries({ [Symbol()]: 123, foo: 'abc' });
// [ [ 'foo', 'abc' ] ]
```



`Object.entries`的基本用途是遍历对象的属性。

```javascript
let obj = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj)) {
  console.log(
    `${JSON.stringify(k)}: ${JSON.stringify(v)}`
  );
}
// "one": 1
// "two": 2
```

`Object.entries`方法的另一个用处是，将对象转为真正的`Map`结构。

```javascript
const obj = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj));
map // Map { foo: "bar", baz: 42 }
```























