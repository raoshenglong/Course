#### Container:

1. STL的六大部件：容器(Containers)，分配器(allocators)，算法(Algorithms)，迭代器(Iterators)，适配器(Adapters)，仿函数(Functors)。

2. 序列式容器和关联式容器。

3. 容器是如何扩容的？

   `Array`：不能扩充。

   `Deque`：分段连续，但是使用者感觉是连续的。每次申请一块buffer，map中的指针指向每块buffer，需要扩容的时候，申请一块这种固定大小的buffer。

   `Vector`：找到一块两倍大的空间，把数据搬移过去。

   `List,Forward_list`：空间利用率最好，每次扩充一个。

4. `Stack`和`queue`内部实现上是由`Dqueue`这一数据结构实现的。所以称为容器的adapter。没有iterator，防止破坏这两种结构先进先出、先进后出的特性。

5. `MultiSet`的数据结构是红黑树，关联式容器对于数据的查找，排序是很快的，元素放入的时候会慢一点。Set容器的key与value是一样的。Map容器的Key与Value是不一样的。MultiSet和MultiMap对于重复的key的元素，也是可以放进去，并且计数的。

6. Pair的使用，first和second方法。

7. Unordered_multiset/map是用hash table实现的。Bucket的概念，篮子一定比元素多。当元素大于篮子个数的时候，篮子会扩充一倍个数。

8. Set,Map,Unordered_set/map 不允许key重复。

9. Slist,hash_map,hash_set,hash_multiset,hash_multimap这些是在GNU C中的非标准命名,名字不同，功能是一样的。

10. 分配内存，不建议使用allocator(分配元素个数和释放，需要自己管理)，建议使用malloc和new，释放的时候不要关注分配了多少得空间。

11. 模板主要类模板和函数模板，成员模板。模板的泛化、特化、偏特化。参数个数偏特化与范围偏特化。

12. OOP(Object-Oriented programming)和GP(Generic programming)。OOP企图将datas和methods关联在一起。GP是将datas和methods分开来。采用GP,Containers和Algorithm可以各自做各自的事情，他们之间以Iterator联通即可。

13. List不能使用全局的sort函数，原因是List的迭代器不是连续的，不能跳跃访问。

14. 编译器会对函数模板(function template)进行实参推导(argument deduction)。

15. operate new()会调用malloc。malloc分配的大小，比你需要的会大，包括前后的cookie，对齐部分，debug模式下多的部分。

16. 分配器allocator这个类，最重要的函数是allocate()和deallocate()。如果每个容器里面存放的元素较多，且每个元素的size较小，那么最终分配出来的空间可能比单纯每个元素的空间之和会大很多。GNU C还有另一个alloc特殊的分配器，内部是实现是通过16个链表，尽量减少malloc的次数，以减少cookie的数据，因为容器里面的每个元素的大小通常情况是一致的。后面这个名字改成了pool_alloc。

#### Iterator:

1. 除了Array和Vector之外的容器的迭代器，其他的都是一个类。智能指针。iterator内有很多操作符重载和typedef。
2. 整数i支持++++i操作，但是不支持i++++操作。所以iterator的前加加操作返回的是ref，后加加返回的是值。
3. Iterator需要遵循的原则：必须有能力回答algorithm的提问，必须提供5种associated types。Iterator的属性：category，differenece_type，value_type，reference和pointer。Iterator_category指的Iterator的操作只能++？只能--？能+=3？Iterator Traits用于分辨他所获得的iterator是class iterator还是native poonter，相当于一个过滤器。标准库里面有各种Traits。

#### Vector，Array：

1. 每次扩充的时候会有大量的拷贝操作以及析构。
2. Array没有构造函数和析构函数。
3. deque的迭代器里面有四个元素：cur，first，last，node。在指针移动到每块buffer的头或者尾部的时候，需要根据这几个元素，正确地移动到(上)下一块buffer。deque中的map是一个vector。旧版本允许定义buffer的大小。deque的insert函数会自动判断是将前面还是后面的部分去移动。
4. queue和stack内部调用dequeue的函数。stack和queue不允许遍历，也不提供iterator。可以选择list或dequeue作为底部结构。stack可以选择vector作为底部结构。都不可以用set或者map作为底部结构。

#### RB_Tree:

1. 关联式容器的底层是由两种数据结构支撑的：红黑树和哈希表。红黑树是二元搜寻树，并且保持了适度的平衡，有利于搜寻和插入。使用iterator遍历就可以得到排序状态。不应该使用迭代器改变元素值，但是编程层面并没有阻止。map允许data改变，不允许key改变。

2. 编译器对于size为0的元素，默认会给1个字节，比如仿函数。

3. set/multiSet的iterator是其底部的RB Tree的const iterator，就是为了禁止用户对元素赋值。`typedef typename rep_type::const_iterator iterator`

4. set这个class有三个参数：Key，Compare，Alloc。用户只需要输入第一个。底层的红黑树有5个参数。set也可以认为是容器的适配器。

5. map元素的key是独一无二的，插入必须使用insert_unique()；multimap的key可以重复，插入使用insert_equal()。map不允许修改key，是通过`pair<const Key， T>`中，对第一个部分做了const限制。map独特的[]重载。

6. C++ 11之后hash开头都换成了unordered。hash table的bucket一定大于元素个数。

#### 算法：

1. 算法看不到容器，只能拿到容器的迭代器。算法通过问答的方式，得到容器的一些属性。



