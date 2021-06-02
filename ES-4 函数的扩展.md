# lesson-4 函数的扩展

1. [函数参数的默认值](https://es6.ruanyifeng.com/#docs/function#函数参数的默认值)
2. [rest 参数](https://es6.ruanyifeng.com/#docs/function#rest 参数)
3. [严格模式](https://es6.ruanyifeng.com/#docs/function#严格模式)
4. [name 属性](https://es6.ruanyifeng.com/#docs/function#name 属性)
5. [箭头函数](https://es6.ruanyifeng.com/#docs/function#箭头函数)

##  函数的默认值

**ES6 之前，不能直接为函数的参数指定默认值**，只能采用变通的方法。（通过自定义逻辑判断）

```javascript
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello World
```

上面代码检查函数`log`的参数`y`有没有赋值，如果没有，则指定默认值为`World`。



**这种写法的缺点在于**，如果参数`y`赋值了，但是对应的布尔值为`false`，则该赋值不起作用。就像上面代码的最后一行，参数`y`等于空字符，结果被改为默认值。

为了避免这个问题，通常需要先判断一下参数`y`是否被赋值，如果没有，再等于默认值。

```javascript
if (typeof y === 'undefined') {
  y = 'World';
}
```

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello  这里第二个参数赋值为“”也算是赋值了
```



>**最佳实践：**
>
>### 参数默认值的位置
>
>通常情况下，定义了默认值的参数，**应该是函数的尾参数**。因为这样比较容易看出来，到底省略了哪些参数。如果非尾部的参数设置默认值，实际上这个参数是没法省略的。
>
>如果函数里有默认值，则把默认值放在尾参数的位置，要不然就得用undefined替代。



**注：参数变量是默认声明的，所以不能用`let`或`const`再次声明。**

```javascript
function foo(x = 5) {
  let x = 1; // error
  const x = 2; // error
}
```

上面代码中，参数变量`x`是默认声明的，在函数体中，不能用`let`或`const`再次声明，否则会报错。

**注：使用参数默认值时，函数不能有同名参数。**

## rest 参数

ES6 引入 rest 参数（形式为`...变量名`），**用于获取函数的多余参数**，这样就不需要使用`arguments`对象了。

rest 参数搭配的变量是一个数组，**该变量将多余的参数放入数组中。**



```js
<script>
    function test(x, y, ...z) {
        console.log(x)
        console.log(y)
        console.log(z)
    }
    test(4, 2, 'zhou', 77, true)
</script>
```

![mark](http://qiniu.wind-zhou.com/blog/210327/4b3dhjeB0e.png?imageslim)



**注**：rest参数必须写在参数最后，且一个函数中只能有一个。

## 严格模式

从 ES5 开始，函数内部可以设定为严格模式。

```javascript
function doSomething(a, b) {
  'use strict';
  // code
}
```

ES2016 做了一点修改，规定只要函数参数使用了**默认值、解构赋值、或者扩展运算符**，那么函数内部就不能显式设定为严格模式，否则会报错。

```javascript
// 报错
function doSomething(a, b = a) {
  'use strict';
  // code
}

// 报错
const doSomething = function ({a, b}) {
  'use strict';
  // code
};

// 报错
const doSomething = (...a) => {
  'use strict';
  // code
};

const obj = {
  // 报错
  doSomething({a, b}) {
    'use strict';
    // code
  }
};
```



**严格模式和非严格模式的区别：**

- 严格模式下，delele运算符后跟随非法标识符国dete不存在的标识符，会抛出语法错误:非严格
  模式下，会静默失败并返回false
- 严格模式中，**对象**直接量中**定义同名属性会抛出语法错误**: 非严格模式不会报错
- 严格模式中，函数形参存在同名的，抛出错误;非严格模式不会
- **严格模式不允许八进制整数直接量**(如: 023)
- 严格模式中，**arguments对象是传入函数内实参列表的静态副本**;非严格模式下，**arguments对象里**
  **的元素和对应的实参是指向同一个值的引用**
- 严格模式中eval和arguments当做关键字，它们不能被赋值和用作变量声明
- 严格模式会限制对调用栈的检测能力， 访问arguments.callee caller会抛出异常
- 严格模式 变量必须先声明，直接给变量赋值， 不会隐式创建全局变量， 不能用wih,
- **严格模式中call apply传入null udefind保持原样不被转换为window**



> es2016中做了一点修改，**规定只要参数使用了默认值、解构赋值、或者扩展运算符，就不能显式指定严格模式。**



## name属性

**作用：函数的`name`属性，返回该函数的函数名。**



```javascript
function foo() {}
foo.name // "foo"
```

这个属性早就被浏览器广泛支持，但是直到 ES6，才将其写入了标准。

需要注意的是，ES6 对这个属性的行为做出了一些修改。如果将一个匿名函数赋值给一个变量，ES5 的`name`属性，会返回空字符串，而 ES6 的`name`属性会返回实际的函数名。

```javascript
var f = function () {};

// ES5
f.name // ""

// ES6
f.name // "f"
```

上面代码中，变量`f`等于一个匿名函数，ES5 和 ES6 的`name`属性返回的值不一样。

如果将一个具名函数赋值给一个变量，则 ES5 和 ES6 的`name`属性都返回这个具名函数原本的名字。

```javascript
const bar = function baz() {};

// ES5
bar.name // "baz"

// ES6
bar.name // "baz"
```

`Function`构造函数返回的函数实例，`name`属性的值为`anonymous`。

```javascript
(new Function).name // "anonymous"
```

`bind`返回的函数，`name`属性值会加上`bound`前缀。

```javascript
function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```

## 箭头函数

箭头函数是es5中函数的简写。

箭头函数只能创建成匿名函数。

### 基本用法

ES6 允许使用“箭头”（`=>`）定义函数。

```javascript
var f = v => v;

// 等同于
var f = function (v) {
  return v;
};
```

（1）括号里是函数放入参数，如果只有一个参数，则概括好可以省略（见上例）、

（2）如果函数体只有一行，且函数体中为函数的返回值，则函数体的大括号可以省略

```js
    let fn = (a, b) => a + b
    console.log(fn(23, 10))

//上面代码等同于
    let fn = (a, b) => {
        return a + b
    }
    console.log(fn(23, 10))
```



```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

**如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用`return`语句返回。**



### 使用注意点

箭头函数有几个使用注意点。

（1）函数体内的`this`对象，**就是定义时所在的对象，而不是使用时所在的对象。**

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。



面四点中，第一点尤其值得注意。`this`对象的指向是可变的，但是在箭头函数中，它是固定的。

```javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
```

上面代码中，`setTimeout()`的参数是一个箭头函数，**这个箭头函数的定义生效是在`foo`函数生成时**，而它的真正执行要等到 100 毫秒后。如果是普通函数，执行时`this`应该指向全局对象`window`，这时应该输出`21`。但是，箭头函数导致`this`总是指向函数定义生效时所在的对象（本例是`{id: 42}`），所以打印出来的是`42`。



>这里再提一嘴call方法：
>
>call和apply方法的作用有二：
>
>（1）函数调用
>
>（2）改变this的指向，call常用来实现继承
>
>今天我才弄明白，foo.call({ id: 42 });的实现原理，其实是将foo函数内部的this替换成对象{ id: 42 }里的this，换个说法也就是foo函数内部的this指向了{ id: 42 }对象。
>
>实现继承时Father.call(this),也是同理，其实就是调用Father构造函数，并将Father函数内部的this替换成子类的this，也就是指向了子类。



