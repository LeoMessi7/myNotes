# diff算法

## diff核心

基于两个假设：

1. 相同的组件有类似的dom结构，不同的组件有不同的dom结构
2. 同一层级的一组节点，他们可以通过唯一的id进行区分

优化策略：

1. web UI节点跨层级移动的操作特别少，可以忽略不计
2. 拥有相同类型的两个组件将会产生相似的树形结构，拥有不同类型的两个组件将会生成不同树形结构
3. 对于同一层级的一组节点，他们可以通过唯一的id进行区分

## react

### 思路

递增法，通过对比新的列表中的节点，在原本的列表中的位置是否是递增，来判断当前节点是否需要移动

**tree diff** 

通过updateDepth分层比较：

对同一父节点下的子节点进行比较

如果是跨组件移动，则删除+添加，而不是移动

**component diff**

同类型进行tree diff

不同类型直接替换

如果类型相同，而虚拟dom可能没有变化，则利用shouldComponentUpdate判断是否需要diff，从而进行优化

**element diff**

1. 老的不存在，则添加

2. 老的存在，新的不存在则删除

3. 位置不一样，则移动

   移动优化：在移动前，会将节点在新集合中的位置和在老集合中lastIndex进行比较，只有在新集合的位置 小于 在老集合中的位置 才进行移动。

### 总结

1. react中尽量减少跨层级的操作。 
2. 可以使用shouldComponentUpdate() 来避免react重复渲染。 
3. 添加唯一key，减少不必要的重渲染

## vue2

递归+双指针

1. 判断是不是同一个元素
2. 不是同一个元素直接替换
3. 是同一个元素，比对属性，比对儿子：四种情况
   - 老的有儿子，新的没有儿子（删除）
   - 新的有儿子，老的没儿子（递归创建）
   - 文本（直接替换）
   - 都有儿子（**双指针**：头头、尾尾、头尾、尾头）对比查找后进行复用

## vue3

最长递增子序列

