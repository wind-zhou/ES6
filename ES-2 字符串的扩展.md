# lesson-2 字符串的扩展

## 字符串查找

传统上，JavaScript 只有`indexOf`方法，**可以用来确定一个字符串是否包含在另一个字符串**中。ES6 又提供了三种新方法。

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

**示例：**



```javascript
let s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

**这三个方法都支持第二个参数，表示开始搜索的位置。**

```javascript
 let s = 'Hello world!';

console.log(s.startsWith('e', 1)) //true
console.log(s.endsWith('w', 7)) //true  注意，这个第二个参数不再是索引值，而是前多少个字母，是字母的个数，改成6贼为false
console.log(s.includes('o', 8)) //fal
```



## 字符串添加

###**字符串复制**

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次。

```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

参数如果是小数，会被取整（向下取整）。

```javascript
'na'.repeat(2.9) // "nana"
```

###**字符串补长**



ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。

`padStart()`用于头部补全，`padEnd()`用于尾部补全。



```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

上面代码中，`padStart()`和`padEnd()`一共接受两个参数，**第一个参数是字符串补全生效的最大长度**，**第二个参数是用来补全的字符串。**



（1）如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。

```javascript
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```

（2）如果用来补全的字符串与原字符串，**两者的长度之和超过了最大长度，则会截去超出位数的补全字符串**。

```javascript
'abc'.padStart(10, '0123456789')
// '0123456abc'
```

（3）如果省略第二个参数，默认使用**空格补全长度**。

```javascript
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```



**用途：**

`padStart()`的常见用途是为数值补全指定位数。下面代码生成 10 位的数值字符串。

```javascript
'1'.padStart(10, '0') // "0000000001"
'12'.padStart(10, '0') // "0000000012"
'123456'.padStart(10, '0') // "0000123456"
```

另一个用途是提示字符串格式。

```javascript
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```



## 去除空格：trimStart()，trimEnd()

[ES2019](https://github.com/tc39/proposal-string-left-right-trim) 对字符串实例新增了`trimStart()`和`trimEnd()`这两个方法。它们的行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。

**它们返回的都是新字符串，不会修改原始字符串。**

```javascript
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

上面代码中，`trimStart()`只消除头部的空格，保留尾部的空格。`trimEnd()`也是类似行为。

除了空格键，这两个方法对字符串头部（或尾部）的 tab 键、换行符等不可见的空白符号也有效。

浏览器还部署了额外的两个方法，`trimLeft()`是`trimStart()`的别名，`trimRight()`是`trimEnd()`的别名。

## 模板字符串

模板字符串（template string）是**增强版的字符串**，用**反引号**（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

- 1.代码中的模板字符串，都是**用反引号表示**。如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
- 2.如果使用模板字符串表示多行字符串，**所有的空格和缩进都会被保留在输出之中**（原样式输出，类比pre）。
- 3.**模板字符串中嵌入变量**，需要将变量名写在`${}`之中（**最重要**）。
- 4.大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。
- 5.模板字符串之中**还能调用函数**。
- 6.如果模板字符串中的变量没有声明，将报错。
- 7.模板字符串甚至还能嵌套。

>目前为止，模板template给我们最大处的便捷就是不用再手动进行字符串的拼接了。

**示例：**

```js
<script>
    let name = 'zhouzheng';
    let age = 24;
    //传统模式
    document.getElementById("con").innerHTML = "我的名字为：" + name + "我的年龄为：" + age + "<br>";
    //模板字符串模式
    //里面可以加表达式
    document.getElementById("con").innerHTML += `我的名字为: ${name}我的年龄为：${age>20?'符合要求':'不符合'}`;
</script>
```

**输出：**

![mark](http://qiniu.wind-zhou.com/blog/210327/delje9B0Ae.png?imageslim)









