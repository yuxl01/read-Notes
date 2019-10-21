# Delegate And Event(一)



### 一、委托的定义，声明，实例化以及调用
######  `1、我自己比较容易理解的定义`
    1、将方法当做参数传递，但类型是安全的。
    
######  `2、声明、实例化、调用`   
``` .cs
//声明一个带参数无返回值的委托
public delegate void DelegateNoReturn(int param);
//实例化一个委托
DelegateNoReturn dnr = new DelegateNoReturn(t =>
{
    Console.WriteLine(t);
});

//声明一个带参数带返回值的委托
public delegate int DelegateReturnAndParam(int param);
//实例化一个委托
DelegateReturnAndParam drap = new DelegateReturnAndParam(t =>
{
    return 1;
});

//声明一个无参数带返回值的委托
public delegate int DelegateNoParam();
//实例化一个委托
DelegateNoParam dnp = new DelegateNoParam(() =>
{
    return 1;
});

//调用
static void Main(string[] args)
{
    new Program().dnr(5);
    new Program().drap(5);
    new Program().dnp();
}
```
### 二、泛型委托Action,Func
###### `1、什么是泛型委托？`
    A: 最终就是一个基于Delagate和泛型结合的封装而已。
 ![原型图片]( https://github.com/yuxl01/read-Notes/blob/master/imag/Action.png)
   
    1：Action用于没有返回值的方法（参数可以根据自己情况进行传递）
    2：Func恰恰相反用于有返回值的方法（同样参数根据自己情况情况）
   

### 三、委托的本质
###### `1、委托是一个类`
``` .cs
internal delegate void TestDelagate();
```
    1、编译器首先会定义一个完整的类并继承自MulticastDelegate类（所有的委托都派生自MulticastDelegate,
       MulticastDelegate派生自Delegate>Object,历史原因造成由2个委托类）。
    2、类中由4个方法，分别是构造方法，Invoke、BeginInvoke、EndInvoke方法。
    3、由于委托是类，所以凡是能够定义类的地方，都能定义委托。
    
   ###### `2、MulticastDelegate包含3个非公共的字段`
    1、_target为Object类型,当委托包装一个静态方法的时候这个值为null,当委托对象包装实例方法时这个字段引用
        回调方法要操作的对象(回调函数的作用域this)。
    2、_MethodPtr为IntPtr类型，CLR利用它标识要回调的方法。
    3、_invocationList为Object类型通常为null，构造委托链时它引用一个委托数组。
 
 ###### `3、每一个委托都有一个构造器`
    1、获取2个参数，一个对象引用，一个引用了回调方法的整数，对象的引用会被传给Object参数类型的_target。
    2、另外一个回传给IntPtr参数类型的_MethodPtr,对于静态方法会将_target和_invocationList设置为null。
 
 ###### `4、结合上面放一个一目了然的小栗子`
 ``` .cs
public class delegateTest
{
    public delegate void DelegateNoReturn(int param);
    //实例化一个委托传入静态方法
    DelegateNoReturn staticCallMethod = new DelegateNoReturn(delegateTest.staticMethod);
    //实例化一个委托传入实例方法
    DelegateNoReturn instanceCallMethod = new DelegateNoReturn(new delegateTest().instanceCallMethod);

    //静态函数
    public static void staticMethod(int Param){ }
    //实例方法
    public void instanceMethod(int Param) { }

}
 ```
 ![原型图片](https://github.com/yuxl01/read-Notes/blob/master/imag/delegate-1.png)

###### `5、用委托回调多个方法（委托链了解一下）`

### 四、事件和观察者模式
