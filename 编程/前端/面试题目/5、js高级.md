### 5、什么是内存泄漏（必会）
### 6、哪些操作会造成内存泄漏？（必会）

[哪些操作会造成内存泄漏？](https://www.jianshu.com/p/a81637b7dead)

### 7、JS 内存泄漏的解决方式（必会）

[JavaScript内存泄露的4种方式及如何避免？](https://blog.csdn.net/qappleh/article/details/80337630)

### 10、常见的 js 中的继承方法有哪些（必会） 

[js继承的多种方式 - 简书 (jianshu.com)](https://www.jianshu.com/p/85899e287694)

### 16、用 JavaScript 实现冒泡排序。数据为 23、45、18、37、 92、13、24 （必会） 

```javascript
var arr = [10, 37, 20, 15, 5, 27]

for(let i = 0; i<arr.length; i++) {
for(let j=0; j<arr.length-i;j++) {
  console.log(j)
  if (arr[j] > arr[j+1]) {
      let temp = arr[j]
      arr[j] = arr[j+1]
      arr[j+1] = temp
    }
}
    break
  }
console.log(arr)

```



### 17、用 js 实现随机选取 10–100 之间的 10 个数字，存入一个数组并排序（必会）

```javascript
//首先创建一个空数组，用来放10个数字
var Array = [];
//接下来定义一个函数，这个函数是写10~100的随机数，我们现在把他封装成一个函数
function getRandom(num1,num2){
    var transition = num2 - num1 + 1;//这里面的加1是为了能够取到100
    var res = Math.floor(Math.random() * transition + num1);
    return res;
}
//上面的代码已经获取了num1~num2的随机数
//下面是遍历出10个随机数,并把十个数用push放法放到新定义的数组里面
for(var i = 0; i < 10; i++){
    Array.push(getRandom(10,100));
}
//最后用sort方法进行排序
Array.sort(function(a,b){
    return a > b;
})
//打印数组Array
console.log(Array);
```

### 29、JavaScript 中如何对一个对象进行深度 clone？

[JavaScript中如何对一个对象进行深度clone_我仅仅是一个Coder-CSDN博客](https://blog.csdn.net/qq8427003/article/details/19983593)

[js 对象深拷贝_javascript 深拷贝和浅拷贝之如何对一个对象进行深度clone_weixin_39974409的博客-CSDN博客](https://blog.csdn.net/weixin_39974409/article/details/110747042)

### 30、js 数组去重，能用几种方法实现（必会） 

[JS数组去重方法_卡卡的博客-CSDN博客_js去重](https://blog.csdn.net/weixin_42322454/article/details/114580787)

