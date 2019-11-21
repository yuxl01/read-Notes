# .NetCore

### .NetFrameWork

###### `一、.Net Framework包含三部分：`
  
     1、CLR （适配计算机，如果需要Linux需要借助MONO）
     2、编程工具
     3、BCL(基类库)
     
###### `二、各版本的对比`
  
     [C#版本][.NET Framework版本]	  [CLR版本]	      [VisualStudio版本]
     C#1.0	  .NET Framework 1.0	    CLR1.0	      Visual Studio 2002
     C#1.2	  .NET Framework 1.1	    CLR1.1	      Visual Studio 2003
     C#2.0	  .NET Framework 2.0	    CLR2.0	      Visual Studio 2005
     
          	  .NET Framework 2.0	    CLR2.0	      Visual Studio 2008
     C#3.0    .NET Framework 3.0	    CLR2.0            Visual Studio 2010
              .NET Framework 3.5	    CLR2.0	
            
     C#4.0	  .NET Framework 4.0	    CLR4.0	      Visual Studio 2010
     
     C#5.0	  .NET Framework 4.5	    CLR4.0	      Visual Studio 2012
                                                          Visual Studio 2013
                                              
     C#6.0	  .NET Framework 4.6	    CLR4.0	      Visual Studio 2015
     C#7.0			                              Visual Studio 2017
     
###### `三、包含的框架`
     
     1、wpf
     1、windowsform
     1、asp.net
     
###### `四、各c#各版本的新语法`

. C# 6.0
```.cs
  //1、自动属性
  public string Name { get; set; } = "summit";
  public IList<int> AgeList{get;set;} = new List<int> { 10, 20, 30, 40, 50 };
  
  //2、导入静态类(Using Static)
  using static System.Math;
  Console.WriteLine($"之前的使用方式: {Math.Pow(4, 2)}");
  Console.WriteLine($"导入后可直接使用方法: {Pow(4, 2)}");
  
  //3、空值运算符(Null-conditional operators)
  int? iValue = 10;
  Console.WriteLine(iValue?.ToString());//不需要判断是否为空
  string name = null;
  Console.WriteLine(name?.ToString());
  
  //4、对象初始化器(Index Initializers),以前必须add加入字典
  IDictionary<int, string> dictOld = new Dictionary<int, string>()
  {
       { 1,"first"},
       { 2,"second"}
  };
   
  //5、异常过滤器(Exception filters)
   int exceptionValue = 10;
   try
   {
       Int32.Parse("s");
   }
   catch (Exception e) when (exceptionValue > 1)//满足条件才进入catch
   {
       Console.WriteLine("catch");
       //return;
   }
   
   //5、nameof表达式
   Console.WriteLine(nameof(obj)); //获取obj这个字符串
```

. C# 7.0
```.CS
 //1、具有模式的 IS 表达式
  public void PrintStars(object o)
  {
      if (o is null) return;     // 常量模式 "null"
      if (!(o is int i)) return; // 类型模式 定义了一个变量 "int i"
      Console.WriteLine(new string('*', i));//可以直接访问
  }
  this.PrintStars(null);
  
 //2、可以设定任何类型的 Switch 语句（不只是原始类型）模式可以用在 case 语句中 Case 语句可以有特殊的条件
 private void Switch(string text)
 {
     int k = 100;
     switch (text)
     {
         case "1" when k > 10:
             Console.WriteLine("1");
             break;
         case "2" when text.Length < 10:
             Console.WriteLine("2");
             break;
         case string s when s.Length > 7://模式
             Console.WriteLine(s);
             break;
         default:
             Console.WriteLine("default");
             break;
         case null:
             Console.WriteLine("null");
             break;
     }
 }
 
 //3、元组
 private (string, string, string) LookupName(long id) // tuple return type
 {
     return ("first", "middle", "last");
 }
 var result = this.LookupName(1);
 Console.WriteLine(result.Item1);
 
 //4、局部函数
  Add(3);
  int Add(int k)//闭合范围内的参数和局部变量在局部函数的内部是可用的，就如同它们在 lambda 表达式中一样。
  {
      return 3 + k;
  }
  
 //5、数字分隔号，方便数每隔三位
 long big = 100_000;
```
### .NetCore
  `为了跨平台，由于framwork向前兼容过于累赘，跨平台成本较高，Core是一套全新的CLR，同时支持windows以及linux`
  
###### 一、Asp.netCore 和Asp.Net
   ##### `KestrelServer 跨平台的服务器，iis只做反向代理`
    1、Asp.Net网站托管在IIS--IIS负责监听-转发请求--响应客户端,不负责请求的监听-转发-响应(IIS)
       封装了管道处理模型，只写业务处理逻辑
    2、Asp.NetCore是控制台利用CreateWebHostBuilder(内置KestrelServer)-启动了服务器--负责监听-转发请求-响应客户端请求
       一系列动作自己完成的，包括管道模型也是自定义
    3、.NetCore的性能相比farmwork快很多，因为东西少
   
###### 二、部署iis
    1、需要安装对应程序
    2、将程序池改为无托管
  
  
