## 第 1 题：介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

1. set（集合）: 存储不重复的成员、可遍历，接受具有 Iterator 接口的数据为参数
2. WeakSet：只能存储对象为成员，对该对象成员的引用为弱引用，垃圾回收机制不考虑对该对象的引用，如:

```
 const obj = {
     name: 'bob'
 }

 const ws = new WeakSet();
 ws.add(obj)

 obj = null

 // 垃圾回收后
 ws.has(ws)   // false ,引用消失,避免内存泄漏
```

3.  map（字典）: 存储键值对的集合，也就是对象，键名可以是对象的数据类型，可遍历
4.  WeakMap : 存储键值对的集合，键名只能是对象这种数据类型，不可遍历，键名所指向的对象也是若引用，不计入垃圾回收机制
