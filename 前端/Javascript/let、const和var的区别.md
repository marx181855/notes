# var与let、const

## 一、var声明的变量会挂载在window上，而let和const声明的变量不会：
```javascript
var a = 100;
console.log(a,window.a); // 100 100

let b = 10;
console.log(b,window.b); // 10 undefined

const c = 1;
console.log(c,window.c); // 1 undefined
```

## 二、var声明变量存在变量提升，let和const不存在变量提升
```javascript
console.log(a); // undefined ===> a已声明还没赋值，默认得到undefined值
var a = 100;

console.log(b); // 报错：ReferenceError: can't access lexical declaration `b' before initialization
let b = 10;

console.log(c); // 报错：ReferenceError: can't access lexical declaration `c' before initialization
const c = 10;

```
## 三、let和const声明形成块作用域
```javascript
if(1){
    var a = 100;
    let b = 10;
}
console.log(a); // 100
console.log(b) // 报错：ReferenceError: b is not defined

if(1){
    var a = 100;
    const c = 1;
}
console.log(a); // 100
console.log(c) // 报错：ReferenceError: c is not defined
```

## 四、同一作用域下let和const不能声明同名变量，而var可以

```javascript
var a = 100;
console.log(a); // 100

var a = 10;
console.log(a); // 10

let a = 100;
let a = 10;
// 控制台报错：SyntaxError: redeclaration of let a note: Previously declared at line 15, column 4
```

## 五、暂存死区

```javascript
var a = 100;

if(1){
    a = 10;
    //在当前块作用域中存在a使用let/const声明的情况下，给a赋值10时，只会在当前作用域找变量a，
    // 而这时，还未到声明时候，所以控制台 ReferenceError: can't access lexical declaration `a' before initialization
    console.log(a)
    let a = 1;
}
```
## 六、const

```javascript
/*
* 　　1、一旦声明必须赋值,不能使用null占位。
*
* 　　2、声明后不能再修改
*
* 　　3、如果声明的是复合类型数据，可以修改其属性
*
* */


const a = 100;


const list = [];
list[0] = 10;
console.log(list);　　// [10]


const obj = {a:100};
obj.name = 'apple';
obj.a = 10000;
console.log(obj);　　// {a:10000,name:'apple'}

```

