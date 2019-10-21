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
   

### 三、委托的意义

### 四、事件和观察者模式
