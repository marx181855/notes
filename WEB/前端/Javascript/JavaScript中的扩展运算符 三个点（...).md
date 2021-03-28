在JavaScript中， ES6开始有rest参数 和 三个点扩展运算符 (spread运算符)
-----------
我们来看看他们各自的用处

# 1. rest参数
rest参数的形式为 ...变量名    用于获取函数调用时传入的参数.  顾名思义, rest参数表示的是除了明确指定的参数外，剩下的参数的集合， 它的类型是Array。

 举个例子如下
 ```javascript
function restFunc(...args)
{
    return args.length;
}

console.log(restFunc('This','is','rest','test')) // 输出4 参数的个数为4
 ```
 另一个例子

 ```javascript

function restFunc(firstArgs,...restArgs) {
    console.log(Array.isArray(restArgs));  
    console.log(firstArgs,restArgs);  
}

restFunc(5,6,7,8,9)
//输出结果
//true
//5,[6,7,8,9]
 ```



# 2. spread运算符 ...

扩展运算符 ...可以用于 数组的构造，也可以用于调用函数时，将一个数组用作函数参数（就是把这个数组转化为参数的列表，所以也就成了一个函数的参数）

## 例子1、在函数调用时使用拓展运算符。
以前如果我们想将数组元素迭代为函数参数使用，一般使用Function.prototype.apply的方式。

```javascript
function myFunction(x, y, z) {
    console.log(x+""+y+""+z);
}

var args = [0, 1, 2];
myFunction.apply(null, args);
```

有了展开语法，我们可以这样写。


```javascript
function myFunction(x, y, z) {
    console.log(x+""+y+""+z);
}

var args = [0, 1, 2];
myFunction(...args);
```
提示: ...arr返回的并不是一个数组，而是各个数组的值。只有[...arr]才是一个数组，所以...arr可以用来对方法进行传值


## 例子2、数组和对象的拷贝。

```javascript
var arr1 = [1,2,3];
var arr2 = [...arr1];
arr2.push(4);

console.log(arr1 === arr2);  // falseconsole.log(arr1); // [1,2,3]console.log(arr2);// [1,2,3,4]
```
对象也是一样，也可以使用拓展运算符

```javascript
var obj1 = {
    a:1,
    b:2
};

var obj2 = {...obj1};
console.log(obj2); //{ a:1, b:2}console.log(obj1 === obj2);// false
```
提示: 在这里你会发现，这是一个深拷贝，其实不然，实际上, 展开语法和 Object.assign() 行为一致, 执行的都是浅拷贝(只遍历一层)。


## 例子3、构造字面量数组。
没有展开语法的时候，只能组合使用 push, splice, concat 等方法，来将已有数组元素变成新数组的一部分。

```javascript
var arr1 = [1,2,3];
var arr2 = [4,5,...arr1];
console.log(arr2);
// [4,5,1,2,3]
```
### 代替Array.concat 函数

```javascript
var arr1 = [1,2,3];
var arr2 = [4,5,6];
var demo = [...arr1,...arr2];
console.log(demo);
//  [1, 2, 3, 4, 5, 6]
````
### 代替Array.unshift 方法

```javascript
var arr1 = [1,2,3];
var arr2 = [4,5,6];
arr1 = [...arr2,...arr1];
console.log(arr1);
//  [4, 5, 6, 1, 2, 3]
```
## 例子4、字符串转数组

```javascript
var demo = "hello"var str = [...demo];
console.log(str);
// ["h", "e", "l", "l", "o"]

```

