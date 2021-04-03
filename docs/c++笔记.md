1. STL的六大部件：容器(Containers)，分配器(allocators)，算法(Algorithms)，迭代器(Iterators)，适配器(Adapters)，仿函数(Functors)。

2.  序列式容器和关联式容器。

3. 容器是如何扩容的？

   `Array`：不能扩充。

   `Deque`：分段连续，但是使用者感觉是连续的。每次申请一块buffer，map中的指针指向每块buffer，需要扩容的时候，申请一块这种固定大小的buffer。

   `Vector`：找到一块两倍大的空间，把数据搬移过去。

   `List,Forward_list`：空间利用率最好，每次扩充一个。

4. `Stack`和`queue`内部实现上是由`Dqueue`这一数据结构实现的。所以称为容器的adapter。没有iterator，防止破坏这两种结构先进先出、先进后出的特性。

5. `MultiSet`的数据结构是红黑树，关联式容器对于数据的查找，排序是很快的，元素放入的时候会慢一点。Set容器的key与value是一样的。Map容器的Key与Value是不一样的。MultiSet和MultiMap对于重复的key的元素，也是可以放进去，并且计数的。

6. Pair的使用，first和second方法。

7. Unordered_multiset/map是用hash table实现的。Bucket的概念，篮子一定比元素多。当元素大于篮子个数的时候，篮子会扩充一倍个数。

8.  Set,Map,Unordered_set/map 不允许key重复。

9. Slist,hash_map,hash_set,hash_multiset,hash_multimap这些是在GNU C中的非标准命名,名字不同，功能是一样的。

10.  分配内存，不建议使用allocator(分配元素个数和释放，需要自己管理)，建议使用malloc和new，释放的时候不要关注分配了多少得空间。

11. 模板主要类模板和函数模板。

12. OOP(Object-Oriented programming)和GP(Generic programming)。OOP企图将datas和methods关联在一起。GP是将datas和methods分开来。采用GP,Containers和Algorithm可以各自做各自的事情，他们之间以Iterator联通即可。

13. List不能使用全局的sort函数，原因是List的迭代器不是连续的，不能跳跃访问。

    