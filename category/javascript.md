# JS基础

## ECMAScript

### 谈谈对ECMAScript6的认识？

ECMA：

 1996年11月，JavaScript的创造者Netscape公司，决定将JavaScript提交给国际标准化组织ECMA，
 希望这种语言能够成为国际标准。
 次年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准
 ，并将这种语言称为ECMAScript。这个版本就是ECMAScript 1.0版。

又名ECMAScript2015，于2015年6月份发布
是继ECMAScript5（2009年发布）后的新一代标准

增加了很多特性，例如Maps,Sets,Promise,Generators等
let,const等声明
箭头函数等用法
Class等语法糖
而且等同于默认使用了严格模式

像TypeScript也实现了ECMAScript6标准，它是JavaScript的超集

### ECMAScript与javascript的区别？

ECMAScript是标准
javascript是实现

ECMAScript定义javascript语言的实现
但是浏览器端的javascript是一般意义上的泛指，同时还包括bom（如navigator对象）和dom等

### ECMAScript6 怎么写class么，为什么会出现class这种东西?

```js
class XXX {
    constructor() {
    }
    
    foo1() {
    }
    
    static foo2() {
    }
}
```

本质仍然是原型链直接的继承。

虽然说它的本质只是一个语法糖（并不是全新的东西），可以让有面向对象思想的人更快速上手。但是从一些细节上看，和普通的原型链继承是有区别的。
譬如，当继承`Date`这种无法被继承的变量时，`ES6`可以，而`ES5`继承法，普通无法实现

### ECMAScript的单体内置对象？

1. Global
在浏览器中变为window的一部分（注意，window不止有global对象，除了这个还有一些其它的内容）

2. Math

定义了两个不依赖于宿主环境的对象。其中

math定义了一些数学公式的使用

global在浏览器中是window形式表现，作为兜底对象，
譬如undefined，Date，Boolean等都属于global。也就是：

**所有在全局作用域中定义的属性和函数，都是global对象的属性**

一些注意：

- 给eval重新赋值会报错

- ECMAScript5中明确禁止给undefined，NaN，Infinity赋值（即使在非严格模式）

## 基础概念

### 立即执行函数，不暴露私有成员

```js
var module1 = (function() {
    var count = 1;

    function change() {
        count++;
    }

    return {
        change: change,
    };
)();
```

上面是一个立即执行函数，而且，一旦外部引用了change，会导致count无法被释放，形成闭包。
（正常没有被引用，函数执行完后会被销毁）

### JSON的了解？

JSON(JavaScript Object Notation)是一种轻量级的数据交换方式

它是基于JavaScript的一个子集。
数据格式简单，易于读写，占用带宽小

例如（注意，必须要引号）

```js
{"name": "zhangsan", "age": "18"}
```

JSON字符串转JSON对象（后者是JS中内置的对象模型）

```js
eval('(' + str + ')')
str.parseJSON
JSON.parse(str)

JSON转字符串
obj.toJSONString()
JSON.stringify(obj)
```

### 什么是闭包(closure)，为什么要用它？

**闭包是指有权访问另一个函数作用域中的变量的函数**（JavaScript高程也这样定义）

* 创建闭包的最常用方式：在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量
* 利用闭包可以突破作用域链，将函数内部的变量和方法传递到外部
* 不过也经常会容易造成内存泄漏问题（无法自动回收）

特性：

1. 函数内再嵌套函数
2. 内部函数引用外层的参数和变量
3. 参数和变量不会被垃圾回收机制回收

```js
// demo
function sayHello() {
    // 函数内部变量
    var word = 'hello,world!';
    var index = 0;

    return function() {
        console.log(word + (index++));
    };
}

var say = sayHello();

say(); // hello,world!0
say(); // hello,world!1
```

上面的通俗点讲，那个被return的匿名函数就是一个闭包（因为它有访问另一个函数作用域（sayHello函数）中的变量的能力）。
而且可以看到，如果匿名函数没有引用外部函数作用域的变量，正常情况下外部函数执行完后相关内存就销毁了，但是由于它引用了，
并且被外界持有了引用`say`，所以形成了闭包，无法回收内存。

## 基础语法

### for-of

for-of里可以break，但是不能return;

### javascript中的`"user strict";`是什么意思？使用它区别是什么？

`use strict`是ECMAscript 5中的严格运行模式

如严格模式是ECMAScript 5 中的一种模式，ECMAScript 5中有两种模式，一种是正常模式，一种是“严格模式(strict mode)”，
使用这种模式使得Javascript在更严格的条件下运行。

设立“严格模式”的目的主要有:

1.  消除JS语法的不合理，不严谨之处，减少一些怪异行为
譬如不能用with,不能给未声明的全局变量赋值，不能callee，不允许直接操作argument等

2.  消除代码运行的不安全之处，保证代码运行的安全

3.  提高编译器效率，增加运行速度

4.  为新版本做铺垫，如ES6中全面使用了严格模式

注意，在正常模式下可以运行的代码很有可能严格模式下运行出错。而且ES6中只允许严格模式的用法。

严格模式有两种用法:

1. 在脚本文件(或`<script>`脚本片段)的第一行，使用`'use strict';` 可以将整个脚本文件(片段)以严格模式运行
(注意，如果前面是一些不产生实际运行结果的语句，可以不再第一行-如在开头注释后面,
注意,前面有;号也会取消严格模式；但是如果前面的语句有效-如输出语句，这样则严格模式无效，整个片段会以正常模式运行)

2. 在函数体的第一行使用`'use strict';`则整个函数以严格模式运行
(这种是最常用的写法，通常会将这句话放在一个立即执行的匿名函数中)

使用严格模式后，JS语法和行为与正常模式有所区别:

1. 全局变量显示声明(正常模式下，如果一个变量没有声明就赋值，默认是全局变量，而在严格模式下，会报错)

2. 静止使用with语句(with语句主要用于设置代码在特定对象中的作用域，严格模式下禁用with语句)

3. 创设eval作用域，ES5中，正常模式下，JS语言中有两种变量作用域:全局作用域和函数作用域。
严格模式创设了第三种作用域:eval作用域(这样,eval里面不能再生成全局变量了-正常模式中eval会生影响外部作用域)。

4. 禁止this关键字指向全局对象。在正常情况下,一个普通函数内部的this会指向一个全局对象window,但是严格模式下禁止了这种用法，
严格模式下,普通函数内部的this为undefind，注意：通过new 出来的对象除外，new 出来的会指向自身

5. 禁止在函数内部遍历调用栈。正常情况下函数内部可以通过caller等方法调用自身,但是严格模式下禁止了这种用法

6. 禁止删除变量（严格模式下无法删除var显示声明的变量，只能删除属性）
  注意,` [object Object]`的属性只有configurable设置为true才能被删除（不过默认隐式创建的一般都是为true），否则无法删除

7. 显式报错。
  正常模式下，对于一个对象的只读属性进行赋值，不会报错，只会默默失败，严格模式下，会报错
  严格模式下，对于一个使用getter方法读取的属性进行赋值，会报错
  严格模式下，对禁止拓展的对象添加新属性（Object.preventExtensions(o)），会报错
  严格模式下，删除一个不可删除的属性，或报错（如删除Object.prototype）
  严格模式下，删除一个不可删除的属性，或报错
  对象不能有重名的属性(正常模式下，如果对象有多个重名属性，最后赋值的那个属性会覆盖前面的值，严格模式下，这属于语法错误)
  函数不能有重名参数(正常模式下，如果函数有多个重名参数，可以用argument[i]读取。严格模式下，属于语法错误)
8. 禁止八进制表示法
  严格模式下，整数的第一位如果是0，表示这是八进制，比如0100等于十进制的64。但是严格模式下禁止这种写法，证书第一位为0，会报错
9. Arguments对象的限制。
不允许对arguments赋值
Arguments不再追踪参数的变化（如果形参a被改变，对应的arguments是不会改变的）
禁止使用arguments.callee。严格模式下，无法使用caller,也就是说匿名函数内部无法调用自身了

10. 函数必须声明在顶层
严格模式下只允许在全局作用域和函数作用域的顶层声明函数。
不允许在非函数的代码块内声明函数(ES6中会加入块级作用域概念)

11. 保留字。严格模式下，新增了一些保留字:
* implements, interface, let, package, private, protected, public, static, yield
    使用这些词作为变量或参数将会报错
* 另外,ES5本身也有些保留字:
    class, enum, export, extends, import, super
    以及各大浏览器自行增加的 const保留字。这些保留字都不能作为变量名或参数

### js对象传参是引用传递还是值传递？

JavaScript高级程序设计中，明确指出，是值传递，而不是引用传递

对象传承时，传递的是在栈内存中的值的地址，所以就是函数中对象改变，外部也会改变，因为这个地址指向堆内存中的实际对象

如何区分是引用还是值传递？

```js
function setName(obj) {
    obj.name = 'hello';
    obj = new Object();
    obj.name = 'world';
}

var word = new Object();

setName(word);
alert( word.name); // hello
```

如果是引用传递，那么obj和word是同一个引用，obj改变时，word的引用地址应该自动变化，所以word.name应该是'world'

但是实际上，word.name的值是'hello'，所以可知是值传递而不是引用传递。

实际上，传递的是一个复制后的值，虽然这个值和word一样，指向相同的堆内存，但它和word确实不是同一个东西，所以改变时互不影响

可以把函数中的参数看成局部变量

### var和let作用域？

```js
for (let i = 0; i < 5; i++) {
 setTimeout(function() {
  console.log(i);
 }, 1000);
}
console.log(i); // 报错 01234
```

这里的的let是在for循环中声明，
所以当前的i只在本轮循环有效，每一次的循环就是一个新的变量
引擎内部会记住上一次循环的值，初始化本轮循环i时，在上一轮的基础上计算
for循环中，循环语句是一个父作用域，循环体内是一个单独的子作用域
内部原理：
for ( LexicalDeclaration Expressionopt ; Expressionopt ) Statement 规则的时候
每次迭代会新建运行环境记录值为拷贝最后迭代内容
（每次循环体都是个独立的新scope）
所以当次循环体里的定义的func往外爬变量就是当次循环体内的值了

```js
let i;
for (i = 0; i < 5; i++) {
 setTimeout(function() {
  console.log(i);
 }, 1000);
}
console.log(i); // 5 55555

for (var i = 0; i < 5; i++) {
 setTimeout(function() {
  console.log(i);
 }, 1000);
}
console.log(i); // 5 55555
```

### js的浮点误差？

```js
var a=10.2;
var b= 10.1;

console.log(a - b === 0.1); // false
console.log(a - 10.1 === 0.1); // false,实际是0.09999999999999964
console.log(a - 0.1 === 10.1); // true
```

一般比较方法是判断两个浮点数的误差不大于某个极小数即可，如
`a - b < 1e-7`

或者.toFixed(10)
在判断浮点运算结果前对计算结果进行精度缩小，因为在精度缩小的过程总会自动四舍五入: 
这样

```js
parseFloat((1.0-0.9).toFixed(10)) === 0.1 // true
parseFloat((1.0-0.8).toFixed(10)) === 0.2 // true

(a - 10.1).toFixed(10) // 0.1000000000（自动四舍五入了）
```

不光是js，只要采用IEEE754浮点数标准(由电气电子工程师学会定义的浮点数在内存中的算法规范。)的语言都存在这个问题。
（由美国电气电子工程师学会（IEEE）计算机学会旗下的微处理器标准委员会（Microprocessor Standards Committee, MSC）发布）
IEEE754浮点数主要有单精度（32位）和双精度（64位），js采用双精度。
有些浮点数比如0.1转化为二进制是无穷的，而64位的浮点数表示法尾数位只允许52位，
超出的部分进一舍零，会造成浮点数精度丢失，两个浮点数转化二进制相加后的结果，也遵循这个原则。

譬如：
    十进制           二进制
    0.1              0.0001 1001 1001 1001 ...
    0.2              0.0011 0011 0011 0011 ...
    0.3              0.0100 1100 1100 1100 ...
    0.4              0.0110 0110 0110 0110 ...
    0.5              0.1
    0.6              0.1001 1001 1001 1001 ...

所以比如 1.1，其程序实际上无法真正的表示 ‘1.1'，而只能做到一定程度上的准确，这是无法避免的精度丢失：`1.09999999999999999`

### JavaScript中的作用域与变量声明提升?

ES6之前没有块级作用域，var等声明会提前

例如

```js
console.log(a); // undefined
console.log(b); // ReferenceError
var a = 1;
let b = 2;
```

等同于：

```js
var a;
console.log(a); // undefined
console.log(b); // ReferenceError
a = 1;
let b = 2;
```

另外

```js
function xxx() {}
```

函数声明也会提升，顺序是：

- 先函数声明

- 如果没有声明，则进行变量声明，如果有，变量声明无效

- 变量赋值

```js
var myName;
function myName () {...};
console.log(typeof myName); // function

var myName = 'hello';
function myName () {...};
console.log(typeof myName); // String
```

### JavaScript中的label语法了解么？

注意，是JavaScript中的label不是dom中的label标签

首先看下label语法的示例：（出自JavaScript高级程序设计）

```js
var num=0;
outermost:
for (var i = 0; i < 10; i++){
    for (var j = 0;j < 10; j++){
        if (i == 5 && j == 5){
            break outermost;
        }
        num++;
    }
}
// 55
alert(num);
```

label语句可以在代码中添加标签，以便将来使用，譬如上述的for循环内部就用到label来跳转

但是，MDN上已经明确声明了禁止使用（这也是为什么几乎没看到这段代码）

```js
Avoid using labels
Labels are not very commonly used in JavaScript since they make
programs harder to read and understand. As much as possible, avoid
using labels and, depending on the cases, prefer calling functions or
throwing an error.
```

- [label - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)

### return?

```js
return {
     name: "hello"
};
return
{
     name: "hello"
};
```

前者是一个Object，后者是undefined.
这是return的设计缺陷-程序被自动补全为了（自动修复机制）

```js
return;
{
     name: "hello"
};
```

### 为什么 parseInt(0.0000008) === 8？

因为隐式转化的问题

parseInt接受的是字符串，所以

`0.0000008`被转化成：

```js
'0.0000008'
'8e-7'
```

嗯，所以因为内部用科学计数法表示了，所以是`8`

另外注意下：

```js
parseInt(0.000008); // 0
```

因为它内部还没有转为科学计数法（这个与JS内部机制有关）

## this

## 谈谈this对象的简单理解。

this总指向函数的直接调用者(而非间接调用者)
譬如 

```js
var ajax = Util.ajax();
ajax();
```

此时this指向window，而不是util

如果有new关键字，this指向new出来的那个对象

在事件中，this指向触发这个事件的对象
特殊（ie中的attachevent的this总是指向全局对象window）

- ES5非严格模式中，函数调用如果未指定，默认this指向window

- 严格模式中则为undefined

### 下述代码的区别？考察指针指向

```js
function foo() {
    console.log(this.a);
}

function active(fn) {
    // 真实调用者，为独立调用
    fn();
}

var a = 20;
var obj = {
    a: 10,
    getA: foo
};

// 20-相当于foo();
active(obj.getA);
```

```js
var a = 20;

function getA() {
    return this.a;
}
var foo = {
    a: 10,
    getA: getA
};

// 10
console.log(foo.getA());
```

### 箭头函数与bind this的区别？

```js
setTimeout(function() {
    console.log("id:", this.id);
}.bind(this),100);
   
setTimeout( () => {
    console.log("id:", this.id);
},100); 
```

这两者是有区别的：

- 箭头函数没有自己的this（所以在里面找就是相当于在外部作用域的this找）

- 普通函数中自己本来是有this的，bind this只不过是把自己的this替换成了传入this的作用域的this而已

另外，不仅仅是this

```js
箭头函数并不绑定 this，arguments，super(ES6)，抑或 new.target(ES6)。
```

## 对象创建

## Javascript创建对象的几种方式？

1. 隐式创建

```js
var obj = {};
```

2. new Object

```js
var a = {};
// b === a
// 注意，指向的对象完全相等，浅复制
var b = new Object(a);
```

3. 构造或工厂

```js
// 构造
new XXX();

// 工厂等产生
var obj = xxx();
```

4.Object.Create

```js
// 此时b.__proto__=== a.prototype
// 创建了一个新的对象，只不过[[prototype]]指向了传入的a.prototype
var b = Object.create(a.prototype);
```

### new操作符具体干了什么呢？

1.创建一个空对象，并且this变量引用该对象，同时继承该对象的原型

2.属性和方法被加入到this引用的对象中

2.新创建的对象由this引用，并且最后隐式返回this

```js
var obj = {};

obj.__proto__ = Base.prototype;
Base.call(obj);
```

#### Object.create()的作用？

`Object.create(proto[,propertiesObject])`是ES5中提出的一种新对象创建方式

- 第一个参数是：要继承的原型，可以传null

- 第二个参数是：对象的属性描述符，可选
可选属性包括：
数据属性-
    包含value(值),writable(是否可任意写),
    enumerable(是否能用for in枚举),
    configurable(是否能被删除,修改)特性(后面三个默认为false)
访问器属性-包含set/get特性


注意:当满足以下任一条件时，则会引发TypeError异常:

1. prototype参数不是对象而且不是null

2. descriptors参数中的描述符具有value或writable特性，并且具有get或set特性(value或writable与get或set不能同时存在)

3. descriptors参数中的描述符具有不为函数的get或set特性(get或set必须是函数)

```js
// 此时b.__proto__=== a.prototype
// 创建了一个新的对象，只不过[[prototype]]指向了传入的a.prototype
var b = Object.create(a.prototype);
```

关键点:可以创建一个继承某对象的对象

### new、new Object()和Object.create(proto,[propertiseObject]之间者的异同?

相同点:

- new和Object.create()都可以用来创建一个新的对象。new Object()当参数为空时也是创建一个新的对象

不同点:

- 本质不同,new 一般配合类的构造函数使用，new的时候，是先创建一个对象，然后将对象的__proto__属性指向该类的prototype。
(obj.__proto__ = Base.prototype)

- Object.create(proto…)一般第一个参数直接传入一个对象，然后创建出来新对象就直接显示指向该对象了。
(obj.__proto__ = Base)

- new Object()当传入参数为一个object时不会创建新对象，而是直接引用传递，
(obj === Base)
当参数不存在时，才创建一个新的{}(此时obj为{})

关键点:创建对象时 _proto_和prototype有区别
javascript使用__proto__指向对象的原型。
另外，需要知道__proto__([[prototype]])是隐式原型链追溯的关键

## jsapi

### JS数组的迭代与归并方法？

迭代

传入的参数是函数，函数内的传值都是：`item, index, array`

1. every

如果每一项返回true，则返回true

2. some

如果任意一项返回true，则返回true

3. filter

会返回为true的项组成的数组

4. forEach

没有返回值，请注意，函数内部是值传递

5. map

返回每次函数调用结果组成的数组（可以认为，相当于全部映射成另一个了）

归并

传入的参数是`函数，初始值`，函数内的传值都是：`prev, cur, index, array`

1. reduce

迭代数组的所有项，并返回一个结果

从第一项开始，逐个向后遍历，遍历到最后

第N项时的pre是前N-1项已迭代的结果

2. reduceRight

迭代数组的所有项，并返回一个结果

从最后一项开始，逐个向前遍历，遍历到最前

和前一个的差别只是从后开始遍历而已。

### 数组和对象有哪些原生方法，列举一下?

数组：

```js
push
pop
shift
unshift
splice
slice
reverse
sort
concat
join
toString
indexOf（可以识别obj的位置-只能判断引用，如果是引用不同而内容相同是无法判断的）
lastIndexOf
forEach
map
filter
every
some
reduce
reduceRight
length
```

Object:

```js
hasOwnProperty
isPrototypeOf
provertyIsEnumerable
toString // {}.toString()返回[object Object]
// 主要区别，一个数组中
// toString访问的是每一个对象的toString方法
// toLocalString(本地环境字符串,会根据机器环境返回字符串)访问的是对象每一个元素的toLocalString
// 两个方法都可以被重写
toLocalString // {}.toLocalString()返回[object Object]
valueOf // 返回的是原始值(对象本身的值)，例如{}.valueOf();返回的是{}对象
call
apply
```

### .call()和.apply()的区别？

这两个方法都可以替换`context`

区别是

```js
.call(context, param1, param2, ...)
.apply(context, [param1, param2, ...])
```

一个是传数组，一个是传多个参数

### indexof与findindex的区别？

indexOf是es5中的（同批次还有every、some 、forEach、filter 、indexOf、lastIndexOf、isArray、map、reduce、reduceRigh）
findIndex是es6中新增的（与find同批次）
findIndex和find都可以发现NaN，弥补了数组的IndexOf方法的不足。

例如

```js
[NaN].indexOf(NaN)  
// -1  
[NaN].findIndex(y => Object.is(NaN, y))  
// 0  
```

indexOf方法无法识别数组的NaN成员，但是findIndex方法可以借助Object.is方法做到。

另外findIndex传入的是条件函数，
（返回符合测试条件-true，的第一个数组元素索引，如果没有符合条件的则返回 -1。）
而indexOf是直接传入目标

### String()与toString()的区别

String()转换规则：

1.如果值有toString()，调用值的toString-不带参

2.如果值是null，返回'null'

3.如果值是undefined，返回'undefined'

除了null与undefined外的值都有toString()

而且toString(基数)可以接收一个基数，譬如传8代表8进制输出

但是注意：

```js
console.log(('11').toString()); // 合法输出11
console.log((true).toString()); // 合法输出true
console.log((22).toString()); // 合法输出22

console.log('11'.toString()); // 合法输出11
console.log(true.toString()); // 合法输出true
console.log(22.toString()); // 报错Uncaught SyntaxError: Invalid or unexpected token
console.log(null.toString()); // 报错Cannot read property 'toString' of null
console.log(undefined.toString()); // 报错Cannot read property 'toString' of undefined
```

### getComputedStyle

DOM2级样式中增强了`document.defaultView`，提供了getComputedStyle方法。

接收两个参数：要取得计算样式的元素，和一个伪元素字符串（如:after，可以为null）

返回一个CSSStyleDeclaration对象（与Style属性的类型相同）

譬如

```js
var computedStyle = document.defaultView.getComputedStyle(myDiv, null);

computedStyle.width; // 100px
computedStyle.color; // red
...
```

作用是用来动态计算，因为默认的style对象获取的信息不包括那些从其它样式表层叠而来并影响到当前元素的样式信息。

老版本IE不支持这个方法，它可以通过`myDiv.currentStyle`来达到类似效果

另外，计算样式是只读的。无法通过修改带来影响

另外，不同浏览器的表现可能会有差异，需要多测试，譬如有的浏览器`visibility`为`visible`，有的为`inherit`，因为任何具有默认值的css属性都会表现在计算后的样式中。

### JSON.stringify

接收三个参数：（后两个可以不传）

- 第1个参数是需要转化的对象

- 第2个参数可以是数组或者函数-过滤器

- 第3个参数表示是否在json字符串中保留缩紧

    - 如果是数字，则代表每一层级的缩进空格数
    
    - 如果是字符串，则会用这个字符串替代原有的空格缩进
    （每一层级，原有的多个空格缩进不再用，用这个字符串替代）

```js
JSON.stringify(json, ["title", "name"], 4);

JSON.stringify(json, function(key, value) {
    if (key === 'title') {
        return key + '-hello';
    }
    
    return value;
}, 4);
```

### JSON.parse

除了第一个是字符串，同样接收第2个参数

该参数是一个函数（还原函数），同样用来过滤（接收key-value）

### scrollIntoView方法知道么？

这是一个标准方法，但是处于实验中的功能。

它的定义是：

让当前的元素滚动到浏览器窗口的可视区域内。

用途：

很多时候用来解决input被输入法遮挡问题。

```js
// 等同于element.scrollIntoView(true) 
element.scrollIntoView();
// Boolean型参数 
element.scrollIntoView(alignToTop);
// Object型参数
element.scrollIntoView(scrollIntoViewOptions);
```

传参数：

- alignToTop
    
    - 如果为true，元素的顶端将和其所在滚动区的可视区域的顶端对齐。
    
    - 如果为false，元素的底端将和其所在滚动区的可视区域的底端对齐。
    
- scrollIntoViewOptions（带options的支持情况较差）

    - 一个boolean或一个带有选项的object：
    
    - 如果是一个boolean, true 相当于{block: "start"}，false 相当于{block: "end"}
    
```js
{
    behavior: "auto"  | "instant" | "smooth",
    block:    "start" | "end",
}
```

注意，取决于其它元素的布局情况，此元素可能不会完全滚动到顶端或底端。

更多见w3c。

### scrollIntoViewIfNeeded方法知道么？

首先，这个方法**不是W3C标准**，是一种WebKit专有的方法，所以尽量不要在生产环境中使用它！

这个方法很多时候都被用到解决input被输入法遮挡问题。

因为它的定义是：

用来将不在浏览器窗口的可见区域内的元素滚动到浏览器窗口的可见区域。 
如果该元素已经在浏览器窗口的可见区域内，则不会发生滚动。 
此方法是**标准**的Element.scrollIntoView()方法的专有变体。

支持传一个参数：

- opt_center（一个 Boolean 类型的值，默认为true）：

    - 如果为true，则元素将在其所在滚动区的可视区域中居中对其。
    
    - 如果为false，则元素将与其所在滚动区的可视区域最近的边缘对齐。
     根据可见区域最靠近元素的哪个边缘，元素的顶部将与可见区域的顶部边缘对准，
     或者元素的底部边缘将与可见区域的底部边缘对准。

不过，按w3c上的描述，这个私有属性往往支持性还要比标准属性scrollIntoView好点。

所以解决输入法遮挡input，可以是：

```js
$('input').on('click', function() {
    var target = this;
    
    setTimeout(function() {
        // 可以自己选择是否传参
        if (target.scrollIntoViewIfNeeded) {
            target.scrollIntoViewIfNeeded();
        } else {
            target.scrollIntoView();
        }
    }, 100);
});
```

### es6的proxy与reflect

- proxy对目标对象的属性读取、设置，亦或函数调用等操作进行拦截（处理）

它的操作包括（get、set、propKey in proxy（has）、deleteProperty、defineProperty、for（enumerate）、
construct、getOwnPropertyDescriptor、getPrototypeOf、isExtensible、ownKeys、preventExtensions、setPrototypeOf）等

使用：

```js
let proxy = new Proxy(target,handle)
```

每次target有变动都会通知handle，这里注意，handle内部必须实现若干对应的方法才能接收到拦截，
而且返回的proxy相当于是target的浅拷贝

```js
let target = { _prop: 'foo', prop: 'foo' };
let proxy = new Proxy(target, handler);

proxy._prop = 'bar';
target._attr = 'new'
console.log(target._prop) // 'bar'
console.log(proxy._attr) //'new'
```

```js
let handler = {
    get(target, key) {
        return target[key]
    },
    set(target, key, value) {
        if (key === 'age') {
            // 这样就只有age改变时才会生效
            target[key] = value > 0 && value < 100 ? value: 0
        }
        
        console.log(target[key]);
        return true; //必须有返回值
    }
};

let target = {};
let proxy = new Proxy(target, handler);
proxy.age = 22; //22
```

- Reflect与ES5的Object有点类似，包含了对象语言内部的方法

Proxy相当于去修改设置对象的属性行为，而Reflect则是获取对象的这些行为。

和proxy类似，也有若干静态方法，譬如

```js
Reflect.apply
Reflect.construct
Reflect.defineProperty
Reflect.deleteProperty
Reflect.enumerate // 废弃的
Reflect.get
Reflect.getOwnPropertyDescriptor
Reflect.getPrototypeOf
Reflect.has
Reflect.isExtensible
Reflect.ownKeys
Reflect.preventExtensions
Reflect.set
Reflect.setPrototypeOf
```

注意，`Reflect.call`是不存在的，目前只有上述几种方法（20180208）

譬如，可以这样用：

```js
Reflect.apply(fn, obj, [])
```

这个的作用等价于

```js
fn.apply(obj, []);
// 或
Function.prototype.apply.call(fn, obj, []);
```

## 事件模型

### JS复合事件

DOM3级别中的一类事件，譬如

IME: Input Method Editor

```js
compositionstart（IME文本复合系统打开时触发）
compositionupdate（在向输入字段中插入新字符时触发）
compositionend（IME文本复合系统关闭时触发，表示返回正常键盘输入状态）
```

常用于筛选输入，在现代浏览器中，都支持的不错（而且IE9居然是率先支持的）

### event.prventDefault()与event.stopPropagation()的区别？

(不考虑部分IE浏览器，
IE浏览器的`preventDefault`得用`Window.event.returnValue=false`替代。
IE下的`stopPropagation`要用`event.cancelBubble=true`替代)

`event.preventDefault()`用于取消事件的默认行为，
例如当点击提交按钮时，监听方法内部使用了这句代码可以阻止默认的表单提交行为。
同理可以适用于阻止a标签的跳转行为等等

`event.stopPropagation()`用于取消事件的传递，
即事件冒泡或事件捕获时阻止事件的冒泡或者阻止被下一级捕获。

比如div中点击a标签。事件冒泡中,在a标签的监听函数内部使用这句代码可以阻止事件冒泡到div上，所以div无法获取到点击事件。
在事件捕获中，在div的监听函数内部使用这句代码可以阻住a标签捕获事件，所以a标签无法捕获到监听事件。
(avveventListener的第三个参数为false代表使用冒泡机制，为true代表使用捕获机制，默认为false)

### 给一个dom同时绑定两个点击事件，一个用捕获，一个用冒泡。会执行几次事件，会先执行冒泡还是捕获？

```js
addEventListener(name, func, useCapture);
```

同时，第二个参数可以传入一个对象（会自动调用对象的handleEvent方法）

```js
document.body.addEventListener('click',
    {
        handleEvent: function() {
            alert('body clicked');
        }
    }, false);
```

第三个参数是是否冒泡

冒泡意味着从下到上
捕获则相反，从上到下

无论是冒泡事件还是捕获事件，元素都会先执行捕获阶段
从上往下，如有捕获事件，则执行；
一直向下到目标元素后，从目标元素开始向上执行冒泡元素，
即第三个参数为true表示捕获阶段调用事件处理程序，如果是false则是冒泡阶段调用事件处理程序。
(在向上执行过程中，已经执行过的捕获事件不再执行，只执行冒泡事件。)

所以同时监听捕获和冒泡时的顺序：
父级捕获->子级捕获->子级冒泡->父级冒泡

`e.stopPropagation();`可阻止冒泡或捕获的传播

### 事件是？IE与火狐的事件机制有什么区别？ 如何阻止冒泡？

譬如在网页点击一个按钮时会产生一个事件，做xxx操作时也可能会产生xxx事件(有的操作对应多个事件)，这种事件可以被js监听到

一般有两种事件模型：捕获型和冒泡型

ie中支持冒泡型，火狐中两种都支持（默认为支持事件捕获）

阻止冒泡：event.stopPropagation();(符合W3C标准)
(旧版IE用event.cancelBubble = true)-IE8及以下，但其实chrome和firefox中也支持（只是考虑到非标准，后续迟早要移除）

IE浏览器中:事件从里向外发生，事件从最精确对象(target)开始触发，然后逐步向上级，最终到最不精确的对象(document)触发，即事件冒泡
Netscape：事件从外向里发生，事件从最不精确的最新(document)开始触发，然后到最精确对象(target)触发，即事件捕获

W3C标准将两者进行中和，在任何的w3c的事件模型中，事件先进入捕获阶段，再进入冒泡阶段。
在w3c dom浏览器中，绑定事件为 addeventListener(type,fn,useCapture)。
其中useCapture:布尔值(true或false)，true代表采用事件捕获机制，false代表采用事件冒泡机制，默认为false(一般为了兼容各种浏览器也会设为false)

注意:在ie678中，不支持事件捕获，所以没有addeventListener()方法，IE提供了另一个函数attachEvent(type,fn)。没有第三个参数(移除用的detachEvent)

非IE中阻止事件传播(event.stopPropagation())
IE中阻止事件传播(event.cancelBubble=true;)

### ["1", "2", "3"].map(parseInt) 答案是多少？

```js
parseInt(val, radix);
```

radix的参数范围是[2,36]

map传了三个参数(element, index, array)

所以分别是`10机制的1-传0相当于默认值`，`进制非法，radix超出范围`，`2进制的3，不合法的解析`

结果: [1, NAN, NAN]

### 移动端的点击事件的有延迟(click的300毫秒延迟)，时间是多久，为什么会有？ 怎么解决这个延时？

click 有 300ms 延迟,为了实现safari的双击事件的设计，浏览器要知道你是不是要双击操作。

一般采用touch方式模拟点击可以去除延迟
或者直接采用fastclick等第三方库

移动端浏览器的默认显示宽度是980px(不同机型各异，但相差不大)，而不是屏幕的宽度(320px或其他)。
为了对早期普通网页更好的体验，iphone设计了双击放大显示的功能--这就是300ms延迟的来源：
如果用户一次点击后300ms内没有其他操作，则认为是个单击行为；否则为双击放大行为。

解决：

1.user-scalable=no。 
不能缩放就不会有双击缩放操作，因此click事件也就没了300ms延迟，这个是Chrome首先在Android中提出的。
2.设置显示宽度：width=device-width。
Chrome 开发团队不久前宣布，在 Chrome 32 这一版中，
他们将在包含 width=device-width 或者置为比 viewport 值更小的页面上禁用双击缩放。
当然，没有双击缩放就没有 300 毫秒点击延迟。
3.直接采用fastclick等第三方库
简而言之，FastClick 在检测到 touchend事件的时候，
会通过 DOM 自定义事件立即触发一个模拟click事件，并把浏览器在 300 毫秒之后真正触发的 click事件阻止掉。


事件执行顺序：
touchstart->-touchmove（如果有的话）>touchend
->mousedown->mousemove（如果有的话）->mouseup
->click->dblckick（如果有的话，IOS上不支持dblclick事件，Android支持dblclick事件）

### 什么是点透行为?

假设有两个层级，A和B；A在上面，B在下面。
如果A监听touch事件(zepto的tap事件)，而且B上有个链接(或者监听click事件)，
那么当touch A后，先后触发了touchStart和touchEnd事件，touchEnd后A层隐藏，
而此刻会触发在document最前面B的click事件；这就是点透行为。

这是因为在移动端浏览器，事件执行的顺序是touchstart > touchend > click。
而click事件有300ms的延迟，当touchstart事件把B元素隐藏之后，隔了300ms，
浏览器触发了click事件，但是此时B元素不见了，所以该事件被派发到了A元素身上。（除非主动组织事件传递）
如果A元素是一个链接，那此时页面就会意外地跳转。

如上述解决了300ms延迟的方案中，自然也会结局点透。。。
如`user-scalable=no`这个方案就避免了点透（因为避免了300ms延迟）

### 如何实现Tap事件？

利用touch（前提也必需设置<meta>禁止页面缩放才能避免点透）

基于touchstart、touchmove、touchend这三个事件

start记录触发的startX和startY（pageX,pagey）
mouve中记录最后的endX和endY
end中进行判断，是否合法（譬如endX-startX,endY-startY 不能大于25）
并且endTime-startTime <150(防止长按等其它事件)
如果符合要求，就触发tap事件，通过如下触发

```js
var event=new CustomEvent('tap',{
    bubbles: true,
    cancelable: true
});
    
// 触发btn上的tap事件
btn.dispatchEvent(event); 

// 同时，trigger时设置30ms的延迟
setTimeout(function(){
    trigger(target, 'tap');
}, 30);
```

如果要在pc上也兼容可以再通过mousedown、mouseup、mousemove来处理，原理一样，只是做个pc与移动判断

其中给setTimeout()设置了30毫秒的延时，实际上手机浏览器计时并不准确，延时定短了tap有可能就在click前面执行了。
虽然松开手指时touchend和click会一前一后触发，但之间的间隔并不是每次都一样，
少的时候只有几毫米，多的时候有二三十毫秒，因此tap需要延时在30毫秒之后，
保证它在click之后执行。
（因为要确保tap不影响原有的）

### onclick和addEventListener('click')的区别？

onclick属于DOM0级的事件处理，
譬如如果使用HTML指定事件处理程序，那么onclick属性就是一个包含着同名HTML特性中指定的代码的函数，
（每一个元素都有自己的事件处理程序属性-包括window和document）

addEventListener是DOM2级的事件处理，事件流中的监听（冒泡或捕获），而且可以阻止继续冒泡或捕获的传递

### 事件中的currentTarget与target

currentTarget指向绑定事件的对象

target指向触发事件的对象

譬如绑定到document.body中

currentTarget一直都是document.body，而target可以是里面任意一个触发事件的元素（譬如只不过是最后冒泡出来而已）

另外，一旦事件执行完毕，event就被销毁了

### 事件中的stopImmediatePropagation与stopPropagation

stopImmediatePropagation方法作用在当前节点以及事件链上的所有后续节点上，
目的是在执行完当前事件处理程序之后，停止当前节点以及所有后续节点的事件处理程序的运行

stopPropagation方法作用在后续节点上，
目的在执行完绑定到当前元素上的所有事件处理程序之后，停止执行所有后续节点的事件处理程序

### addEventlistener的传入参数

```js
dom.addEventlistener('click', func, options, useCapture, wantsUntrusted);

options包括：
capture:  Boolean（表示 listener 会在该类型的事件捕获阶段传播到该 EventTarget 时触发）
once:  Boolean（表示 listener 在添加之后最多只调用一次）
passive: Boolean（表示 listener 永远不会调用 preventDefault()）

useCapture：
是否使用捕获事件，true为捕获，false为冒泡，默认冒泡

wantsUntrusted：
如果为 true , 则事件处理程序会接收网页自定义的事件（此参数只适用于 Gecko）
```

### 是否所有的dom事件都会冒泡

并不是，譬如focus之类的就不会

判断：每个 event 都有一个event.bubbles属性，可以知道它可否冒泡（W3C定义）

## 原型与原型链

### JavaScript原型，原型链 ? 有什么特点？

这里随便简单描述点。

每一个对象都有原型链`__proto__`对象（由浏览器决定）可以顺着原型链往上找

函数对象有一个`prototype`，`(new Func()).xxx`即可调用`prototype.xxx`（如果对象没有，才会顺着原型链找）

`prototype`中的方法属于`xxx.prototype`，不属于实例对象，这点得区分

原型和原型链常被用于模拟其它面向对象语言的继承语法

```js
instance.constructor.prototype = instance.__proto__
```

我们找一个属性时，会先看对象中是否有，如果没有，沿着原型链判断是否有，一直到检索Object的内置对象

### JS中有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

- `hasOwnProperty`

js中hasOwnProperty函数是返回一个布尔值，
指出一个对象是否具有指定名称的属性
此方法无法检查该对象的原型链中是否具有该属性
该属性必须是该对象本身的成员，不能是原型链上的

使用：

```js
Object.hasOwnProperty.call(object, proName);
```

object必须是对象，proName必须是属性名称的字符串形式

有则返回true,否则false

## BOM

### location.assign知道么？

它的作用是打开新的URL并在浏览器的历史记录中生成一条记录。

以下效果等同：

```js
window.location = 'http://www.google.com';
location.href = 'http://www.google.com';
location.assign('http://www.google.com');
```

`location.href`底层就是调用的`location.assign`

### 如何跳转到新的页面并不产生历史记录？

使用:（可以调整所有）

```js
location.replace('http://www.google.com');
```

或者：（同源限制）

```js
// 改变历史记录，但并不会主动去访问
// 如果调整跨域地址，会报错，history api不会允许
1. history.replaceState(null, document.title, 'http://192.168.x.x/xxx/xx.html');

2. location.replace('');
```

### location.reload?

作用是重载当前显示的页面，可接受一个参数

```js
// 重新加载（有可能从缓存中加载）
location.reload();

// 重新加载（从服务器重新加载）
location.reload(true);
```

## dom操作

### documen.write和 innerHTML的区别

document.write只能重绘整个页面

innerHtml可以重绘页面的一部分

加入页面已经加载完毕（HTML解析完毕），再调研documen.write的话，会将整个页面重置

### innerHtml设置脚本会有什么效果？

除了IE8及更早版本，现代浏览器中通过innerHTML插入的脚本元素并不会执行。（dom对象已经插入了，但是不会执行这个脚本）

```html
1. div.innerHTML = "<script defer> alert('hello!');<\/script>";

2. div.innerHTML = "_<script defer> alert('hello!');<\/script>";

3. div.innerHTML = "<div></div><script defer> alert('hello!');<\/script>";

4. div.innerHTML = "<div>&nbsp;</div><script defer> alert('hello!');<\/script>";

5. div.innerHTML = "<input type=\"hidden\"/><script defer> alert('hello!');<\/script>";
```

譬如上述的一系列设置都不会执行（chrome等一系列现代浏览器中的效果）

IE8中则是只要脚本加上了defer属性，并且前面有一个“有作用域元素”，就可以正常执行。（这里不过度描述ie8）

'无作用域的元素'(NoScope element):

如`style`，`script`元素或注释类似。
有作用域则相反

### DOM操作，怎么添加、移除、移动、复制、创建和查找节点？

1.创建

```js
createDocumentFragment() // 创建一个dom片段
createElement() // 创建一个具体元素
createTextNode() // 创建一个文本节点
```

2.添加，移除，替换，插入

```js
appendChild()
removeChild()
replaceChild()
insertBefore()
```

3.查找

```js
getElementsByTagName()
getElementsByClassName()
getElementsByName()
getElementById()
document.querySelector()
```

### children与childNodes的区别？

children只包含元素中同样还是元素的子节点（不包含普通文本节点）

childNodes会包含空白符和文本节点

### 屏幕坐标

### 鼠标事件中的screenX，clientX与pageX的区别

screenX是相对于整个屏幕的坐标（整个显示器屏幕），所以在浏览器可视区域外也会计算的。

clientX是相对于视口（浏览器可视窗口）的坐标（不计算页面滚动）

pageX是相对于页面的坐标（包含滚动）

### getBoundingClientRect与offset的区别

getBoundingClientRect()是用来获取页面元素的位置的方法.
这个方法最终返回的是一个矩形对象,
包括四个属性:left top right bottom

**譬如top指的是距离可视区域顶部的距离**

而后者offsetTop则包括滚动条卷起的部分

**注意，是相对其祖先元素，如果要计算绝对的坐标，需要将祖先元素的offert也计算出**

## 操作符规则

### `<,>,<=,>=`的比较规则

所以比较运算符都支持任意类型，但是比较只支持数字和字符串，所以需要执行必要的转换然后进行比较，转换规则如下：

1.如果操作数是对象，转为原始值：如果valueOf方法返回原始值，则使用这个值，否则使用toString方法的结果，如果转换失败则报错

2.经过必要的对象到原始值的转换后，如果两个操作数都是字符串，按照字符串顺序进行比较（它们的16位unicode值的大小）
注意，不是字母表中的位置，是编码，譬如B的编码是66，a的编码是97，所以B < a

3.否则，如果有一个操作数不是字符串，将两个操作数转位数字进行比较

4.NaN与任何结果比较都返回false

### ==运算符判断相等的流程是怎样的

1.如果两个值类型相同，按照===比较方法进行比较

2.如果类型不同，使用如下规则进行比较

3.如果其中一个值是null，另一个是undefined，它们相等

4.如果一个值是数字另一个是字符串，将字符串转换为数字进行比较

5.如果有布尔类型，将true转换为1，false转换为0，然后用==规则继续比较

6.如果一个值是对象，另一个是数字或字符串，将对象转换为原始值然后用==规则继续比较

7.其他所有情况都认为不相等
譬如NaN与任何数都不相等包括它本身
undefined和null两者都与0是不相等的

### ===运算符判断相等的流程是怎样的

1.如果两个值不是相同类型，它们不相等

2.如果两个值都是null或者都是undefined，它们相等

3.如果两个值都是布尔类型true或者都是false，它们相等

4.如果其中有一个是NaN，它们不相等

5.如果都是数值型并且数值相等，他们相等， -0等于0

6.如果他们都是字符串并且在相同位置包含相同的16位值，他它们相等;
如果在长度或者内容上不等，它们不相等；
两个字符串显示结果相同但是编码不同==和===都认为他们不相等

7.如果他们指向相同对象、数组、函数，它们相等；
如果指向不同对象，他们不相等

### ,号操作符

```js
var num = (5, 1, 4, 3, 2); // num的值为2
```

逗号操作符总会返回表达式中最后一项

### ~~和Math.floor()的区别

首先，两者都能实现的功能就是：取整

如

```js
~~1.1; // 1
Math.floor(1.1); // 1
```

区别在于：
javascript内部的数值默认按IEEE-754 64位格式存储

而~~属于位操作，位操作只会在32位上进行，所以会将值先转位32位，执行完操作后再转回去
而Math.floor是直接在64位上操作

所以说，两者的适合精度不一样，Math.floor更通用，但是~~效率更高

### !运算符的工作流程

这是逻辑非操作

1.如果操作数是一个对象，返回false

2.如果操作对象是一个空字符串，返回true

3.如果操作对象是一个非空字符串，返回false

4.如果操作对象是0，返回true

5.如果操作对象是非0数值（包括Infinity），返回false

6.如果操作对象是NaN，返回true

7.如果操作对象是null，返回true

8.如果操作对象是undefined，返回true

#### ++和--运算符的工作流程

数值的或直接++或--，其它的

1.如果应用对象是一个包含有效数字的字符串时，会先将其转换为数字，然后++或--，字符串变数值

2.如果不包含有效数字，将变量的值设置为NaN，字符串变数值

3.如果是false，先变为0，然后++或--，布尔变数值

4.如果是true，先变为1，然后++或--，布尔变数值

5.如果是浮点数字，直接++或--

6.应用于对象，先调用对象的valueOf()以取得一个可操作的值，按照前述规则解析，如果是NaN，继续调用toString()，
继续前述规则解析，对象变数值

### +和-运算符的工作流程

该操作符会像Number()一样对值进行转换

1.false,true-0,1

2.字符串按照特殊规则解析

3.对象先调用valueOf()，如果非法则调用toString()

### *运算符工作流程

1.如果操作符都是数值，执行常规的乘法操作，即正负得负，负负得正，
如果乘积超过了ECMAScript数值的表示范围，则返回Infinity或-Infinity

2.如果有一个操作数是NaN，则结果是NaN

3.如果是Infinity与0相乘，结果是NaN

4.如果是Infinity与非0相乘，则结果是Infinity或-Infinity，取决于有符号操作数的符号。

5.如果是Infinity与Infinity相乘，则结果是Infinity

6.如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用到上面的规则

### /运算符工作流程

1.如果操作符都是数值，执行常规的除法操作，
如果商超过了ECMAScript数值的表示范围，则返回Infinity或-Infinity

2.如果有一个操作数是NaN，则结果是NaN

3.如果是Infinity被Infinit除，结果是NaN

4.如果是零被零除，则结果是NaN

5.如果是非零的有限数被零除，则结果是Infinity或-Infinity，取决于有符号操作数的符号

6.如果是Infinity被任何非零数值除，则结果是Infinity或-Infinity，取决于有符号操作数的符号。

7.如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用于上面的规则。

### 求模运算符工作流程

1.如果两个操作符都是数值，执行常规的除法计算，返回除得的余数

2.如果被除数是无穷大值而除数是有限大的数值，返回NaN

3.被除数是有限大的值，而除数是零，则结果是NaN

4.如果是Infinity被Infinity除，则结果是NaN

5.被除数是有限大的数值而除数是无穷大的数值，则结果是被除数。

6.如果被除数是零，则结果是零

7.如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用于上面的我规则

### 加性操作符工作流程

1.如果有一个操作数是NaN，则结果是NaN

2.如果是Infinity加Infinity，结果是Infinity

3.如果是-Infinity加-Infinity，结果是-Infinity

4.如果是Infinity加-Infinity，结果是NaN

5.如果是+0加+0，结果是+0

6.如果是-0加-0，结果是-0

7.如果是+0加-0，结果是+0

不过，如果有一个操作符是字符串，则运用一下规则

1.如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接

2.如果只有一个操作数是字符串，则将另一个操作数转为字符串，然后两个操作数拼接

3.如果有一个操作数是对象，数值或布尔，则调用它们的toString()方法获取相应的字符串值，然后再应用前面关于字符串的规则，
对于undefined和null，分别调用String()函数并取得字符串'undefined'和'null'

注意：

```js
'1' + 2 + 3; // 结果是123
```

### 减性操作符工作流程

1.如果两个操作符都是数值，则执行常规的算数操作符并返回结果

2.如果有一个操作符是NaN，返回NaN

3.如果是Infinity减Infinity，结果是NaN

4.如果是-Infinity减-Infinity，结果是NaN

5.如果是Infinity减-Infinity，结果是Infinity

6.如果是-Infinity减Infinity，结果是-Infinity

7.如果是+0减+0，结果是+0

8.如果是-0减+0，结果是-0

9.如果是-0减-0，结果是+0

10.如果有一个操作符是字符串，布尔值，undefined或null，则在后台先调用Number()转为数值，
然后再根据前面的规则执行减法计算，
如果转换结果是NaN，则减法结果是NaN

11.如果有一个操作符是对象，则调用对象的valueOf()获取改对象的值，如果得到NaN，
那么结果是NaN，
如果对象没有valueOf()，则调用toString()并转为数值

### 连等号赋值顺序

注意，**不推荐使用连等赋值**

```js
var a = {n: 1}
var b = a;
a.x = a = {n: 2}
console.log(a); // {n: 2}
console.log(a.x); // undefined
console.log(b); // {n: 1, x: {n: 2}}
console.log(b.x) // {n: 2}
```

因为连等号这个语句中会**先确定所有遍历的指针**，然后才会去对于赋值

其中

```js
a.x = a = {n: 2} 
```

- 指针确定如下
a.x的指针已经确定了，指向了原始a的（因为原始a没有x，因此创建了一个指向null的指针）
a指向也是原始a

- 赋值如下
a重新指向到了新的地址 {n: 2}（栈中的指针指向了堆中新的对象）
原始a.x的指向到了 {n: 2}

- 因此最后
a指向到了新的{n: 2}
a.x为undefined

b指向原始a
b.x = {n: 2}

简单的理解，因为js中是值传递模式，所以在连等开始赋值之前，以及分别有`a.x`和`a`这两个指针的值的。
最初时，`a`和`a.x`分别指向堆内存中的`{n: 1, x: null}`以及里面的`x`。
然后赋值阶段，`a`重新换了一个指向，指向了`{n: 2}`，而`a.x`仍然是原始的a（也就是和另一个备份的`b`指向一样）

### 为什么说+拼接字符串效率低

因为js中，字符串是原始值，创建后是无法更改的（栈内存中）

```js
var lang = 'hello';

lang = lang + ' world';
```

1.变量lang开始时包含字符串'hello'

2.第二行代码把'hello'重新定义为'hello'与' world'的组合

实现这个操作的过程如下：
1.首先创建一个新的字符串（容纳组合的所有字符）
2.然后这个字符串填充'hello'与' world'的组合
3.销毁原来的字符串'hello'与' world'（因为这两个字符串已经没用了）

这也是为什么某些旧版浏览器字符串拼接时速度很慢

不过，一般新版的浏览器中已经修复了这个问题，当然了，一般情况下我们还是会避免代码拼接字符串的

### Object.is与原来的比较操作符 ===， ==的区别？

ES6才新增
两等号判等，会在比较时进行类型转换；
三等号判等(判断严格)，比较时不进行隐式类型转换,（类型不同则会返回false）；

Object.is 在三等号判等的基础上特别处理了 NaN 、-0 和 +0 ，保证 -0 和 +0 不再相同，
但 Object.is(NaN, NaN) 会返回 true.

Object.is 应被认为有其特殊的用途，而不能用它认为它比其它的相等对比更宽松或严格。

## 类型判断

## 判断一个对象是否是数组？

1.`Array.isArray(arr)`(ECMAScript5引入-当然了，不考虑ie8)
2.`arr instanceof Array`
3.`arr.constructor === Array`

注意，由于跨iframe实例化的对象彼此不共享原型，因此2，3检测可能会有问题

4.`Object.prototype.toString.call(arr) === '[object Array]'`最保险的一种判断

### 判断是否是函数

```js
/**
 * 判断对象是否为函数，如果当前运行环境对可调用对象（如正则表达式）
 * 的typeof返回'function'，采用通用方法，否则采用优化方法
 *
 * @param {Any} arg 需要检测是否为函数的对象
 * @return {boolean} 如果参数是函数，返回true，否则false
 */
function isFunction(arg) {
    if (arg) {
        // 解决以前的一些老浏览器正则表达式返回object的bug
        if (typeof (/./) !== 'function') {
            return typeof arg === 'function';
        } else {
            return Object.prototype.toString.call(arg) === '[object Function]';
        }
    }
    return false;
}
```

```js
解释：
typeof (/./) !== 'function'的作用是-
typeof /./ === 'function'; // Chrome 1-12 , 不符合 ECMAScript 5.1
typeof /./ === 'object'; // Firefox 5+ , 符合 ECMAScript 5.1
```

### typeof知多少

typeof可以用来检测给定变量的值的数据类型，可能的返回值如下：

undefined:undefined类型
boolean:boolean类型
string:string类型
number:number类型
function:这个值是函数,是一种object拓展的特殊类型
Object:null类型或者其它的object类型和object拓展类型(去除Function)
symbol:对es6中新增的symbol类型

### instanceof知多少

instanceof用于判断一个变量是否是某个对象的实例，
主要判断某个构造函数的prototype属性是否在另一个要检查对象的原型链上

instanceof可以用于判断是否原型链继承，可以判断内置的对象类型(基于obejct拓展的,如Array,Date等)，可以判断自定义类型，
但是不能判断简单类型(因为本质是通过原型来判断，但是简单类型只是一个常量，并不是Object)

`a instanceof b`真正的语义是检查` b.prototype `是否在 a 的原型链上，仅此而已。

所以，对b会有要求，如果b没有原型链，会报错，譬如

```js
obj instanceof undefined 会报错，Right-hand side of 'instanceof' is not an object
如果obj.prototype = undefined;
那么123 instanceof obj 会报错，Right-hand side of 'instanceof' is not callable
```

可以看出，如果右侧的值没有prototype对象，会报错

### Object.prototype.toString知多少

Object.prototype.toString的可以用于解决typeof和instanceof的不足
比如typeof无法识别内置类型(Array Date等),而instanceof无法识别简单类型。

Object.prototype.toString可以识别5种简单类型，以及全部内置类型(Array.Date等一些内置类型)，
但是无法识别自定义类型（如自己创一个Parent，Child类无法识别出来-Object，instanceof是因为原型链上对比，所以可以匹配）

每种内置对象都定义了 [[Class]] 内部属性的值。
宿主对象的 [[Class]] 内部属性的值可以是除了
 "Arguments", "Array", "Boolean", "Date", "Error", "Function", "JSON", "Math", "Number", "Object", "RegExp", "String"
的任何字符串。
  
[[Class]] 内部属性的值用于内部区分对象的种类。
除了通过 `Object.prototype.toString` 没有提供任何手段使程序访问此值。
这也是它为什么能获取内置对象的类别

在ES6里，之前的 [[Class]] 不再使用，取而代之的是一系列的 internal slot ，有一个比较完整的解释：
Internal slot 对应于与对象相关联并由各种ECMAScript规范算法使用的内部状态，它们没有对象属性，也不能被继承，
根据具体的 Internal slot 规范，这种状态可以由任何ECMAScript语言类型或特定ECMAScript规范类型值的值组成。

此外，通过对 Object.prototype.toString 在ES6的实现步骤分析，
我们其实可以很容易改变 Object.prototype.toString.call 的结果，像下面一样：

```js
let obj = {}

Object.defineProperty(obj, Symbol.toStringTag, {
    get: function() {
        return "newClass"
    }
})

console.log(Object.prototype.toString.call(obj)) // "[object newClass]"
```

### 如何判断一个对象是否属于某个类？

一般常用的两种：

1. instanceof

```js
a instanceof Date;
```

可以判断`Date.prototype`是否有出现在`a`的原型链上

2. Object.prototype.toString

```js
const getClassName = (object) => Object.prototype.toString.call(object).match(/^\[object\s(.*)\]$/)[1];

getClassName('sss') === 'String'; // true
```

可以直接输出内部隐藏的`[[Class]]`对象，内置对象可以直接识别（`String`），普通的对象都是`Object`

当然了，es6中，可以通过`Symbol.toStringTag`修改

```js
Object.defineProperty(a, Symbol.toStringTag, {
    get: function() {
        return "Date"
    }
});

Object.prototype.toString.call(a); // [object Date]
```

## 类型转换

### 对象到字符串的转换步骤

1.如果对象有toString()方法，javascript调用它。如果返回一个原始值（primitive value如：string number boolean）,将这个值转换为字符串作为结果
如果tostring返回{}，算有效

2.如果对象没有toString()方法或者返回值不是原始值，javascript寻找对象的valueOf()方法，如果存在就调用它，返回结果是原始值则转为字符串作为结果

3.否则，javascript不能从toString()或者valueOf()获得一个原始值，此时throws a TypeError

### 对象到数字的转换步骤

1.如果对象有valueOf()方法并且返回元素值，javascript将返回值转换为数字作为结果

2.否则，如果对象有toString()并且返回原始值，javascript将返回结果转换为数字作为结果

3.否则，throws a TypeError

### 隐式转换

隐式转换是按ECMAScript规范定义来进行的，但是规则很多，不好记，最好从实践出发，
如果下述示例都知道，应该掌握的差不多了（参考了github文章，在refer的参考来源）

```js
// []的tostring会调用元素的tostring然后拼接，空元素默认为空字符串
[].toString(); // 空字符串

// 转为字符串然后拼接
[1, 2].toString(); // 1,2

// 子元素转为字符串然后拼接
[{}, {}].toString(); // [object Object],[object Object]

// 默认返回它本身
[].valueOf(); // []

// valueof如果不是原始值就会去找tostring，然后转为数字，所以是0
+[]; // 0

// valueof不为原始值，转为字符串为1，然后转数字
+[1]; // 1

// valueof不为原始值，转为字符串为1,2，然后转数字NaN
+[1, 2]; // NaN

// 默认返回它本身
{}.valueOf(); // {}

// 分别是原始值转字符串让拼接
{}+{}; // [object Object][object Object]

// 隐式转换时[]转成了字符串
{}+[]; // [object Object]

// 隐式转换时[]转成了字符串
[]+{}; // [object Object]

// 隐式转换时{}转成了字符串
{}+1; // [object Object]1

// 隐式转换时{}转成了字符串
({}+1); // [object Object]1

// 隐式转换时{}转成了字符串
1+{}; // 1[object Object]

// 隐式转换时[]valueof不为原始值，所以转成了字符串，然后再转数字，为0
[]+1; // 1

// 隐式转换时[]valueof不为原始值，所以转成了字符串，然后再转数字，为0
1+[]; // 1

// 隐式转换时[]valueof不为原始值，所以转成了字符串，然后再转数字，为0
1-[]; // 1

// {}会调用valueof，valueof不为原始值后再调用tostring，然后由于无法转换成数字（[object Object]），所以NAN
+{}; // NAN

//先接以上（因为-号会先尽量转为数字），NAN，然后NAN和任何数都是NAN
1-{}; // NaN

// !{}为false，然后转为数字计算
1-!{}; // 1

// !{}为false，然后转为数字计算
1+!{}; // 1

// 字符串拼接
1+"2"+"2"; // 122

// +"2"的优先级高，然后数字的1+2为3，然后再字符串拼接
1+ +"2"+"2"; // 32

1++"2"+"2"; // 会报错：Invalid left-hand side expression in postfix operation

// 先计算![]为false，然后[]和false比，隐式转换成数字比较，因此相等（都是0）
[]==![]; // true

// 因为类型不匹配，所以直接就是false
[]===![]; // false

const obj = [];

obj.valueOf = function() {
    return 22;
};

obj.toString = function() {
    return '11';
};

// 转为数字时，会优先valueOf，所以是22
console.log(+obj); // 22
```

## 继承

### ES5中js经典的继承代码（最公认有效的那个）

```js
    // subClass的构造中，需要
    superClass.apply(this, arguments);
    
    ...
    
    function inherit(subClass, superClass) {  
        function F() {}
        F.prototype = superClass.prototype;
        // 将实例作为子类的原型
        // 为什么不直接 new superClass()，因为new superClass消耗的内存更多，而一个空对象消耗的较少
        subClass.prototype = new F();
        subClass.prototype.constructor = subClass;
    }
```

### 有试过继承Date对象么？

首先，一般情况下，这个需求都是，由于需要自己定制拓展一个Date工具类才有的。

但是，Date是无法继承的（指的是是经典的那种继承法）。MDN文档有如下说明：

```js
Note: Note that Date objects can only be instantiated by calling Date or using it as a constructor;
unlike other JavaScript object types, Date objects have no literal syntax.
```

看起来就是，Date并不是一个普通的对象，而是被特殊定制过的。所以无法让你去直接继承。

然后再看看V8中的代码

```js
function DateGetHours() {
  var t = DATE_VALUE(this);
  if (NUMBER_IS_NAN(t)) return t;
  return HOUR_FROM_TIME(LocalTimeNoCheck(t));
}

...

DATE_VALUE(arg) = (%_ClassOf(arg) === 'Date' ? %_ValueOf(arg) : ThrowDateTypeError());
```

所以，其实v8并不允许让Date被继承。另外，进一步，你会发现，
就算改变`Object.prototype.toString.call`的值，仍然无效。这是因为它依赖于内部属性`[[Class]]`的缘故，这个属性不是`Date`，就无效

当然，经典的继承无法使用，并不代表真的没有方法，譬如以下一些：

- 可以用以下方法进行伪继承：

```js
function MyDate() {
   var _d=new Date();
   function init(that) {
      var i;
      var which=['getDate','getDay','getFullYear','getHours',/*...*/,'toString'];
      for (i=0;i<which.length;i++) {
         that[which[i]]=_d[which[i]]; 
      }
   }
   init(this);
   this.doSomething=function() {
    console.log("DO");
   }
}
```

上述代码仅是距离，实际上可以结合原型，总的来说，相当于是把Date原有的方法全部代理一遍。

- 可以通过原型链欺骗方式（核心是原型链的指向），如下

```js
    /**
     * Date无法被直接继承，需要用些技巧
     * 参考：https://stackoverflow.com/questions/6075231/how-to-extend-the-javascript-date-object
     */
    function MyDate() {
        var dateInst = new (Function.prototype.bind.apply(
            Date,
            [Date].concat(Array.prototype.slice.call(arguments))
            ));
        
        // 更改原型指向，否则无法调用MyDate原型上的方法
        Object.setPrototypeOf(dateInst, MyDate.prototype);
        
        return dateInst;
    }
    
    // 原型重新指回Date，否则根本无法算是继承
    Object.setPrototypeOf(MyDate.prototype, Date.prototype);
    
    MyDate.prototype.format = function(fmt) {
        ...
    };

    var myDate = new MyDate();
    
    myDate.getFullYear();
    myDate.format();
```


核心思路是：

```js
- 返回的对象仍然是Date（相当于是寄生模式），所以符合底层的类型判断

- 然后这个Date的原型指向了SubDate，所以它可以调用到SubDate原型上的方法

- 然后SubDate的原型又指回了Date，这样的话Date又回到了原型上（判断类型也方便，看起来也更像继承了）

- 有一个缺点是无法覆盖date原有方法（不过看起来并不是缺点，因为本来就不应该覆盖）
```

### es5如何实现super关键字

首先了解super的一些关键点：

- super关键字只能在class内部使用，外部直接调用就会出错（原因是根本不知道父类的构造函数是哪个）

- super本质上就是借用构造函数的一种表现形式


构造函数中的`super()`本质是一个语法糖。作用是借用父类的构造函数。

`siper.xx()`的作用是通过`[[prototype]]`回溯到父类的原型方法（或静态方法），然后用`parent.fun.apply(this, argument)`调用之类的
