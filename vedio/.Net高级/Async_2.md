# Async And Thread (二)

#### 一、线程类(Thread)

##### `1、线程类的定义`
      Thread 是在C# 1.0就已经诞生了,是C#使用多线程封装的类。
``` .cs
//实例化一个多线程
Thread thread = new Thread(new ThreadStart(()=> 
{
    Thread.Sleep(5000);
   //执行的函数
}));
//启动多线程
thread.Start();

 ```
      
      

##### `2、前台线程/后台线程`
`定义:`</br>
  
    前台线程：利用 Thread创建的线程默认是是前台线程，UI线程退出后，他依然会执行完成之后再结束。
    后台线程：利用线程池创建的线程默认是后台线程，UI线程退出了，后台线程就直接退出结束了。
    
`区别:`</br>

    1、后台线程不会阻止进程的中止。属于某个进程的所有前台线程都中止后，进程就会被中止
       所有剩余的后台线程都会停止且不会完成。
    2、可以在任何时候将前台线程修改为后台线程，方式是设置Thread.Isbackground属性。
    3、通过通过BeginInvoke方法运行的线程都是后台线程。
    
##### `3、Thread的常用API`
###### `真的这么强大吗？其实并不是那么的“靠谱”,因为c#的线程想CLR申请的，CLR又是想操作系统申请的，经过了几次转换，所以会有偏差。`
    // 挂起线程，或者如果线程已挂起，则不起作用。
    thread.Suspend(); 
    //继续已挂起的线程。
    thread.Resume();  
    //阻止调用线程，直到由该实例表示的线程终止。
    3、thread.Join();    
    //终止线程。
    4、thread.Abort();
    //设置当前线程为后台线程，默认为前台线程
    thread.IsBackground = true;
    //获取线程执行状态
    thread.ThreadState
##### `4、基于Thread封装的回调`
``` .cs
 /// <summary>
 /// 基于Thread封装支持回调
 /// BeginInvoke的回调
 /// </summary>
 /// <param name="threadStart"></param>
 /// <param name="callback"></param>
 private void ThreadWithCallback(ThreadStart threadStart, Action callback)
 {
     //封装被线程执行的方法，在执行回调
     ThreadStart startNew = new ThreadStart(
         () =>
         {
             threadStart();
             callback.Invoke();
         });
      //给线程执行
     Thread thread = new Thread(startNew);
     thread.Start();
 }
 
 //代返回值的
private Func<T> ThreadWithReturn<T>(Func<T> funcT)
{
    T t = default(T);
    ThreadStart startNew = new ThreadStart(
        () =>
        {
            t = funcT.Invoke();
        });
    Thread thread = new Thread(startNew);
    thread.Start();

    return new Func<T>(() =>
    {
        thread.Join();
        return t;
    });
}
```
#### 二、线程池(ThreadPool)
##### `1、线程池使用`
##### `2、设置线程池`
##### `3、ManualResetEvent`


