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
[o, a] = [a, o]; // 这种写法可以不引入第三个变量的情况下交换两个变量的值 (es6)
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
// 1.Symbols are unique identifiers
const dog = Symbol('Dog'); // 这里的 'Dog' 不代表值，只是一种 descriptor，用来描述创建的这个 symbol
console.log(dog); // Symbol(Dog)
const dog2 = Symbol('Dog'); // Symbol(Dog)
dog === dog2; // ??? false, because symbols are absolutely unique

// 2.Use symbols to avoid naming collision
const classRoom = {
  'Mark': { grade: 50, gender: 'male' },
  'Olivia': { grade: 60, gender: 'female' },
  'Olivia': { grade: 80, gender: 'female' }, // naming collision here
}
// =>
const classRoom = {
  [Symbol('Mark')]: { grade: 50, gender: 'male' },
  [Symbol('Olivia')]: { grade: 60, gender: 'female' },
  [Symbol('Olivia')]: { grade: 80, gender: 'female' }, // use symbol to avoid this
}

// 3.Symbols are not enumerable which means we cannot loop over them
for (person in classRoom) {
  console.log(person); // nothing
}
const syms = Object.getOwnPropertySymbols(classRoom); // just like using Object.keys() if it's a normal object
console.log(syms); // [ Symbol(Mark), Symbol(Olivia), Symbol(Olivia) ]
const data = syms.map(sym => classRoom[sym]);
console.log(data);
```

```js
// 什么是对象？
// 对象是属性（property）的集合，属性由 key: value 构成，定义在属性上的函数也叫做方法（method）
```

```js
// object
// 普通的对象是 key: value 的无序集合

// 创建对象
var obj = {};
var obj2 = new Object(); // {}
var obj3 = Object.create({ x: 1, y: 2 }); // { x: 1, y: 2 } // 该方法的第一个参数是创建对象的原型对象，第二个参数可选，用来进一步描述对象的属性
// 问题：如何通过 Object.create() 创建一个空对象
var obj4 = Object.create(Object.prototype); // {}

// 读取和设置属性
var obj = {
  say: 'hello world'
};
obj.say; // "hello world"
obj.['say']; // "hello world" // 如果属性名是一个变量，必须使用 [] 这种方式
obj.say = 'say hi';

// 删除属性
var obj = {
  name: 'dog',
  age: 7
};
delete obj.age; // true // 无论这个属性存不存在都返回 true，只有删除可配置性为 false 时的属性时返回 false 或报错。继承的属性无法删除，但也会返回 true
console.log(obj); // { name: 'dog' }

// 检测属性
var obj = {
  x: 1,
  y: 2,
  z: 3
};
// 1. in operator
'x' in obj; // true
'a' in obj; // false
'toString' in obj; // true // in 运算符无法区别自有属性和继承属性
// 2. hasOwnProperty()
obj.hasOwnProperty('x'); // true
obj.hasOwnProperty('a'); // false
obj.hasOwnProperty('toString'); // false // toString() 方式是继承来的
// 3. propertyIsEnumerable()
// 检测对象中的某一个属性是自有的，且可枚举才返回 true
// 通常直接创建的属性都是可枚举的，但某些内置属性是不可枚举的
// 比如说，创建一个对象 {}，它会有一个原型对象 Object，也就是内置的构造函数 Object() {} 中 prototype 的值，会有一个 toString 属性
'toString' in {}; // true // toString 不是 {} 的自有属性，但原型对象上有这个属性
Object.prototype.hasOwnProperty('toString'); // true
Object.prototype.propertyIsEnumerable('toString'); // false // 原型对象上有这个属性，但它是不可枚举的

// 遍历属性
var obj = {
  name: 'dog',
  age: 7
};
// 1. for ... in
// 遍历所有可枚举属性，包括自有属性和继承来的属性
for (key in obj) {
  if (obj.hasOwnProperty(key)) { // 避免遍历到继承来的属性
    console.log(key);
    console.log(obj[key]);
  }
}
// 2. Object.keys()
// 遍历所有可枚举属性，不包括继承来的属性，返回一个数组
var keys = Object.keys(obj);
console.log(keys); // [ "name", "age" ]
var values = keys.map(key => obj[key]);
console.log(values); // [ "dog", 7 ]
// 3. Object.getOwnPropertyNames()
// 遍历所有自有属性，无论是否可枚举，返回一个数组
var keys = Object.getOwnPropertyNames(obj);
console.log(keys); // [ "name", "age" ]
```

```js
// array
// 数组是一种特殊的对象，表示带 index 的有序集合。可以理解为数组是 index: value 形式的一种对象

// 创建数组
var arr = []; // []
var arr = [1, 2, 3]; // [ 1, 2, 3 ]
var arr = [ , , ]; // ??? // [ empty, empty ] // 省略的元素此时并不存在，但会产生空位
arr[0] = undefined; // 读取空位的数组元素，会得到 undefined
arr.length = 2;
var arr = [1, 2, 3, ]; // ??? // [ 1, 2, 3 ] // 数组允许有可选的逗号结尾
// 类似地，对象每一对 key: value 包括最后一对可以都加上 , 结尾，ESLint 中此规则叫作 comma-dangle
var arr = new Array(); // []
var arr = new Array(10); // 创建一个数组，长度为 10，共有十个空位
var arr = new Array(5, 4, 3, 2, 1); // [ 5, 4, 3, 2, 1 ]

// 数组元素的读写
var arr = [1, 'a', true];
arr[0]; // 1
arr[2] = false; // [ 1, 'a', false ]
arr['name'] = 'yell'; // [ 1, 'a', false, name: 'yell' ]
// 索引与普通对象属性名的区别在于，可以为数组创建任意名字的属性，但只有非负整数（0 ~ 2^32 - 2）的属性名才是索引
arr.length = 3; // 为数组添加不是索引的属性时，不会影响数组的长度
arr[100]; // undefined // 数组没有越界的概念，访问不存在的元素时，会得到 undefined

// length 属性
// length 属性，返回数组的成员数量，等于键名最大整数 + 1
// 可以手动改变这个值
var arr = [1, 2, 3];
arr.length; // 3
arr[9] = 10;
arr.length = 10; // 9 + 1 = 10，此时 arr[2] 与 arr[9] 之间的元素都为 empty
var arr = [1, 2, 3, 4, 5];
arr.length = 3;
console.log(arr); // [ 1, 2, 3 ]

// delete
var arr = [1, 2, 3];
delete arr[2];
console.log(arr); // [1, 2, empty] // 删除数组中的元素时，会产生空位
arr.length; // 3 // 但数组长度不变

// in operator
// 利用索引检测某个元素是否存在
var arr = [1, 2, 3];
0 in arr; // true
3 in arr; // false
arr[100] = 'a';
100 in arr; // true
99 in arr; // false // 空位也会返回 false

// 遍历数组
var arr = [1, 2, 3];
arr.yell = 'hello';
// 1. for ... in 会遍历到非数字索引的属性，不推荐
for (i in arr) {
  console.log(arr[i]);
}
// 依次输出 1, 2, 3, 'hello'
// 2. for ...
for (var i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
// 依次输出 1, 2, 3
// 3. forEach()
arr.forEach(item => console.log(item));
// 依次输出 1, 2, 3

// empty 和 undefined 的不同
var a1 = new Array(3); // [ empty x 3 ]
var a2 = [undefined, undefined, undefined];
// 直接读取元素时，都得到 undefined
a1[0]; // undefined
a2[0]; // undefined
// 但遍历时
a1.forEach(a => console.log(a)); // 什么都不会输出
a2.forEach(a => console.log(a)); // 依次输出 undefined
for (i in a1) {
  console.log(a1[i]); // 什么都不会输出
}
for (i in a2) {
  console.log(a2[i]); // 依次输出 undefined
}
Object.keys(a1); // []
Object.keys(a2); // [ '0', '1', '2' ]

// 类似数组的对象
// 键名为 0 或 正整数，拥有 length 属性的对象，但不具有数组方法
// 1. arguments 对象
function add() {
  return arguments;
}
var args = add(1, 2, 3);
args[0]; // 1
args.length = 3;
args instanceof Array; // false
// 2. DOM 元素集或 NodeList
// 3. 字符串
// 转化为真正的数组
var argArr = Array.prototype.slice.call(args);
argArr instanceof Array; // true
var argArr2 = Array.from(args); // (es6)
argArr2 instanceof Array; // true
var argArr3 = [...args]; // (es6)
argArr3 instanceof Array; // true

// 数组方法
// 静态方法 Array.isArray()
var arr = [1, 2, 3];
typeof arr; // object
Array.isArray(arr); // true
// 实例方法
// valueOf(), toString() // 所有对象都有的方法
var arr = [1, 2, 3];
arr.valueOf(); // [1, 2, 3] // 返回数组本身
arr.toString(); // '1, 2, 3' // 返回数组的字符串形式
// 改变原数组
// push(), pop(), shift(), unshift(), reverse(), splice(), sort()
// 不改变原数组
// join(), concat(), slice(), map(), forEach(), filter(), some(), every(), reduce(), reduceRight(), indexOf(), lastIndexOf()
```

```js
// function
// 函数也是一种特殊的对象。函数是具有与它相关联的可执行代码的对象，通过调用函数来运行可执行代码，返回运算结果。
```

  > 参考资料
  + Javascript 权威指南第三版
  + [Javascript 标准参考教程](http://javascript.ruanyifeng.com/#grammar)
  + [ES6 for everyone](https://es6.io)
  + [ECMAScript 6 入门](http://es6.ruanyifeng.com/#README)

### Global object

```js
// 全局对象的属性是全局定义的符号，Javascript 程序可以直接使用，当 Javascript 解释器启动时（或浏览器加载新页面时），它将创建一个新的全局对象，并给它一组定义的初始属性：
// 全局属性：如 undefined, Infinity, NaN
// 全局函数：如 isNaN(), parseInt(), eval()
// 构造函数：如 Date(), RegExp(), String(), Object(), Array()
// 全局对象：如 Math, JSON

// 在代码最顶级，可以使用 this 来引用全局对象
// var global = this;
// 在客户端 Javascript 中（浏览器），Window 对象充当了全局对象，Window 对象定义了一些额外的全局属性，如：
// setTimeout(), setInterval(), location, history, navigator...
// 在服务器端 Javascript 中（node），全局对象是 global 和 process
```

### Constructor


