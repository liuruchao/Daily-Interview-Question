## 1. 写 React 项目时为什么要在列表组件中写 key，其作用是什么？

#### my answer:

写 key 为了给组件一个特定标识，更新组件时，diff 算法能够高效的让 react 识别出那些元素发生了更改，在对比新旧虚拟 dom 时，提升更新的效率。这里有几点要注意，1.不同列表中的 key 值可以不重复，也就是 key 值不是全局唯一，而是单个列表中唯一；2.如果使用的列表中的下标作为 key,当这个列表中项目顺序发生变化，会使得 diff 性能变差，如果该列表中是输入组件，则可能会造成 bug
