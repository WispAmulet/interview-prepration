# Fundamentals

## JS

+ [Variable Types](#variable-types)
+ [Global object](#global-object)
+ [Constructor](#constructor)

### Variable Types

```js
// Primitive type (原始类型)
// number, string, boolean, null, undefined and symbol(es6)
// 也可以把 null 和 undefined 看成一种特殊值

// Object type (对象类型)
// object, array, function
// array 和 function 都是特殊的对象

// 问题：如何确定一个值的类型？
// 1. typeof
typeof 123; // "number"
typeof '123'; // "string"
typeof true; // "boolean"
typeof null; // "object" 历史原因，在最初的版本中，null 是 object 的一种特殊值
typeof undefined; // "undefined"
typeof {}; // "object"
typeof []; // "object" 数组是一种特殊的 object
function f() {}
typeof f; // "function"
// 2. instanceof
var o = {};
var a = [];
o instanceof Array; // false
a instanceof Array; // true
// 也可以这样声明 😂
var [o, a] = [{}, []];
// 这种写法可以不引入第三个变量的情况下交换两个变量的值 (es6)
[o, a] = [a, o];
// 3. Object.prototype.toSting()
// TODO
```

```js
// number
// Javascript 不区分整数值和浮点数值，Javascript 中所有数字都是 64 位浮点格式
// 十进制
var decimal = 3.14;
// 十六进制
var hexadecimal = 0xf00d;
// ES6 新增了二进制和八进制的表达法 (es6)
var binary = 0b1010;
var octal = 0o744;
// 科学计数法
123e3 // 123000
123e+3 // 123000
123e-3 // 0.123
// 特殊数值
// 0 和 -0
0 === -0 // true
0 === +0 // true
-0 === +0 // true
0 === +0 === -0 // false
1 === 1 === 1 // ???
// from stackoverflow
// 1 === 1 === 1 is evaluated as (1 === 1) === 1 which is true === 1 which is false
// 0 和 -0 的区别
(1 / 0) === (1 / -0) // ???
// Infinity is not equal with -Infinity, so it's false
// NaN
5 - 'a'; // NaN
0 / 0; // NaN
typeof NaN; // 'number' // NaN 是一个特殊数值，但仍然是 number
NaN === NaN; // false // NaN 不等于它本身
NaN + 30; // NaN // NaN 与任何数值的运算都等于它本身
// Infinity 和 -Infinity
1 / 0; // Infinity // 用来表示正无穷
-1 / 0; // -Infinity // 用来表示负无穷
Infinity === -Infinity; // false

// 问题：Javascript 如何转换进制？
// 其它进制转化为十进制
parseInt(11011001, 2) // 217
parseInt(331, 8) // 217
parseInt(d9, 16); // 217
// 十进制转化为其它进制，注意这里得到的是字符串
var num = 217;
num.toString(2); // "11011001"
num.toString(8); // "331"
var hexStr = num.toString(16); // "d9"
var hex = parseInt(hexStr); // d9
// 😂
parseInt(num.toString(2), 2); // ???
// 217
```

```js
// string
'abc';
"abc";
`abc`; // (es6)
// 反转引号
"I\'m OK." // I'm OK.
// 字符串连接
var hello = 'hello';
var world = 'world';
var say = hello + ' ' + world;
var yell = `${hello} ${world}`; // (es6)
// 类似数组的地方
hello[0]; // 'h'
hello[100]; // undefined
hello.length; // 5
// 但只可以访问，并不能直接修改它们的值
```

```js
// boolean
true;
false;
// 这些运算符会返回布尔值
// && (And), || (Or)
// ! (Not)
// ===, !==, ==, !=
// >, >=, <, <=
// 这些值会自动判断为 false
// undefined, null, false, 0, NaN, ''

// 问题
if ('') { console.log('hello'); } // ???
// console.log() 并不会执行，但函数会默认返回 undefined
if ([]) { console.log('hello'); } // ???
if ({}) { console.log('hello'); } // ???
// console.log() 都会执行 😂
```

```js
// null & undefined

// 问题：null 和 undefined 有什么不同？
// 历史原因，Javascript 最初只有 null，null 表示一个空的对象
// null 可以转为 0
Number(null); // 0
5 + null; // 5
// undefined 不会
Number(undefined); // NaN
5 + undefined; // NaN
// undefined 通常理解为未定义
// 如声明了变量没有赋值
var i;
i; // undefined // 如果使用 const 声明一个变量必须赋值而 let 不用 (es6)
// 调用函数的参数没有提供
function f(x) { return x; }
f(); // undefined
// 对象没有赋值的属性
var obj = {};
obj.hello; // undefined
// 函数没有返回值，默认返回 undefined
function f() {}
f() // undefined
```

```js
// symbol (es6)
```

```js
// 什么是对象？
// 对象是属性（property）的集合，属性由 key: value 构成，定义在属性上的函数也叫做方法（method）

// object
// 普通的对象是 key: value 的无序集合
// array
// 数组是一种特殊的对象，表示带 index 的有序集合。可以理解为数组是 index: value 形式的一种对象
// function
// 函数也是一种特殊的对象。函数是具有与它相关联的可执行代码的对象，通过调用函数来运行可执行代码，返回运算结果。
```

  > 参考资料
  + Javascript 权威指南第三版
  + [Javascript 标准参考教程](http://javascript.ruanyifeng.com/#grammar)
  + [ES6 for everyone](https://es6.io)
  + [ECMAScript 6 入门](http://es6.ruanyifeng.com/#README)

### Global object

### Constructor


