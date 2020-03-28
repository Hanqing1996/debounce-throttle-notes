## 防抖(debounce)
> 如果下达该命令后，在t毫秒内再次下达该命令，则取消刚刚下达的命令，只执行新命令

> 侧重用户操作

#### 【面试】写一个防抖封装函数
```
function debounce(fn, wait) {
    let timer = null
    return function() {
        if(timer) 
            clearTimeout(timer)
        timer = setTimeout(() => fn.apply(this, arguments), wait)
    }
}

let fn = () => console.log('用户停止滚动一定时间后执行此操作')
fn = debounce(fn, 1000)

document.body.onscroll = fn
```

#### 哪些情况需要用防抖
* 比如我们需要设置滚动条在拖拽期间可显示，停止拖拽后隐藏
```
const onScrollContent= (e) => {
    // 在 scroll 期间要执行的动作：显示滚动条
    setBarVisible(true)
    
    // 清除刚才的命令
    if(timerId){
      window.clearTimeout(timerId)
    }
  
    // 在 scroll 结束后要执行的动作：隐藏滚动条
    timerId=window.setTimeout(()=>{
        // 停止 scroll 三秒后，barVisible 变为 false
        setBarVisible(false)
        window.clearTimeout(timerId)
    },3000)
}
```
* 搜索框提示内容的请求展示
* 输入框在用户输入完毕后再验证输入内容格式
* 给按钮加函数防抖防止表单多次提交。(即sb用户在提交时连续点好几次提交按钮)
```

```

#### 哪些情况不应该用防抖
* 比如我们需要制作一个 sticky 组件：在 scroll 期间某元素距离窗口顶端固定距离。由于动作只发生在 scroll 期间，所以不应该用防抖。
---
## 节流(throttle)
> 对于连续动作，会过滤出部分动作，让这些过滤后的动作之间的执行间隔大于等于t

> 侧重时间间隔
```
let gapTime = 1000 
let lastTime = null
let nowTime = null
let fn = () => console.log('我执行了')
document.body.onscroll = () => {
    nowTime = Date.now() 
    if(!lastTime || nowTime - lastTime > gapTime) {
        fn()
        lastTime = nowTime
    }
}
```

#### 【面试】写一个节流封装函数
```
function throttle(fn, gapTime) {
    let lastTime = null
    let nowTime = null
    return function() {
        nowTime = Date.now()
        if(!lastTime || nowTime - lastTime > gapTime) {
            fn()
            lastTime = nowTime
        }
    }
}

let fn = () => console.log('我执行了')
fn = throttle(fn, 1000)

document.body.onscroll = fn
```

#### 哪些情况需要用节流
* 比如每隔一定时间保存一次用户在 input 中输入的内容。
* 游戏中的刷新率
* DOM元素拖拽
* 验证码已发送，则指定时间间隔内再次点击无效
```
if (this.countdown !== 60) return
else{
    this.$fetch('getCode', {username: this.userName}).then((res: any) => {
        this.timer = setInterval(() => {
            this.countdown--
            if (this.countdown === 0) {
                clearInterval(this.timer)
                this.countdown = 60
            }
        }, 1000)
    })
}
```
