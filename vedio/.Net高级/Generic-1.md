# Generic (一)

### ps
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;泛型（generic）是C#语言2.0和通用语言运行时（CLR）的一个新特性,泛型为.NET框架引入了类型参数（type parameters）的概念类型参数使得设计类和方法时，不必确定一个或多个具体参数，其的具体参数可延迟到客户代码中声明、实现。

#### 一、泛型方法的定义(部分代码使用的是c# 7.0的语法)
``` .cs
//带返回值的泛型
public List<T> Method<T>() => new List<T>();
//泛型参数
public string method<T>(T t)=> "";
```
#### 二、泛型接口的定义
``` .cs
//泛型接口
public interface CustomCollection<T>{}
```
#### 三、泛型类的定义
``` .cs
//泛型类
public class CustomClass<T>{}
```

#### 四、泛型委托的定义
``` .cs
//带参数的泛型委托
 public Action<T> act = null;
 //带一个返回值的泛型委托
 public Func<T> fuc = null;
```
#### 五、泛型代码中的 default 关键字
1、T将是值类型还是引用类型。</br>
2、如果T是值类型，那么T将是数值还是结构。</br>
&nbsp;&nbsp;当T为引用类型时 T=null,而T=0对数值类型有效,结构体却不行,可以使用Default关键字兼容3种,它对引用类型返回空，对值类型的数值型返回零,遇到结构体会根据结构成员的类型返回0或者null。

#### 六、泛型的约束（基类约束，接口约束，类型约束，值类型约束，无参构造函数约束）
``` .cs
///定义类型C1
public class C1
{
   public int Id { get; set; }
   public string Name { get; set; }
}

//定义方法,表示泛型参数
//约束1：T必须为C1类型或者继承自C1的子类,就可以实现调用属性
//约束2：至少带有一个无参构造函数
public static void Method<T>(T t) where T : C1,new()
{
      t.Id();
      T t = new T();
}

1、带来权力，可以使用基类里面的属性和方法。</br>
2、带来义务，类型参数必须是基类或者其子类。
3、泛型的几种类型约束是And关系。
```


#### 七、泛型的逆变与协变

```  .cs
public class Test
{
  /// <summary>
  /// 父类A
  /// </summary>
  public class A { }

  /// <summary>
  /// 子类B
  /// </summary>
  public class B:A { }

 A a1 = new A();//可以构造实例
 B b1 = new B();//可以构造实例
 A a2 = new B();//可以构造实例
 
 //集合A可以构造
 List<A> _aList = new List<A>();
 
 //不能创建实例,因为是一个新的类型List<A>类型，List<B>类型
 List<A> _bList = new List<B>();
 
 //利用协变，接口泛型参数加out修饰
 IEnumerable<A> a_list = new List<B>();
 
 //利用逆变，接口泛型参数加in修饰
 IEnumerable<B> a_list = new List<A>();
}

1、List<A> _bList = new List<B>()协变是由父类向子类的转换</br>
2、List<B> _bList = new List<A>()逆变是由子类像父类的转换</br>
3、out 协变：修饰返回值，表示T在类型中只能作为返回值。</br>
4、in 逆变：修饰参数，表示T在类型中只能作为参数。</br>
5、只能放在接口或者委托的泛型参数前面。</br>
```


#### 八、泛型缓存
1、泛型类的静态字段，是独立的,可以实现缓存(放入内存中，利用静态构造函数或者静态字段实现)
2、泛型缓存可以为不同的类型实现缓存
``` .cs
//利用静态构造函数实现缓存
public class GenericCache<T>
{
    static GenericCache()
    {
        _TypeTime = string.Format("{0}_{1}", typeof(T).FullName, DateTime.Now.ToString("yyyyMMddHHmmss.fff"));
    }

    private static string _TypeTime = "";

    public static void GetCache()
    {
        console.writeLine(_TypeTime);
    }
}
//调用会创建5个不同类型，由于静态构造函数后续的调用各个类型分别不会再次构造类型
for (int i = 0; i < 5; i++)
{
    GenericCache<int>.GetCache();
    Thread.Sleep(10);
    GenericCache<long>.GetCache();
    Thread.Sleep(10);
    GenericCache<DateTime>.GetCache();
    Thread.Sleep(10);
    GenericCache<string>.GetCache();
    Thread.Sleep(10);
    GenericCache<GenericCacheTest>.GetCache();
    Thread.Sleep(10);
}
```

#### 九、泛型的优点
1、可实现延迟加载,由.net编译过程流程产生</br>
2、性能安全,消除强制转换导致的装箱拆箱。

#### 十、泛型实现原理及.NetFarmWork编译
1、vs编译器将C#代码编译为exe或者dll(产生IL和metadata)。</br>
2、exe和dll在运行时会经过CLR里的JIT再次编译为机器码被cpu执行。</br>
3、语言升级兼容"<>"，编译器升级识别"<>"将"<>"在IL中编译为占位符,可以通过反编译软件看到第一编译后的IL代码(\`1代表一个参数\`2代表2个参数)。</br>
4、再经过JIT即时编译的时候,根据调用方指定的类型,编译为原始类型。</br>
5、同一个泛型类型，传入不同的参数，经过JIT二次编译会变成不同类型。</br>
6、静态字段/静态构造函数只会初始化一次，常驻内存。

### 用object传参是邪恶的
  为什么说object作为参数是邪恶的,第一由于会发生装箱拆箱的转换，所以在性能上会有一定的损失,第二由于c#本身特点的原因,Obejct是一切类型的基类,那么所有具体类型作为参数出现的地方都可以使用Object来代替,但是他是不安全的，会导致部分代码编译不会报错，执行时会报错。
 
