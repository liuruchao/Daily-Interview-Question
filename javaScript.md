## 第 1 题：['1', '2', '3'].map(parseInt) what & why ?

#### result: [1,NaN,NaN,NaN]

#### my answer:

1.  map 函数是 js 的迭代方法，执行后会返回一个新数组，它接受一个回调函数为参数，该回调函数接受三个参数,分别是调用数组的项、index、原数组，具体为
    var new_arr = arr.map(function(item,index,arr) {//return })
2.  parseInt 接受两个参数，第一个是要转化成整数的字符串，如果该参数不是字符串，则先转成字符串，第二个参数是要转成数值的进制数，范围是 2~36。在基数为 undefined，或者基数为 0 或者没有指定的情况下，该函数会根据实际传入字符串来使用对应进制数.在未传入第二个参数时，默认以十进制解析，但如果 0 开头，则按照 8 进制数解析；如果 0x 开头，则按照 16 进制数解析。因此使用该函数时，应明确给出 radix 参数的值
3.  该题实际执行为:

```
 parseInt('1',0)  result 1
 parseInt('2',1)  result NaN ,没有1进制
 parseInt('3',2)  result NaN ,2进制中没有3这个数值，无法解析，返回NaN
 parseInt('12894',8) result 10, 8进制中没有8这个数值，当解析到8后，会将8以及后面的字符抛弃掉，所以可以相当于parseInt('12',8)
```

## 第 2 题：什么是防抖和节流？有什么区别？如何实现？

1. 防抖：任务频繁触发的条件下（如：scroll,resize,人为的连续点击事件），只有在任务触发的时间间隔超出指定间隔时，任务才会触发，为了节省资源，减少不必要损耗
   应用场景： 输入框中模糊搜索，输入值后请求接口返回对应的搜索结果

```
// 防抖
    function debounce(func, wait) {
      var timeout;

      return function() {
        var context = this;
        var args = arguments;

        clearTimeout(timeout);
        timeout = setTimeout(function() {
          func.apply(context, args);
        }, wait);
      };
    }
```

2. 节流：任务频繁触发的条件下，在指定的时间间隔内，只执行以此任务
   应用场景： 图片懒加载，再向下滑动时，超出首屏时，在间隔时间内，请求图片资源

```
// 节流
    function throttle(func, wait) {
      var timeout;
      return function() {
        var context = this;
        var args = arguments;
        if (!timeout) {
          timeout = setTimeout(function() {
            func.apply(context, args);
            timeout = null;
          }, wait);
        }
      };
    }
```

ps: 无论是防抖和节流，本质上作用都是为了节约计算机资源，提高程序的性能，使用的是闭包和定时器

## 第 3 题：setTimeout、Promise、Async/Await 的区别

关键字： 主线程、调用栈、执行上下文、同步、异步、宏观、微观、事件循环、消息队列

1. 主线程： js 是单线程执行的语言，它的代码只能在同一个线程中执行
2. 调用栈： 需要执行的 js 代码会放到一个栈中执行，这个栈是后进先出的数据结构，当执行一个函数时，先将这个函数推入到栈中，执行完成后再推出，接着执行下一个函数或代码
3. 执行上下文：当执行一个 js 时,调用栈中首先会保存一个主函数的全局上下文，后续推入的函数，就可以通过这上下文来访问设置的一些全局属性，所以上下文本质上是提供一个供其它子函数或代码访问其变量的环境
4. 同步：js 顺序执行代码的过程就是同步
5. 异步: 因为 js 是单线程，当遇到一些比较耗时且阻塞的操作时（网络请求，I/O）,为了不影响后续代码的执行，会将这些代码做一个保存，以便在未来某个时刻再运行，这个过程叫异步
6. 宏观：异步执行产生的任务又可以分为宏观任务和微观任务，宏观任务是由宿主环境主动发起的：如 script 标签中的代码，setTimeout、setInterval、DOM 事件、I/O 等
7. 微观任务：js 引擎发起的任务叫微观任务，es6 中的 promise async/await，微观任务总是先于宏观任务执行
8. 事件循环：js 引擎会循环往复的来查看调用栈中是否为空，如果为空，则会从消息队列中依照先后顺序读取任务放到调用栈中执行
9. 消息队列：异步产生的宏观任务会依次存放到一个队列中，这个队列就是消息队列

####1. setTimeout

```
console.log('script start')
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序：script start->script end->settimeout
```

1.js 顺序执行代码，先输出'script start',遇到 setTimeout 产生异步代码，回调函数会在倒计时结束后，将回调函数添加到消息队列中， 2.输出‘script end’, 3.当前调用栈中代码全部执行完成，js 引擎查询消息队列是否有回调函数,发现有一个，然后取出执行，输出‘settimeout’

####2. Promise

```
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
```

1. 先输出 script start,
2. 遇到 promise 先执行里面代码，resolve,reject 不会阻塞后续代码执行，输出 promise1 promise1 end,遇到 resolve 或 reject,会产生一个微观任务并添加到微观任务队列中，
3. 遇到 setTimeout,倒计时结束后，添加宏观任务到消息队列，
4. 输出 script end
5. 调用栈中执行结束，事件循环先查看微观任务队列，并依次取任务到调用栈中执行，直到微观任务队列为空时，再去宏观任务队列中取宏观任务到调用栈中执行，循环这个过程，直到再无任务（微观任务先与宏观任务执行）
6. 输出 promise2,settimeout

#### 3.async/await

```
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}

console.log('script start');
async1();
console.log('script end')

// 输出顺序：script start->async1 start->async2->script end->async1 end

```

1. async 和 await 都会返回一个 promise 对象
2. await 后的代码 会等到 await 产生的微观任务执行结束后，才会去执行，可以看成 await 后的代码被装进了这个 await 产生的 promise 的回调函数中
