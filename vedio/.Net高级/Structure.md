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
     2、只要内部有IEnumerator GetEnumerator() 就可以使用foreach
     
###### `2 :特点和原理`
     1、实现迭代器模式
 ```.cs
 //定义访问器
 public interface IIterator<T>
 {
   /// <summary>
   /// 当前的对象
   /// </summary>
   T Current { get; }
   /// <summary>
   /// 移动到下一个对象，是否存在
   /// </summary>
   /// <returns></returns>
   bool MoveNext();
   /// <summary>
   /// 重置
   /// </summary>
   void Reset();
 }
 //实现迭代器
 public class MenuIterator : IIterator<Food>
 {
     private Food[] _FoodList = null;
     //构造函数获取数据源
     public MenuIterator(KFCMenu kfcMenu)
     {
         this._FoodList = kfcMenu.GetFoods();
     }

     private int _CurrentIndex = -1;
     //返回当前索引处的元素
     public Food Current
     {
         get
         {
             return this._FoodList[_CurrentIndex];
         }
     }
     //如果长度大于当前索引，移动下一个
     public bool MoveNext()
     {
         return this._FoodList.Length > ++this._CurrentIndex;

     }
     //重置索引
     public void Reset()
     {
         this._CurrentIndex = -1;
     }
 }
 
 //实例使用
 public class KFCMenu
 {
     private Food[] _FoodList = new Food[3];

     public KFCMenu()
     {
         this._FoodList[0] = new Food()
         {
             Id = 1,
             Name = "汉堡包",
             Price = 15
         };
         this._FoodList[1] = new Food()
         {
             Id = 2,
             Name = "可乐",
             Price = 10
         };
         this._FoodList[2] = new Food()
         {
             Id = 3,
             Name = "薯条",
             Price = 8
         };
     }

     public Food[] GetFoods()
     {
         return this._FoodList;
     }


     public IIterator<Food> GetEnumerator()
     {
         return new MenuIterator(this);
     }
}

//调用
 KFCMenu kfcMenu = new KFCMenu();
 IIterator<Food> foodIterator = kfcMenu.GetEnumerator();
 while (foodIterator.MoveNext())
 {
     Food food = foodIterator.Current;
 }
 ```
     

### 四、dynamic关键字
