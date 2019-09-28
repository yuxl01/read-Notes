# Generic (-)

### ps
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;泛型（generic）是C#语言2.0和通用语言运行时（CLR）的一个新特性,泛型为.NET框架引入了类型参数（type parameters）的概念类型参数使得设计类和方法时，不必确定一个或多个具体参数，其的具体参数可延迟到客户代码中声明、实现。

#### 一、泛型方法的定义
``` .cs
//带返回值的泛型
public List<T> Method<T>()
{
    return new List<T>(); 
}
//泛型参数
public string method<T>(T t)
{
    return "";
}
```
#### 二、泛型接口的定义
``` .cs
//泛型接口
public interface CustomCollection<T>
{
    
}
```
#### 三、泛型类的定义
``` .cs
//泛型类
public class CustomClass<T>
{
    
}
```

#### 四、泛型委托的定义
``` .cs
//带参数的泛型委托
 public Action<T> act = null;
 //带一个返回值的泛型委托
 public Func<T> fuc = null;
```

#### 五、泛型的优点

#### 六、泛型实现原理及.NetFarmWork编译
1、vs编译器将C#代码编译为exe或者dll(产生IL和metadata)。</br>
2、exe和dll在运行时会经过CLR里的JIT再次编译为机器码被cpu执行。</br>
3、语言升级兼容"<>"，编译器升级识别"<>"将"<>"在IL中编译为占位符,可以通过反编译软件看到第一编译后的IL代码。</br>
4、再经过JIT即时编译的时候,根据调用方指定的类型,编译为原始类型。</br>
