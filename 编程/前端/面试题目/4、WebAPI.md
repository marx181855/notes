!#brushQuestions

### 6、dom 事件的级别？（必会） 

先看这个链接，[Dom事件等级](https://blog.csdn.net/love464524131/article/details/84329970)，再看文档即可

### 18、封装运动函数（必会） 

```javascript
function animate(ele, arrObj, mode = 'uniform-speed', speed = 5, callback = null) {
  for (let arr in arrObj) {
    arrObj[arr] = {
      // 考虑属性为“opacity”的特殊情况
      targetValue: (arr === 'opacity') ? arrObj[arr] * 100 : arrObj[arr],
      nowValue: (arr === 'opacity') ? parseInt(getComputedStyle(ele)[arr]) * 100 : parseInt(getComputedStyle(ele)[arr])
    }
  }
  // 定时器都放入ele的对象中，保证每个元素使用自己的定时器互不干扰
  ele.timer = setInterval(() => {
    // 遍历传入的属性对象，逐个属性进行运动
    for (let arr in arrObj) {
      // 匀速运动下的速度设置
      if (mode === 'uniform-speed') {
        // 判断运动方向，即speed的正负
        speed = (arrObj[arr].targetValue > arrObj[arr].nowValue) ? speed : -speed
      }
      // 缓冲运动下的速度设置
      if (mode === 'buffer') {
        // 根据距离目标值的距离不断调整速度，越靠近目标点速度越慢，且能判断运动方向
        speed = (arrObj[arr].targetValue - arrObj[arr].nowValue) / 10
        // 速度的精确处理
        speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed)
      }
      // 终止条件
      if (Math.abs(arrObj[arr].targetValue - arrObj[arr].nowValue) <= speed) {
        ele.style[arr] = arr === 'opacity' ? arrObj[arr].targetValue / 100 : arrObj[arr].targetValue + 'px'
        // 目标的不一致会让运动执行次数不同，有可能提前关闭定时器，故某条属性运动完成则删除对象里的属性数据
        delete arrObj[arr]
        for (let a in arrObj) {
          return false
        }
        // 直到对象里没有属性，关闭定时器
        clearInterval(ele.timer)
        if (callback) callback()
      } else {
        arrObj[arr].nowValue += speed
        ele.style[arr] = (arr === 'opactiy') ? arrObj[arr].nowValue / 100 : arrObj[arr].nowValue
      }
    }
  }, 30)
}
```

