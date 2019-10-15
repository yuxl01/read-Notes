#  Async And Delagate (一)
#### 一、进程、线程、多线程 
###### `进程和线程是计算机中的概念。`</br>
  
    1、进程是一个程序在电脑上运行时，全部计算资源的合集叫进程。
    2、线程是程序的最小执行单位，包含计算资源,任何一个操作的响应都是线程完成的。
    3、多线程代表多个线程并发执行。
    4、Thread是.Net框架封装的一个类，描述线程这个东西。
    
#### 二、同步和异步委托启动异步调用。

    1、同步方法：发起一个调用，一定要等着计算结束才运行下一行。
    2、异步方法: 发起一个调用，并不等着计算结束，而是直接去运行下一行,刚才的计算，会启动一个新的线程去执行。
    3、多线程：在C#中多线程说的是CLR线程，由cpu调度。
    4、异步：异步IO线程，DMA(直接内存存储)
    5、多线程和异步的调试比较困难，因为执行顺序有clr完成，所以多半使用控制台或者日志监控。
    
``` .cs

Action<string> act =t=> Console.WriteLine(t);

//委托的同步调用
act.Invoke("测试委托同步调用");
//委托的异步调用，BeginInvoke会开启一个线程执行
act.BeginInvoke("测试委托异步调用",null,null);
```
    
#### 三、多线程的特点
###### `任何通过启动顺序或者时间等待来控制流程都是纸老虎，因为不可能成功0.0`
    1、同步方法卡界面，因为UI线程忙于计算，异步多线程方法不卡界面，主线程闲置，计算任务交给子线程在做。
    2、同步方法慢，只有一个线程计算；异步多线程方法快，多个线程并发计算。
    3、多线程的资源消耗更多，线程并不是越多越好(资源有限/管理线程也消耗资源)
    4、异步多线程是无序的，启动无序，执行时间不确定，结束无序
    
#### 四、异步的回调和状态参数以及异步等待
    常见的委托异步等待方式：
    
    1、callback and AsyncState
    2、监控状态执行等待
    3、EndInvoke
    
###### `1、callback and AsyncState `
    1.1：永远都是异步之后执行回调，因为委托异步执行会开启一个线程，等线程执行完委托之后,该线程会再去执行回调
    说明执行委托和回调的从开始到结束是由一个线程完成
      
```.cs
Action<string> act =t=> Console.WriteLine(t);

//声明一个回调
AsyncCallback acbak = ab =>
{
    //异步操作信息
    string str = ab.AsyncState.ToString();
    Console.WriteLine(str);
};

//异步调用完成之后执行回调
act.BeginInvoke("测试委托异步调用", acbak, "回调参数");

```
###### `2、IsCompleted and IAsyncResult.AsyncWaitHandle.WaitOne()` 
       
    2.1：IsCompleted状态参数，利用主线程等待，会卡界面，可以输出一些内容边等待边做提示，有误差。
      
      
``` .cs
 //异步执行委托
IAsyncResult iAsyncResult = act.BeginInvoke("测试委托异步调用", null, null);
//监控是否执行完成的状态
while (!iAsyncResult.IsCompleted)
{
    Thread.Sleep(200);
}
```
    2.2：WaitOne没有误差，等待任务完成，第一时间执行后续程序，但是不能做额外的操作，只是单纯等待异步完成。
```.cs
//一直等待任务完成，第一时间进入下一行
iAsyncResult.AsyncWaitHandle.WaitOne();
iAsyncResult.AsyncWaitHandle.WaitOne(-1);

//最多等待3s，否则就进入下一行，可以做一些超时控制
iAsyncResult.AsyncWaitHandle.WaitOne(3000);
  
```
###### `3、EndInvoke` 
    3.1：可以实现等待，和获取返回值,但对于每个异步操作，只能调用一次 EndInvoke,调用EndInvoke会主动回收释放资源。
 ``` .cs
 //声明一个传入int参数返回string的委托
 Func<int, string> func = i => i.ToString();
 
 func.BeginInvoke(DateTime.Now.Year,new AsyncCallback(ar=> 
 {
    //在回调中获取使用异步的返回结果
    string result = func.EndInvoke(ar);
    Console.WriteLine($"{ar.AsyncState} 的异步调用结果 {result}");
 }),"异步回调的参数");
 
 
 ```


