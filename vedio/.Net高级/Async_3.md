# task And  Parallel
### 一、任务(Task)
##### `1、task定义`
     Task诞生于FarmWork3.0,使用的是线程池的线程,全部是后台线程,API很强大.
     
##### `任务和线程的区别：`

    1、任务是架构在线程之上的，也就是说任务最终还是要抛给线程去执行。
    2、任务跟线程不是一对一的关系，比如开10个任务并不是说会开10个线程，这一点任务有点类似线程池
       但是任务相比线程池有很小的开销和精确的控制。
       
##### `2、task使用方式`
```.cs
      1、Task.Run(Action act);
      2、taskFactory.StartNew(Action act);
      3、var task = new Task(() =>{}).Start();
```
     
      
##### `3、task的常用API`


### 二、并行(Parallel)

      跟task很像,启动多个线程计算,由于主线程也参与计算等于task+waitall。

```.cs
Parallel.ForEach(new int[] { 0, 1, 2, 3, 4 }, options, (t, state) =>
{
    this.DoSomethingLong($"btnParallel_Click_00{t}");
    //结束全部的
    state.Stop(); 
    //停止当前的
    state.Break();
    return;
});

```
### 三、线程异常处理、线程取消、多线程的临时变量和lock
###### `1、线程异常处理`

      1、多线程中的异常会被吞噬，除非WaitAll让主线程等待
      2、多线程内部不允许异常,必须在内部try catch处理

```.cs
for (int i = 0; i < 20; i++)
{
    string name = string.Format($"btnThreadCore_Click_{i}");
    Action<object> act = t =>
    {
       //内部异常捕获
        try
        {
            Thread.Sleep(2000);
            if (t.ToString().Equals("btnThreadCore_Click_11"))
            {
                throw new Exception(string.Format($"{t} 执行失败"));
            }
            if (t.ToString().Equals("btnThreadCore_Click_12"))
            {
                throw new Exception(string.Format($"{t} 执行失败"));
            }
            Console.WriteLine("{0} 执行成功", t);
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    };
    taskList.Add(taskFactory.StartNew(act, name));
}
```

###### `2、线程取消`

      1、线程取消不是操作线程,而是操作信号量(共享变量:多个能都访问到数据)
      2、每个线程在执行过程中,断续查看信号量，然后自己结束自己
      3、线程不能被别的线程终止,只能自己销毁,存在一定延迟

`CancellationTokenSource`   
      1、根据实例属性IsCancellationRequested判断是否有请求取消
      2、在外部使用实例方法cancel()传达取消的请求
      3、在某些task没启动的情况下,只要已经传达了取消的请求,那么也会取消启动

###### `3、多线程的临时变量的问题`

      1、如果多个线程共享一个变量那就会发生共享
      2、如果在循环内部赋值就是每一个线程对应一个变量对象

```.cs
 //i  只有一个，真实实行的时候，已经是5了，
 //k  多个k，每次是独立的k，跟i没关系
 //int k;
 for (int i = 0; i < 5; i++)
 {
     int k = i;

     new Action(() =>
     {
         Thread.Sleep(100);
         //Console.WriteLine(k);
        // Console.WriteLine(i);
     }).BeginInvoke(null, null);
 }
```

###### `4、线程安全 lock`
    
`> 共有变量：都能访问局部变量/全局变量/数据库的一个值/硬盘文件`

```.cs
private int TotalCount = 0;
for (int i = 0; i < 10000; i++)
{
   ///线程不安全
  taskFactory.StartNew(() =>
    {
        //多个线程同时操作，有时候操作被覆盖了
        this.TotalCount += 1; 
    });
}
```
`> 解决线程不安全`
    
    1、使用线程内的变量
    2、加锁
    3、集合操作使用线程安全集合ConcurrentDictionary

`使用锁需要注意`

    1、lock的方法块儿里面是单线程的；lock里面的代码要尽量的少
    2、解决冲突，从数据上隔离开
    3、lock(this)会锁定当前实例
    4、lock('str')因为享元模式的内存分配,字符串值是唯一的,会锁定其他变量
```.cs
private int TotalCount = 0;
//推荐申明锁的代码
 private static readonly object _lock= new object();
for (int i = 0; i < 10000; i++)
{
   ///线程安全
  taskFactory.StartNew(() =>
    {
        lock(_lock)
        {
            //多个线程同时操作，有时候操作被覆盖了
            this.TotalCount += 1; 
        }
    });
}
```

### 四、await async
