## 第 1 题：['1', '2', '3'].map(parseInt) what & why ?

#### result: [1,NaN,NaN,NaN]

#### my answer:

1.  map 函数是 js 的迭代方法，执行后会返回一个新数组，它接受一个回调函数为参数，该回调函数接受三个参数,分别是调用数组的项、index、原数组，具体为
    var new_arr = arr.map(function(item,index,arr) {//return })
2.  parseInt 接受两个参数，第一个是要转化成整数的字符串，如果该参数不是字符串，则调用 toString 先转成字符串，第二个参数是要转成数值的进制数，范围是 2~36。在基数为 undefined，或者基数为 0 或者没有指定的情况下，该函数会根据实际传入字符串来使用对应进制数，因此使用该函数时，应明确给出 radix 参数的值
3.  该题实际执行为:

```
 parseInt('1',0)  result 1
 parseInt('2',1)  result NaN ,没有1进制
 parseInt('3',2)  result NaN ,2进制中没有3这个数值，无法解析，返回NaN
```
