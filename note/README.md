# Note

```js
// 原始类型有哪几种？null 是对象吗？
```

```js
// 原始类型有 6 种
// number, string, boolean, null, undefined, symbol
// 原始类型存储的都是值，没有函数调用
// '1'.toString() 可以调用，因为这里自动转换成了对象类型
// number 类型都是浮点数，比如 0.1 + 0.2 !== 0.3
// string 类型是不可变的，比如
let str = 'hello';
str[0] = 'H';
console.log(str); // 'hello'

// null 会被 typeof 判断为对象，但不是对象类型。历史原因
console.log(typeof null); // object
```

```js
// 对象类型和原始类型的不同之处？函数参数是对象会发生什么问题？
```

```js
// 除了原始类型都是对象类型
// 原始类型存储的是值，对象类型存储的是地址（对地址的引用）
const a = [];
const b = a;
a.push(1);
console.log(a, b); // [1] [1]
// 为什么 const 声明的变量改变了？
// 引用类型中保存的是到地址的指针，可以改变内部的属性，但直接把 a 赋值给其它的值是不允许的
// 如何修正？
// 可以使用 spread operator
const c = [];
const d = [...c];
c.push(1);
console.log(c, d); // [1] []

// 函数参数是对象时
const person = {
  name: 'jarvis',
  age: 20
}
function test(person) {
  person.age = 30; // 修改了 person 对象的属性值，person 变量改变
  person = { // person 指向了一个新的对象地址，并返回它
    name: 'wes', // 写成这样可能比较好理解，直接返回这个对象
    age: 20 // return {
  };        //    name: 'wes',
            //    age: 20
            // }
  return person;
}
const person2 = test(person);
console.log(person, person2); // { name: 'jarvis', age: 30 } { name: 'wes', age: 20 }
```

```js
// typeof 是否能正确判断类型？instanceof 能正确判断对象的原理是什么？
```

```js
// 对于原始类型，typeof 除了 null 都可以正确判断
// 对于对象类型，数组会返回 object，函数会返回 function

// instanceof 通过原型链来判断对象的类型
const str = 'hello';
console.log(str instanceof String); // false
const str2 = new String('hello');
console.log(str2 instanceof String); // true
// instanceof 的行为可以被更改
class MyArray {
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}
console.log([] instanceof MyArray); // true
// 以上代码相当于
Array.isArray([]); // true
```

```js
// 类型转换
```

```js
// 转布尔值
// 除了 undefined, null, false, NaN, '', 0, -0 都转为 true
// 转字符串
// Symbol 不能转为字符串
// 对象会转为 '[object Object]'
// 其它类型会根据值直接转为字符串
// 转数字
// Symbol 不能转为数字
// 字符串会直接转为数字或 NaN
// null 会转为 0
// undefined 会转为 NaN
// 空数组会转为 0，存在一个元素且是数字会转为数字，其它情况转为 NaN
// 除了数组的引用类型会转为 NaN

// 对象转为原始类型
// 对象会调用内置的函数转换为原始类型
// 优先级： [[ToPrimitive]] 函数 > valueOf() > toString()
// 如果函数的返回值是原始类型，就会直接返回那个值
let a = {
  valueOf() {
    return 0
  },
  toString() {
    return '1'
  },
  [Symbol.toPrimitive]() {
    return 2
  }
}
a + 1; // 3

// 四则运算符
// 加法运算符比较特殊
// 除了转换为数字，还可能转换为字符串（因为字符串也可以使用 + 运算符）
// 除了加法运算符之外的运算符，只会转换为数字
'a' + + 'b'; // ???
// 'aNaN'
// 优先执行 + 'b' 等于 NaN，'a' + NaN 就等于 'aNaN'
// + 运算符会把后面的值转化为数字，不能转化为的数字就得到 NaN，比如 + '100' = 100

// 比较运算符
// 如果是对象，通过 toPrimitive 转换对象
// 如果是字符串，通过 unicode 字符索引来比较
```

```js
// 如何正确判断 this？箭头函数的 this 是什么？
```

```js
function foo() {
  console.log(this.a);
}
var a = 1;
foo();

const obj = {
  a: 2,
  foo: foo
};
obj.foo();

const c = new foo();
// 直接调用 foo()，this 永远都是 window
// 对于 obj.foo()，谁调用了函数，谁就是 this，这里即为 obj
// 对于 new 的方式，this 被绑定在了创建的实例上，也就是 c

// 对于箭头函数
// 箭头函数本身没有 this，只取决于它的词法作用域，也就是箭头函数存在的 context
const foo = () => {
  console.log(this); // 直接创建的箭头函数存在于顶级作用域中，this 也就是 window
}
function a() {
  return () => {
    return () => {
      console.log(this); // 这里的 this 只取决于包裹箭头函数的第一个普通函数
    }                    // 也就是 a 函数的 this
  }
}
console.log(a()()()); // window

// bind
// 箭头函数无法使用 bind 方法
const obj = {};
function func() { console.log(this); } // 这里的 this 等于 window
func.bind(obj)(); // 这里的 this 被绑定到了 obj，也就是 {}
func.bind().bind(obj)(); // 只取决于第一次绑定，没有参数传入，等同于 window
                         // 第二次绑定无效
// 等同于
function func2() {
  return function () {
    return func.apply(); // this 被绑定到了 window
  }.apply(obj)
}
func2(); // window

// 如何使用普通函数和箭头函数
const btn = document.querySelector('.button');

// 1.
btn.addEventListener('click', function() {
  console.log(this); // btn，普通函数会根据使用方法重新绑定 this，这里的 this 被绑定到了调用它的 btn 上
});
// 2.
btn.addEventListener('click', () => {
  console.log(this); // window，因为箭头函数不会重新绑定 this，只取决于它存在的作用域，这里也就是顶级作用域
});
// 3.
btn.addEventListener('click', function() {
  console.log(this); // btn
  setTimeout(function() {
    console.log(this); // window，setTimeout 函数是直接调用的，并没有绑定到父级作用域的 btn
  }, 1000);
});
// 4.
btn.addEventListener('click', function() {
  console.log(this); // btn
  setTimeout(() => {
    console.log(this); // btn，这里的父级作用域中 this 是 btn
  }, 1000);
});
```

```js
// == 和 === 有什么区别？
```
