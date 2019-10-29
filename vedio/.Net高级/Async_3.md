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
