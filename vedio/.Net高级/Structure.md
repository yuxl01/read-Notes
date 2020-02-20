# 数据结构(Structure)

### 一、常用数据结构

###### `1 :Array、ArrayList`
     1、Array分配在连续内存,不能随意扩展,插入数据很慢。
     2、Array性能高，索引查找快，数据再多性能没有影响。
     3、ArrayList可变长度的,不限制类型,所以可能不安全,放了不同类型，可能用错。
     4、ArrayList一切元素都是object,值类型会装箱。
     
###### `2 :List、LinkedList`
     1、List也是数组,泛型,变长的,没有装箱拆箱类型安全。
     2、LinkedList添加删除效率高,查询慢，不支持索引因为不在一块儿内存，要从头找起。
     
###### `3 :Queue/Stack（最伟大的也是最容易理解的结构）`
     1、Queue先进先出。
     2、Stack先进后出。
     
###### `4 :Hashtable/Dictionary不是线性结构为散列`
     1、Hashtable数据不能太多
     2、快速的增删改查：空间换时间的
     3、线程安全的集合

     1、Dictionary泛型的hashtable
     2、支持索引访问
     


### 二、迭代器模式，yield

###### `1 :实现方式`
     1、必须实现IEnumerable接口
     2、只要内部有IEnumerator GetEnumerator()
###### `2 :特点和原理`

### 四、dynamic关键字
