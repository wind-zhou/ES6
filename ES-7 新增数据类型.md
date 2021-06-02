# lesson-7 新增数据类型

##symbol

symbol是一种新增的原始（基本）数据类型。

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

**作用：主要作为对象的属性名，防止属性名和方法名冲突问题。**



**基本语法：**

```js
<script>
    let s1 = Symbol('s1');
    let s2 = Symbol('s2'); //加这个参数是为了区分两个symbol，转成字符串是可以区分，主要作为对象的属性名

    console.log(s1, s2)
</script>
```

输出：![mark](http://qiniu.wind-zhou.com/blog/210330/ccjAeml7g6.png?imageslim)



上面代码中，`s1`和`s2`是两个 Symbol 值。如果不加参数，它们在控制台的输出都是`Symbol()`，不利于区分。

**有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。**

> 注意，`Symbol`函数前**不能使用`new`命令**，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。



**具体使用**：作为对象的属性

>##Symbol理解和使用场景
>
>ES 6 引入了一个新的数据类型 Symbol，它是用来做什么的呢？
>
>为了说明 Symbol 的作用，我们先来描述一个使用场景。
>
>我们在做一个游戏程序，用户需要选择角色的种族。
>
>```js
>var race = {
>  protoss: 'protoss', // 神族
>  terran: 'terran', // 人族
>  zerg: 'zerg' // 虫族
>}
>
>function createRole(type){
>  if(type === race.protoss){创建神族角色}
>  else if(type === race.terran){创建人族角色}
>  else if(type === race.zerg){创建虫族角色}
>}
>```
>
>那么用户选择种族后，就需要调用 createRole 来创建角色：
>
>```js
>// 传入字符串
>createRole('zerg') 
>// 或者传入变量
>createRole(race.zerg)
>```
>
>一般传入字符串被认为是不好的做法，所以使用 createRole(race.zerg) 的更多。
>
>如果使用 createRole(race.zerg)，那么聪明的读者会发现一个问题：race.protoss、race.terran、race.zerg 的值为多少并不重要。
>
>
>
>改为如下写法，对 createRole(race.zerg) 毫无影响：
>
>```js
>var race = {
>  protoss: 'askdjaslkfjas;lfkjas;flkj', // 神族
>  terran: ';lkfalksjfl;askjfsfal;skfj', // 人族
>  zerg: 'qwieqwoirqwoiruoiwqoisrqwroiu' // 虫族
>}
>```
>
>也就是说：
>
>**race.zerg 的值是多少无所谓，只要它的值跟 race.protoss 和 race.terran 的值不一样就行。**
>
>
>
>Symbol 的用途就是如此：Symbol 可以创建一个独一无二的值（但并不是字符串）。
>
>用 Symbol 来改写上面的 race：
>
>```js
>var race = {
>  protoss: Symbol(),
>  terran: Symbol(),
>  zerg: Symbol()
>}
>
>race.protoss !== race.terran // true
>race.protoss !== race.zerg // true
>```
>
>你也可以给每个 Symbol 起一个名字：
>
>```js
>var race = {
>  protoss: Symbol('protoss'),
>  terran: Symbol('terran'),
>  zerg: Symbol('zerg')
>}
>```
>
>不过这个名字跟 Symbol 的值并没有关系，你可以认为这个名字就是个注释。如下代码可以证明 Symbol 的名字与值无关：
>
>```js
>var a1 = Symbol('a')
>var a2 = Symbol('a')
>a1 !== a2 // true
>```
>
>如果你觉得我说得还是太复杂了，看不懂，你可以记一句话：
>
>Symbol 生成一个全局唯一的值。
>
>
>
>**推荐阅读：**
>
>> https://zhuanlan.zhihu.com/p/22652486
>>
>> https://www.nblogs.com/archives/489/



>##注意事项：
>
>Symbol
>声明
>var s1 = Symbol();
>var s2 = Symbol();
>注意：
>1、Symbol是一种新原始数据类型；
>2、Symbol前**不能使用new关键字**，否则会报错；这是原因Symbol是一个原始类型的值，而不是对象，所以不能为它添加属性是类似于字符串的数据类型；
>3、Symbol函数可以接受一个字符串参数，表示对Symbol实例的描述，主要是**为了在控制台显示或考转为字符串(s1.toString())，容易区分**；
>4、s1和s2都Symbol函数的返回值，而且参数相同，但是它们是不相等的；
>5、**Symbol函数返回值不能与其它类型的值进行运算**，会报错；(s1 + "hi Symbol");
>6、Symbol函数返回值可以显示转为字符串如：String(s1)或s2.toString();
>7、Symbol函数返回值可以转移为布尔值(Boolean(s1)),但是不能转为数值
>8、**Symbol函数返回值作为对象属性名进，不能用点运算符；**
>
>



## set

### 基本用法

ES6 提供了新的数据结构 Set。**它类似于数组**，但是成员的值都是唯一的，**没有重复的值**。(**可用作数组去重**)

声明：使用new 进行声明，例如`let a=new Set()`。(`Set`本身是一个构造函数，用来生成 Set 数据结构。)

**由于 Set 结构没有键名，只有键值。（set的键名和键值为同一个值。）**

**示例：**

```js
<script>
    var set = new Set([1, 2, 3, 4, 4]);
    console.log(set)
</script>
```

输出：![mark](http://qiniu.wind-zhou.com/blog/210330/m38fkIiAb7.png?imageslim)

上面实例化的set，是Set类型，且没有重复。

`Set`函数**可以接受一个数组**（**或者具有 iterable 接口的其他数据结构**）作为参数，用来初始化。

上面例子中，就接受了一个数组作为参数。

```javascript
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56

// 类似于
const set = new Set();
document
 .querySelectorAll('div')
 .forEach(div => set.add(div));
set.size // 56
```

上面代码中，例一和例二都是`Set`函数接受数组作为参数，例三是接受**类数组**的对象作为参数。**（说明了类数组也拥有iterable接口）**

**应用**：

（1）数组去重

```js
<script>
    var a = [45, 22, 75, 32, 8, 12, 22, 45, 22, 22];

    var b = [...new Set(a)];
    console.log(b)
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/1E1GcIKhca.png?imageslim)

（2）字符串去除重复字符串

```js
<script>
    var str = 'abaaccd';
    var newStr = [...new Set(str)].join('')
    console.log(newStr)
</script>
```



![mark](http://qiniu.wind-zhou.com/blog/210330/fDdj4if6bK.png?imageslim)



> 这个本质还是数组的去重，只是中间转换了一下。
>
> 让我惊讶的是，Set里竟然也**可以穿字符串作为参数**。看来**字符串也实现了iterable接口**。



向 Set 加入值的时候，不会发生类型转换，所以`5`和`"5"`是两个不同的值。Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（`===`），主要的区别是向 Set 加入值时认为`NaN`等于自身，而精确相等运算符认为`NaN`不等于自身。

```javascript
let set = new Set();
let a = NaN;
let b = NaN;
set.add(a);
set.add(b);
set // Set {NaN}
```

上面代码向 Set 实例添加了两次`NaN`，但是只会加入一个。这表明，在 Set 内部，两个`NaN`是相等的。

**另外，两个对象总是不相等的。**

```javascript
let set = new Set();

set.add({});
set.size // 1

set.add({});
set.size // 2
```

上面代码表示，由于两个空对象不相等，所以它们被视为两个值。



### set的属性和方法

我们先输出一个Set对象看看。

![mark](http://qiniu.wind-zhou.com/blog/210330/gmAJbF5fEL.png?imageslim)

从上面我们可以看出，set就是个对象，他的原型的原型指向了Object。

#### 属性

Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

#### 方法

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。

- `Set.prototype.add(value)`：添加某个值，**返回 Set 结构本身**。
- `Set.prototype.delete(value)`：删除某个值，**返回一个布尔值**，表示删除是否成功。
- `Set.prototype.has(value)`：**返回一个布尔值**，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：**清除所有成员，没有返回值**。

上面这些属性和方法的实例如下。

```javascript
s.add(1).add(2).add(2);
// 注意2被加入了两次

s.size // 2

s.has(1) // true
s.has(2) // true
s.has(3) // false

s.delete(2);
s.has(2) // false
```

下面是一个对比，看看在判断是否包括一个键上面，`Object`结构和`Set`结构的写法不同。

```javascript
// 对象的写法
const properties = {
  'width': 1,
  'height': 1
};

if (properties[someName]) {
  // do something
}

// Set的写法
const properties = new Set();

properties.add('width');
properties.add('height');

if (properties.has(someName)) {
  // do something
}
```

**`Array.from`方法可以将 Set 结构转为数组。**

```javascript
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了去除数组重复成员的另一种方法。

```javascript
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]
```



**简单总结**：数组去重的方法



```js
<script>
    let arr = [12, 23, 44, 12, 56, 73, 23, 12, 56];


    //使用indexOf 同理可以使用includes
    function dedupe1(arr) {
        var newArr = [];

        for (let j in arr) {
            if (newArr.includes(arr[j]) == false) {
                newArr.push(arr[j])
            }
        }

        return newArr;
    }


    //使用Set
    function dedupe2(arr) {
        var newArr = [...new Set(arr)];
        return newArr;
    }

    //使用Set+Array.form
    function dedupe3(arr) {
        var newArr = Array.from(new Set(arr))
        return newArr;
    }
    var newArr1 = dedupe1(arr);
    var newArr2 = dedupe2(arr);
    var newArr3 = dedupe3(arr);
    console.log(newArr1)
    console.log(newArr2)
    console.log(newArr3)
</script>
```



输出：![mark](http://qiniu.wind-zhou.com/blog/210330/J1hH0l6emB.png?imageslim)

### 遍历操作

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：**返回键值对的遍历器**
- `Set.prototype.forEach()`：使用回调函数遍历每个成员

需要特别指出的是，`Set`的遍历顺序就是插入顺序。这个特性有时非常有用，比如使用 Set 保存一个回调函数列表，调用时就能保证按照添加顺序调用。

**（1）**

`keys`方法、`values`方法、`entries`方法返回的**都是遍历器对象**（详见《Iterator 对象》一章）。

**由于 Set 结构没有键名，只有键值**（或者说键名和键值是同一个值），**所以`keys`方法和`values`方法的行为完全一致。**



**示例：**

```js
<script>
    let set = new Set([23, 'red', true]);
    console.log(set.keys())
    console.log(set.values())
    console.log(set.entries())
</script>
```

<img src="http://qiniu.wind-zhou.com/blog/210330/m5fe4AGmd6.png?imageslim" alt="mark" style="zoom:25%;" />



> 这几个返回的遍历器对象都可以使用for...of遍历

**示例**：使用for..of遍历

```js
<script>
    let set = new Set([23, 'red', true]);

    for (let i of set.keys()) {
        console.log(i)
    }

    for (let i of set.values()) {
        console.log(i)
    }

    for (let i of set.entries()) {
        console.log(i)
    }
</script>
```

输出：

![mark](http://qiniu.wind-zhou.com/blog/210330/i8LBe3kbg1.png?imageslim)

> 上面代码中，`entries`方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。



**注意：**

**Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的`values`方法。**

```javascript
Set.prototype[Symbol.iterator] === Set.prototype.values
// true
```

这意味着，**可以省略`values`方法**，直接用`for...of`循环遍历 Set。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```



**（2）`forEach()`**

Set 结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，**没有返回值**







## map

### Map结构的目的和基本用法



**map和对象的区别：**

JavaScript的对象（Object），本质上是键值对的集合（Hash结构），但是**传统上只能用字符串当作键**。这给它的使用带来了很大的限制。

为了解决这个问题，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“**键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键**。



创建：



>map set 数组 底层是怎么存储的？

