!#brushQuestions

### JavaScript 的基本类型有哪些？引用类型有哪些？null 和 undefined 的区别？（必会） 

**数据类型** 

基本数据类型：number、string、boolean、null、undefined 

引用数据类型：function、object、Array 

**区别** 

undefined:表示变量声明但未初始化时的值 

null：表示准备用来保存对象，还没有真正保存对象的值。从逻辑角度看，null 值表示一个空对象指针 

JavaScript（ECMAScript 标准）里共有 5 种基本类型：Undefined，Null，Boolean，Number，String，和一种复杂类型 Object。可以看到 null 和 undefined 分属不同型，未初定义的值用 typeof 检测出来是"undefined"(字符串)，而 null 值用 typeof 检来是"object"（字符串）。任何时候都不建议显式的设置一个变量为 undefined，但是如果保对象的变量还没有真正保存对象，应该设置成 null。实际上，undefined 值是派生自 null的，ECMAScript规定对二者进行相等性测试要返回 true

### 如何判断 JavaScript 的数据类型？（必会） 

JavaScript 数据类型一共有 7 种： 

Undefined、Null、Boolean、String、Symbol、Number、Object ，除了 Object 之外的 6 种属于原始数据类型。有时，我们还会细分 Object 的类型，比如：Array， Function，Date，RegExp 等 

**判断方法** 

typeof 

typeof 可以用来区分除了 Null 类型以外的原始数据类型，对象类型的可以从普通对象里面 

识别出函数：

```javascript
typeof undefined // "undefined" 
typeof null // "object" 
typeof 1 // "number" 
typeof "1" // "string" 
typeof Symbol() // "symbol" 
typeof function () { } // "function" 
typeof {} // "object"
```

问题一：typeof 不能识别 null，如何识别 null？ 

答案：如果想要判断是否为 null，可以直接使用===全等运算符来判断（或者使用下面的Object.prototype.toString 方法）：

```javascript
let a = null 
a === null // true
```

问题二：typeof 作用于未定义的变量，会报错吗？ 

答案：不会报错，返回"undefined"。

```javascript
typeof randomVariable // "undefined"
```

问题三：typeof Number(1)的返回值是什么？ 

答案："number"。注意 Number 和 String 作为普通函数调用的时候，是把参数转化为相应的原始数据类型，也就是类似于做一个强制类型转换的操作，而不是默认当做构造函数调用。注意和 Array 区分，Array(...)等价于 new Array(...)

```javascript
typeof Number(1) // "number" 
typeof String("1") // "string" 
Array(1, 2, 3)
// 等价于 
new Array(1, 2, 3)
```

问题四：typeof new Number(1)的返回值是什么？ 

答案："object"。

```javascript
typeof new Number(1) // "object" 
typeof new String(1) // "object"
```

instanceof

instanceof 不能用于判断原始数据类型的数据： 

```javascript
3 instanceof Number // false 
'3' instanceof String // false 
true instanceof Boolean // false
```

instanceof 可以用来判断对象的类型：

```javascript
var date = new Date() 
date instanceof Date // true 
var number = new Number() 
number instanceof Number // true 
var string = new String() 
string instanceof String // true
```

需要注意的是，instanceof 的结果并不一定是可靠的，因为在 ECMAScript7 规范中可以通过自定义 Symbol.hasInstance 方法来覆盖默认行为。 

Object.prototype.toString

```javascript
Object.prototype.toString.call(undefined).slice(8, -1) // "Undefined" 
Object.prototype.toString.call(null).slice(8, -1) // "Null" 
Object.prototype.toString.call(3).slice(8, -1) // "Number" 
Object.prototype.toString.call(new Number(3)).slice(8, -1) // "Number" 
Object.prototype.toString.call(true).slice(8, -1) // "Boolean" 
Object.prototype.toString.call('3').slice(8, -1) // "String" 
Object.prototype.toString.call(Symbol()).slice(8, -1) // "Symbol"
```

由上面的示例可知，该方法没有办法区分数字类型和数字对象类型，同理还有字符串类型和字符串对象类型、布尔类型和布尔对象类型 

另外，ECMAScript7 规范定义了符号 Symbol.toStringTag，你可以通过这个符号自定义 

Object.prototype.toString 方法的行为：

```javascript
'use strict'
var number = new Number(3)
number[Symbol.toStringTag] = 'Custom'
Object.prototype.toString.call(number).slice(8, -1) // "Custom" 
function a() { } 
a[Symbol.toStringTag] = 'Custom'
Object.prototype.toString.call(a).slice(8, -1) // "Custom" 
var array = [] 
array[Symbol.toStringTag] = 'Custom'
Object.prototype.toString.call(array).slice(8, -1) // "Custom"
```

因为 Object.prototype.toString 方法可以通过 Symbol.toStringTag 属性来覆盖默认行为，所以使用这个方法来判断数据类型也不一定是可靠的 

Array.isArray 

Array.isArray(value)可以用来判断 value 是否是数组：

```javascript
Array.isArray([]) // true 
Array.isArray({}) // false 
  (function () { console.log(Array.isArray(arguments)) }()) // false
```
### 创建对象有几种方法（必会）
```javascript
// 创建对象有几种方法（必会） 创建方法
// 1、字面量对象 // 默认这个对象的原型链指向 object 
var o1 = { name: 'o1' };

// 2、通过 new Object 声明一个对象 
var o2 = new Object({ name: 'o2' });

// 3、使用显式的构造函数创建对象
var M = function () {
  this.name = 'o3'
};
var o3 = new M();
o3.__proto__ === M.prototype
// o3 的构造函数是 M ，o3 这个普通函数，是 M 这个构造函数的实例 

// 4、Object.create() 
var P = { name: 'o4' };
var o4 = Object.create(P);
```

### 简述创建函数的几种方式？ （必会）

```javascript
// 第一种（函数声明）
function sum1(num1, num2) {
  return num1 + num2;
}

// 第二种（函数表达式）
let sum2 = function (num1, num2) {
  return num1 + num2;
}

// 第三种（函数对象形式）
let sum3 = new Function('num1', 'num2', 'return num1 + num2')

```


### Javascript 创建对象的几种方式？ （必会）

**1、简单对象的创建**

使用对象字面量{ }创建一个对象（最简单，好理解，推荐使用）

代码如下

```javascript
let Cat = {}
Cat.name = 'kity' // 添加属性并赋值
Cat.age = 2
Cat.sayHello = function () {
  alert('hello' + Cat.name + ',今年' + Cat.age + '岁了')
}

Cat.sayHello();
```

**2、用 function（函数）来模拟class**
    2.1 创建一个对象，相当于new一个类的实例（无参构造函数）
代码如下

```javascript
function Person() {

}

let personOne = new Person() // 定义一个 function，如果有new关键字去“实例化”，那么该function可以看作是一个类
personOne.name = 'dylan'
personOne.hobby = 'coding'
personOne.work = function () {
  alert(personOne.name + 'is coding now...')
}

personOne.work()
```

​    2.2 可以使用有参构造函数来实现，这样定义更方便，扩展性更强（推荐使用）

代码如下

```javascript
function Pet(name, age, hobby) {
  this.name = name
  this.age = age
  this.hobby = hobby
  this.eat = function () {
    alert('我叫' + this.name + '，我喜欢' + this.hobby + '，也是个吃货')
  }
}
let maidou = new Pet('麦兜', 5, '睡觉')
maidou.eat()
```

**3、使用工厂函数方式来创建（Object 关键字）**
代码如下：

```javascript
let wcDog = new Object()
wcDog.name = '旺财'
wcDog.age = 3
wcDog.work = function () {
  alert('我是' + wcDog.name + '，汪汪汪....')
}
wcDog.work()
```

**4、使用原型对象的方式 prototype 关键字**
代码如下：

```javascript
function Dog() { }
Dog.prototype.name = "旺财"
Dog.prototype.eat = function () {
  alert(this.name + '是个吃货')
}
let wangcai = new Dog()
wangcai.eat()
```

**5、混合模式（原型和构造函数）**
代码如下：

```javascript
function Car(name, price) {
  this.name = name
  this.price = price
}

Car.prototype.sell = function () {
  alert('我是' + this.name + '，我现在卖' + this.price + '万元')
}
let camry = new Car('凯美瑞', 27)
camry.sell();
```

**6、动态原型的方式（可以看作是混合模式的一种特例）**
代码如下：

```javascript
function Car(name, price) {
  this.name = name
  this.price = price
  if (typeof Car.sell == 'undefined') {
    Car.prototype.sell = function () {
      alert('我是' + this.name + '，我现在卖' + this.price + '万元')
    }
    Car.sell = true
  }
}
let camry = new Car('凯美瑞', 27)
camry.sell();
```



### 请指出 JavaScript 宿主对象和原生对象的区别？（必会）

**原生对象** 

ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript实现提供的对象”，“本地对象”包含哪些内容：Object、Function、Array、String、Boolean、Number、Date、RegExp、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeErroURIError由此可以看出，本地对象就是 ECMA-262 定义的类（引用类型） 

**内置对象**

ECMA-262 把内置对象（built-inobject）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了同样是“独立于宿主环境”。根据定义我们似乎很难分清“内置对象”与“本地对象”的区别。而ECMA-262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。如此就可以理解了。内置对象是本地对象的一 种 

**宿主对象** 

何为“宿主对象”？主要在这个“宿主”的概念上，ECMAScript 中的“宿主”当然就是我 们网页的运行环境，即“操作系统”和“浏览器” 

所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供对象。所有的 BOM 和 DOM 都是宿主对象。因为其对于不同的“宿主”环境所展示的同。其实说白了就是，ECMAScript 官方未定义的对象都属于宿主对象，因为其未定 义的对数是自己通过 ECMAScript 程序创建的对象

### JavaScript 内置的常用对象有哪些？并列举该对象常用的方法？（必会） 



**对象及方法** 

函数参数集合
Arguments    一个函数的参数和其他属性 
Arguments.callee    当前正在运行的函数 
Arguments.length    传递给函数的参数的个数

**Array数组**

length    属性 动态获取数组长度 
join()    将一个数组转成字符串。返回一个字符串 
reverse()    将数组中各元素颠倒顺序 
delete    运算符 只能删除数组元素的值，而所占空间还在，总长度没变(arr.length) 
shift()    删除数组中第一个元素，返回删除的那个值，并将长度减 1 
pop()    删除数组中最后一个元素，返回删除的那个值，并将长度减 1
unshift()    往数组前面添加一个或多个数组元素，长度要改变。arrObj.unshift(“a” ， “b，“c”)
push()    往数组结尾添加一个或多个数组元素，长度要改变。arrObj.push(“a” ，“b”， “c”) 
concat( )    连接数组 
slice( )    返回数组的一部分 
sort( )    对数组元素进行排序 
splice( )    插入、删除或替换数组的元素 
toLocaleString( )    把数组转换成局部字符串
toString( )	将数组转换成一个字符串
forEach	遍历所有元素var  

```javascript
arr = [1, 2, 3];
arr.forEach(function (item, index) {
  // 遍历数组的所有元素console.log(index, item);
});
```
every 判断所有元素是否都符合条件
```javascript
let arr = [1, 2, 3]
let arr1 = arr.every(function (item) {
  if (item < 4) {
    return true
  }
})
console.log(arr1)
```

sort 排序
```javascript
let arr = [1, 5, 2, 7, 3, 4]
let arr2 = arr.sort(function(a, b) {
  //从小到大，升序
  return a - b
  // 从大到小，倒序
  return b - a
})
console.log(arr2)
```
filter 过滤符合元素
```javascript
let arr = [1, 2, 3, 4]
let arr2 = arr.filter(function(item) {
  if (item > 2) {
    return true
  }
})
console.log(arr2)
```

**Boolean**

Boolean.toString() 将布尔值转换成字符串
Boolean.valueOf() Boolean 对象的布尔值


**Date 日期时间**
（1）创建当前（现在）日期对象的实例，不带任何参数
```javascript
let today = new Date() // Date Wed Jul 14 2021 16:10:40 GMT+0800 (中国标准时间)
```
（2）创建指定时间戳的日期对象实例，参数是时间戳
时间戳：是指某一个时间距离 1970 年 1 月 1 日 0 分 0 秒，过去了多少毫秒（1秒=1000毫秒）
```javascript
let timer = new Date(10000) // Date Thu Jan 01 1970 08:00:10 GMT+0800 (中国标准时间)
```
（3）指定一个字符串的日期时间信息，参数是一个日期时间字符串
```javascript
let timer = new Date('2015/5/25 10:00:00') // Date Mon May 25 2015 10:00:00 GMT+0800 (中国标准时间)
```
（4）指定多个数值参数
```javascript
// 参数顺序是：年、月、日、时、分、秒；年、月、日时必须的
let timer = new Date(2015+100, 4, 25, 10, 20 ,0) // Date Sat May 25 2115 10:20:00 GMT+0800 (中国标准时间)
```
Date对象方法：
参考 [JavaScript Date 对象 ](https://www.runoob.com/jsref/jsref-obj-date.html)



**Error 异常对象**

Error.message    可以读取的错误消息
Error.name    错误的类型
Error.toString()    把Error 对象转换成字符串
SyntaxError    抛出该错误来通知语法错误
RangeError    抛出该错误用来通知语法错误
ReferenceError    在读取不存在的变量时抛出
TypeError    当一个值的类型错误时，抛出该异常
URIError    由URI的编码和解码方法抛出

**Function 函数构造器**

Function.apply()    将函数作为一个对象的方法调用
Function.arguments[]    传递给函数的参数
Function.call()    将函数作为对象的方法调用
Function.length    已声明的参数的个数
Function.prototype    函数的原型
Function.toString()    把函数转换为字符串

**Math 数学对象**

Math    对象是一个静态对象
Math.PI    圆周率
Math.abs()    绝对值
Math.ceil()    向上取整（整数加1，小数去掉）
Math.floor()    向下取整（直接去掉小数）
Math.round()    四舍五入
Math.pow(x, y)    求 x 的 y 次 方
Math.sqrt()    求平方根

**Number 数值对象**

MAX_VALUE    最大数值
MIN_VALUE    最小数值
NaN    特殊的非数字值
NEGATIVE_INFINITY    负无穷大
POSITIVE_INFINTY    正无穷大
toExponential()    用指数计数法格式化数字
toFixed()    采用定点计数法格式化数字
toLocaleString()    把数字转换成本地格式的字符串
toPrecision()    格式化数字的有效位
toString()    将一个数字转换成字符串
valueOf()    返回原始数值

**Object 基础对象**

Object    含有所有 Javascript 对象的特性的超类
constrctor    对象构造函数
hasOwnProperty()    指示对象自身属性中是否具有指定的属性
isProtoypeOf()    是否可以通过for/in 循环看到属性
toLocaleString()    返回对象的本地字符串表示形式
toString()    定义一个对象的字符串表示形式
valueOf()    返回对象的原始值

**RegExp 正则表达式对象**

exec()    通用的匹配模式
global    正则表达式是否全局匹配
ingoreCase    正则表达是否区分大小写
lastIndex    下次匹配的起始位置
test()    检测一个字符串是否匹配某个模式
toString()    把正则表达式转换成字符串

**String 字符串对象**
length    获取字符串的长度
toLowerCase()    将字符串的字母转成全小写
toUpperCase()    将字符串中的字母转成全大写
charAt(index)    返回指定下标位置的一个字符，如果没有找到，则返回空字符串
substr()    在原始字符串，返回一个字符串
substring()    在原始字符串，返回一个字符串
区别：

```javascript
'abcefg'.substring(2, 3) = 'c'
'abcefg'.substr(2, 3) = 'cde'
```

split()    将一个字符串转成数组
charCodeAt()    返回字符串中的第 n 个字符的代码
concat()    连接字符串
fromCharCode()    从字符编码创建一个字符串
indexOf()    返回一个子字符串在原始字符串中的索引值（查找顺序从左往右查找）。如果没找到，则返回-1
lastIndexOf()  从后向前检索一个字符串  
localeCompare()    用本地特定的顺序来比较两个字符串
match()    找到一个或多个正则表达式的匹配
replace()    替换一个与正则表达式匹配的子串
search()    检索与正则表达式匹配的子串
slice()    抽取一个字串
toLocaleLowerCase()    把字符串转换成小写
toLocaleUpperCase()    把字符串转换成大写
toLowerCase()    把字符串转换成小写
toString()    返回字符串
toUpperCase()    将字符串转换成大写
valueOf()    返回对象的原始值


### === 和 == 的区别？（必会）

**区别**：
===：三个等号我们称为等同符，当等号两边的值为相同类型时，直接比较等号两边的值，值相同返回 true，若等号两边的值类型不同时直接返回 false。也就是说三个等号既要判断值也要判断类型是否相等

==：两个等号我们称为等值符，当等号两边的值为相同类型时比较值是否，类型不同时会发生类型的自动转换，转换为相同的类型后再做比较。也就是说两个等号只要值相同就可以。

### null，undefind 的区别？（必会）
**区别**
null表示一个对象被定义了，值为“空值“；
undefined 表示不存在这个值
typeof undefined // "undefined"
undefined：是一个表示”无“的原始值或者说表示”缺少值“，就是此处应该有一个值，但还没有定义。当尝试读取时就返回 undefined；
例如：变量被声明了，但没有赋值时，就等于 undefined
typeof null // "object"
例如：作为函数的参数表示该函数的参数不是对象；
注意：
在验证 null 时，一定要使用 ===，因为 == 无法分别 null 和 undefined，undefined表示”缺少值“，就是此处应该有一个值，但还没有定义。当尝试读取时就返回 undefined；
- 变量被声明，但没有赋值时，就等于 undefined
- 调用函数时，应该提供的参数没有提供，该参数等于undefined
- 对象没有赋值的属性，该属性的值为 undefined
- 函数没有返回值，默认返回undefined

null 表示”没有对象“，即该处不应该有值，典型用法是：
- 作为函数的参数，表示该函数的参数不是对象
- 作为对象原型链的终点



### Javascript 中什么情况会返回 undefined值？（必会）
1、返回声明，但是没有初始化的变量
```javascript
let a;
console.log(aaa); // undefined
```
2、访问不存在的属性
```javascript
let a = {}
console.log(a.c) // undefined
```
3、访问函数的参数没有被显式的传递值
```javascript
(function (b) {
  console.log(b) // undefined
})
```
4、访问任何被设置为 undefined值的变量
```javascript
let a = undefined;
console.log(a) // undefined
```
5、没有定义 return 的函数隐式返回
```javascript
function a() {}
console.log(a()) // undefined
```
6、函数 return 没有显式地返回任何内容
```javascript
function a() {
  return ;
}
console.log(a()) // undefined
```
### 如何区分数组和对象

方法一：通过ES6中的Array.isArray来识别
```javascript
Array.isArray([]) // true
Array.isArray({}) // false
```
方法二：通过 instanceof 来识别
```javascript
[] instanceof Array // true
{} instanceof Array // false
```

方法三：通过调用 constuctor 来识别
```javascript
{}.constructor // 返回 object
[].constructor // 返回 Array
```

方法四：通过 Object.prototype.toString.call 方法来识别
```javascript
Object.prototype.toString.call([]) // ["object Array"]
Object.prototype.toString.call({}) // ["object Object"]
```

### 怎么判断两个对象相等？（必会）

ES6中有一个方法判断两个对象是否相等，这个方法判断是两个对象的引用地址是否一致
```javascript
let obj1 = {}
let obj2 = {}
console.log(Object.is(obj1, obj2)) // false
let obj3 = obj1
console.log(Object.is(obj1, obj3)) // true
console.log(OBject.is(obj2, obj3)) // false
```
当需求时比较两个对象内容是否一致时就没用了
想要比较两个对象内容是否一致，思路是要遍历对象的所有键名和键值是否都一致
1、判断两个对象是否指向同一内存地址
2、使用 Object.getOwnPropertyNames 获取对象所有键名数组
3、判断两个对象的键名数组是否相等
4、遍历键名，判断键值是否都相等
```javascript
let obj1 = {
  a: 1,
  b: {
    c: 2
  }
}

let obj2 = {
  b: {
    c: 3
  },
  a: 1
}

function isObjectValueEqual(a, b) {
  // 判断两个对象是否指向同一内存，指向同一内存返回true
  if (a === b) return true
  // 获取两个对象键值数组
  let aProps = Object.getOwnPropertyNames(a)
  let bProps = Object.getOwnPropertyNames(b)
  // 判断两个对象键值数组长度是否一致，不一致返回false
  if (aProps.length !== bProps.length) return false
  // 遍历对象的键值
  for (let prop in a) {
    // 判断a的键值，在b中是否存在，不存在，返回false
    if (b.hasOwnProperty(prop)) {
      // 判断a的键值是否为对象，是则递归，不是对象直接判断简直是否相等，不相等返回false
      if (typeof a[prop] === 'object') {
        if (!isObjectValueEqual(a[prop], b[prop])) return false
      } else if (a[prop] !== b[prop]) {
        return false
      }
    } else {
      return false
    }
  }
  return true
}
console.log(isObjectValueEqual(obj1, obj2)) // false
```


### JavaScript 中 怎么获取当前日期的月份？（必会）

**方法**
JavaScript 中 获取当前日期是使用 new Date这个内置对象的实例，其他一些进阶的操作也是基于这个内置对象的实例。

获取完整的日期（默认格式）
```javascript
let date = new Date; // Date Wed Jul 14 2021 22:26:56 GMT+0800 (中国标准时间)
```
获取当前年份
```javascript
let year = date.getFullYear(); // 2021
```
获取当前月份
```javascript
let month = date.getMonth() + 1; // 7
```
获取当前星期 X(0-6,0 代表星期天)
```javascript
let month = date.getDay(); // 3
```
获取当前日期（年-月-日）
```javascript
month = (month > 9) ? month: ('0' + month)
day = (day < 10) ? ('0' + day) : day
let today = year + '-' + month + '-' + day
console.log(today) // 2021-7-14
```
另外的一些操作：
getYear()    获取当前年份(2 位)
getFullYear()    获取完整的年份(4 位, 2021)
getMonth()    获取当前月份(0-11,0 代表 1 月)
getDate()    获取当前日(1-31)
GetDay()    获取当前星期 X(0-6,0 代表星期天)
getTime()    获取当前时间(从 1970.1.1 开始的毫秒数)
getHours()    获取当前小时数(0-23)
getMinutes()    获取当前分钟数(0-59)
getSeconds()    获取当前秒数(0-59)
getMilliseconds()    获取当前毫秒数(0-999)
toLocaleDateString()    获取当前日期
toLocaleTimeString()    获取当前时间
toLocaleString()    获取日期与时间


### 什么是类数组（伪数组），如何将其转换为真是的数组？（必会）

**伪数组**
1、具有length属性
2、按索引方式储存数据
3、不具有数组的push和pop等方法

伪数组（类数组）：无法直接调用数组方法或期望 length 属性有什么特殊的行为，不具有数组的push，pop等方法，但仍可以对真正数据遍历方法来遍历它们。典型的是函数document.childnodes 之类的，它们返回的 nodeList 对象都属于伪数组

**伪数组——>真实数组**

1、使用Array.from(eleArr)——ES6
2、[].slice.call(eleArr) 或则 Array.prototype.slice.call(eleArr)
示例：

```javascript
let eleArr = document.querySelector('li')
Array.from(eleArr).forEach(function(item) {
  alert(item)
})

let eleArr = document.querySelector('li')
[].slice.call(eleArr).forEach(function(item) {
  alert(item)
})

```

### 如何遍历对象的属性？（必会）
1、遍历自身可枚举的属性（可枚举，非继承属性） Object.keys() 方法，该方法会返回一个有一个给定对象的自身可枚举属性组成的数组，数组中的属性名的排列顺序和使用 for .. in遍历该对象时返回的顺序一致（两者区别是 for .. in 还会枚举其原型链上的属性）

```javascript
// Array 对象
let arr = ['a', 'b', 'c']
console.log(Object.keys(arr)) // [ "0", "1", "2" ]

// Object 对象
let obj = { foo: 'bar', baz: 42 }
console.log(Object.keys(obj)) // [ "foo", "baz" ]

// 类数组对象 随机key排序
let anObj = { 100: 'a', 2: 'b', 7: 'c' }
console.log(Object.keys(anObj)) // [ "2", "7", "100" ]

// getFoo 是一个不可枚举的属性
let myObj = Object.create({}, {
  getFoo: {
    value: function () {
      return this.foo
    }
  }
})
myObj.foo = 1
console.log(Object.keys(myObj)) // [ "foo" ]
```
2、遍历自身的所有属性（可枚举，不可枚举，非继承属性）Object.getOwnPropertyNames() 方法，该方法返回一个有指定对象的所有自身属性组成的数组（包括不可枚举属性，但不包括Symbol 值作为名称的属性）

```javascript
let arr = ['a', 'b', 'c']
console.log(Object.getOwnPropertyNames(arr).sort()) // [ "0", "1", "2", "length" ]

// 类数组对象
let obj = { 0: 'a', 1: 'b', 2: 'c'}
console.log(Object.getOwnPropertyNames(obj).sort()) // [ "0", "1", "2" ]
// 使用Array.forEach 输出属性名和属性值
Object.getOwnPropertyNames(obj).forEach(function (value, index, array) {
  console.log(value + ' —> ' + obj[value])
})
// 输出
// 0 —> a 
// 1 —> b 
// 2 —> c

// 不可枚举属性
let myObj = Object.create({}, {
  getFoo: {
    value: function () {
      return this.foo
    },
    enumerable: false
  }
})
myObj.foo = 1
console.log(Object.getOwnPropertyNames(myObj).sort()) // [ "foo", "getFoo" ]
````
3、遍历可枚举的自身属性和继承属性（可枚举，可继承的属性）for in 遍历对象的属性
```javascript
let obj = {
  name: '张三',
  age: '24',
  getAge: function () {
    console.log(this.age)
  }
}
let arr = {}
for(let i in obj) {
  if(obj.hasOwnProperty(i) && typeof obj[i] !== 'function') {
    arr[i] = obj[i]
  }
}
console.log(arr) // { name: "张三", age: "24" }
```
注意：hasOwnProperty() 方法判断对象是否有某个属性（本身的属性，不是继承的属性）

4、遍历所有自身属性和继承属性

```javascript
(function () {
  let getAllPropertyNames = function (obj) {
    let props = []
    do {
      props = props.concat(Object.getOwnPropertyNames(obj))
    } while (obj = Object.getPrototypeOf(obj))
    return props
  }
  let propertys = getAllPropertyNames(window)
  alert(propertys.length)
  alert(propertys.join('\n'))
})()
```

### src  和 href 的区别是？（必会）
**区别**
src（source）指向外部资源的位置，指向的内容会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档中，如 Javascript 脚本，img 图和iframe 等元素。
当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，类似于所指向资源嵌入当前标签内。

href（hypertext reference/超文本引用）指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的连接，如果我们在文档中添加`<link href="common.css"rel="stylesheet"/>`那么浏览器会识别该文档为CSS文件，就会并行下载资源并且不会停止对当前文档进行处理

### 如何使用原生 Javascript给一个按钮绑定两个onclick事件？（必会）

```javascript
let btn = document.getElementById('btn')
btn.addEventListener('click', hello1)
btn.addEventListener('click', hello2)
function hello1() {
  alert('hello 1')
}
function hello2() {
  alert('hello 2')
}
````
### 如何在 JavaScript中比较两个对象？（必会）

**比较方法**
使用== 或 === 对两个不同却具有相同属性及属性值的对象进行比较，他们的结果却不会相等。这是因为等号比较的是它们的引用（内存地址），而不是基本类型，为了测试两个对象在结构上是否相等，需要一个辅助函数。它将遍历每个对象的所有属性，然后测试它们是否具有相同的值，嵌套对象也许如此，当然，也可以使用参数来控制是否对原型链进行比较。
注意：此代码只对普通对象、数组、函数、日期和基本类型的数据结构进行对比
```javascript
function isDeepEqual(obj1, obj2, testPrototypes = false) {
  if (obj1 === obj2) return false
  if (typeof obj1 === 'function' && typeof obj2 === 'function') {
    return obj1.toString() === obj2.toString()
  }
  if (obj1 instanceof Date && obj instanceof Date) {
    return obj1.getTime() === obj2.getTime()
  }
  if (Object.prototype.toString.call(obj1)
    !== Object.prototype.toString.call(obj2)
    || typeof obj1 !== 'object') {
    return false
  }

  const protypesAreEqual = testPrototypes ?
    isDeepEqual(
      Object.getPrototypeOf(obj1),
      Object.getPrototypeOf(obj2),
      true
    ) :
    true

  const obj1Props = Object.getOwnPropertyNames(obj1)
  const obj2Props = Object.getOwnPropertyNames(obj2)

  return (
    obj1Props.length === obj2Props.length
    && protypesAreEqual
    && obj1Props.every(prop => isDeepEqual(obj1[prop], obj2[prop]))
  )
}
```

### Javascript中作用域、预解析与变量申明？（必会）
**作用域**
就是变量的有效范围，在一定的空间里可以对数据进行对写操作，这个空间就是数据的作用域
1、全局作用域：最外层函数定义的变量拥有全局作用域，即对任何内部函数来说，都是可以访问的。
2、局部作用域：局部作用域一般只在固定的代码片段内可访问到，而对于函数外部都是无法访问的，最常见的例如函数内部。在ES6之前，只有函数可以划分变量的作用域，所以在函数的外面无法访问函数内的变量
3、块级作用域：凡是代码块就可以划分变量的作用域，这种作用域的规则就叫块级作用域。

块级作用域 函数作用域 词法作用域之间的区别：

块级作用域和函数作用域描述的是，什么东西可以划分变量的作用域
词法作用域描述的是，变量的查找规则

之间的关系：
块级作用域包含函数作用域
词法作用域与块级作用域、函数作用域之间没有任何交集，它们从两个角度描述了作用于的规则

ES6之前的Javascript采用的是函数作用域+词法作用域，ES6 js 采用的是块级作用域+词法作用域

**预解析**
Javascript 代码执行是由浏览器中的 Javascript解析器来执行的。JavaScript解析器执行JavaScript代码的时候，分为两个过程：预解析过程和代码执行过程

预解析过程：
1、把变量的声明提升到当前作用域的最前面，只会提升声明，不会提升赋值
2、把函数的声明提升到当前作用域的最前面，只会提升声明，不会提升调用
3、先提升var，再提升function

Javascript的执行过程：
```javascript
// 案例1
var a = 25
function abc() {
  console.log(a)
  var a = 10
}
abc() // undefined

// 案例2
console.log(a)
function a() {
  console.log('aaaa')
}
var a = 1
console.log(a) // 
```
**变量提升**
变量提升：定义变量的时候，变量的声明会被提升到作用域的最上面，变量的赋值不会提升
函数提升：Javascript解析器首先会把当前作用域的函数声明提前到整个作用域的最前面

```javascript
// 1、----------------------
var num = 10
fun() // undefined
function fun() {
  console.log(num)
  var num = 10
}

// 2、-------------------
var a = 18
fun2() 
function fun2() {
  var b = 9
  console.log(a) // undefined
  console.log(b) // 9
  var a = '123'
}

// 3、-----------------------
fun3()
console.log(c)
console.log(b)
console.log(a)
function fun3() {
  var a = b = c = 9
  console.log(a)
  console.log(b)
  console.log(c)
}
```
变量声明提升：
使用 var 关键字定义的变量，被称为变量声明
函数声明提升的特点是，在函数声明的前面，可以调用这个函数

### 变量提升与函数提升的区别？（必会）
**变量提升**
简单说就是在JavaScript代码执行前引擎会先进行预编译，与编译期间会将变量声明与函数声明提升至对应作用域的最顶端，函数内声明的变量只会提升至该函数作用域最顶层，当函数内部定义的一个变量与外部相同时，那么函数体内的这个变量就会被上升到最顶端。
举例来说：

```javascript
console.log(a) // undefined
// 预编译后的代码结构可以看做如下运行顺序
var a; // 将变量a的声明提升至最顶端，赋值逻辑不提升
console.log(a) // undefined
a = 3 // 代码执行到原位置即执行原赋值逻辑
```

**函数提升**
函数提升只会提升函数声明式写法，函数表达式的写法不存在函数提升
函数提升的优先级大于变量提升的优先级，即函数提升在变量提升之上

### 什么是作用域链？（必会）

**作用域链**
当代码在一个环境中执行时，会创建变量对象的一个作用域链
由子级作用域返回父级作用域中寻找变量，就叫做作用域链
作用域链中的下一个变量对象来自包含环境，也叫外部环境。而下一个变量对象则来自下一个包含环境，一直延续到全局执行环境。全局执行环境的变量对象始终都是作用域链中的最后一个对象。
作用域链前端始终都是当前执行的代码所在变量环境，如果环境是函数，则将其活动对象作为变量对象。

### 如何延长作用域链？（必会）

作用域链是可以延长的，执行环境的类型只有两种，全局和局部（函数）。但是有些语句可以在作用域的前端临时增加一个变量对象，该变量对象会在代码执行后被移除

1、try-catch 语句的 catch 块，会创建一个新的变量对象，包含的是被抛出的错误对象的声明
2、with 语句。with 语句会将指定的对象添加到作用域中

### javascript 变量按照储存方式区分为哪些类型，并描述其特点？（必会）
1、值类型和引用类型
2、值类型储存的是值，赋值之后原变量的值不改变
3、引用类型储存的是地址，赋值之后是把原变量的引用地址赋给新变量，新变量改变原来的会跟着改变

### 如何实现数组的随机排序？（必会）

方法一：
```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
function randSort(arr) {
  for(let i = 0, len = arr.length;i< len; i++) {
    let rand = Math.floor(Math.random()*len)
    let temp = arr[rand]
    arr[rand] = arr[i]
    arr[i] = temp
  }
  return arr
}
console.log(randSort(arr))
```
方法二：
```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
function randSort2(arr) {
  let mixedArray = []
  while(arr.length > 0) {
    let randomIndex = Math.floor(Math.random()*arr.length)
    mixedArray.push(arr[randomIndex])
    arr.splice(randomIndex, 1)
  }
  return mixedArray
}
console.log(randomSort2(arr))
```

方法三：
```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
arr.sort(function() {
  return Math.random() - 0.5
})
console.log(arr)
```

### Function foo() {} 和 let foo = function() {} 之间foo的用法上的区别？（必会）
**区别**
1、`let foo = function() {}`
这种方式是声明了个变量，而这个变量是个方法，变量在Javascript中是可以改变的。

2、`function foo() {}`
这种方式是声明了个方法，foo这个名字无法改变
例如：
```javascript
function b() {
  console.log('a')
}
var a = function() {
  console.log('123')
}
b() // a
a() // 123
```
好像没有什么区别，看下面
```javascript
b() // a
a() // undefined
function b() {
  console.log('a')
}
var a = function() {
  console.log('124')
}
```
是不是有区别了，`function b(){}`为函数声明，程序运行前就已存在
`let a = function(){}`为函数表达式，是变量的声明，属于按顺序执行，所以a为undefined

-----

后面还有几条了解的，看文档即可











