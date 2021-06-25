# .NetCore(依赖注入)

### 一、配置文件的读取
  `利用Startup类中的configuration读取appsettings.json中的配置`
  ```.js
  {
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "option1": "value1_from_json",
  "option2": 2,
  "subsection": {
    "suboption1": "subvalue1_from_json"
  },
  "wizards": [
    {
      "Name": "Gandalf",
      "Age": "1000"
    },
    {
      "Name": "Harry",
      "Age": "17"
    }
  ],
  "AllowedHosts": "*"
}
```
  
```.cs
 public IConfiguration Configuration { get; }
 public Startup(IConfiguration configuration)
 {
     Configuration = configuration;
 }
 
 this.Configuration["Option1"]
 this.Configuration["subsection:suboption1"]
 ```
 
 ### 二、自带IOC容器
 
 `1、基本使用`
 ```.cs
  // 1、NuGet安装引用Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.DependencyInjection;
  //2、实例化容器 
  IServiceCollection container = new ServiceCollection();
  //注册服务
  container.AddTransient<Interface,class>();
  //创建一个System.IServiceProvider提供程序
  IServiceProvider provider = container.BuildServiceProvider();
  //创建实例
  _Interface  interface = provider.GetService<_Interface>()
 ```
  `2、服务类型`
  
     1、container.AddTransient为瞬时生命周期,每次创建都是一个全新的实例
  ```.cs
 IServiceCollection container = new ServiceCollection();
 container.AddTransient<Interface,class>();
 IServiceProvider provider = container.BuildServiceProvider();
 _Interface  interface = provider.GetService<_Interface>();
 _Interface  interface2 = provider.GetService<_Interface>();
 bool bResult = object.ReferenceEquals(interface, interface2);
 //b is false 
    
   ```
    2、container.AddSingleton单例：全容器都是一个
```.cs
IServiceCollection container = new ServiceCollection();
container.AddSingleton<Interface,class>();
IServiceProvider provider = container.BuildServiceProvider();
_Interface  interface = provider.GetService<_Interface>();
_Interface  interface2 = provider.GetService<_Interface>();
bool bResult = object.ReferenceEquals(interface, interface2);
 //b is True 
    
   ```
    3、container.AddScoped请求单例，一个请求代表一个作用域
```.cs
IServiceCollection container = new ServiceCollection();
container.AddScoped<Interface,class>();
IServiceProvider provider = container.BuildServiceProvider();

//创建一个Scope作用域,那么由这个域创建的实例是独立的（相较Scope1）
IServiceScope Scope = provider.CreateScope();
//创建一个Scope1作用域,那么由这个域创建的实例是独立的
IServiceScope Scope1 = provider.CreateScope();

_Interface  interface =Scope.ServiceProvider.GetService<_Interface>();
_Interface  interface2 = Scope1.ServiceProvider.GetService<_Interface>();
bool bResult = object.ReferenceEquals(interface, interface2);
 //b is false 
    
   ```
  ### 三、自带IOC容器(IServiceCollection)基础
      
      
   ##### 1.生命周期
   
      //瞬时，即时构造，即时销毁
      services.AddTransient<ITestServiceA, TestServiceA>();
      //单例，永远只构造一次
      services.AddSingleton<ITestServiceB, TestServiceB>();
      //作用域单例，一次请求只构造一个
      services.AddScoped<ITestServiceC, TestServiceC>(); 
      
   ```.cs
   public TestServiceA()
   {
       Console.WriteLine($"{this.GetType().Name}被构造。。。");
   }
   
   public TestServiceB(ITestServiceA iTestServiceA)
   {
       Console.WriteLine($"{this.GetType().Name}被构造。。。");
   }
   
   public TestServiceC(ITestServiceB iTestServiceB)
   {
    Console.WriteLine($"{this.GetType().Name}被构造。。。");
   }
   
   public TestServiceD()
   {
       Console.WriteLine($"{this.GetType().Name}被构造。。。");
   }

  public TestServiceE(ITestServiceC serviceC)
  {
      Console.WriteLine($"{this.GetType().Name}被构造。。。");
  }

  //1.A类构造
  //2.B类构造需要A类
  //3.C类构造需要B类
  //4.D类构造
  //5.E类构造需要C类
  
  //瞬时
  services.AddTransient<ITestServiceA, TestServiceA>();
  //单例
  services.AddSingleton<ITestServiceB, TestServiceB>();
  //作用域单例
  services.AddScoped<ITestServiceC, TestServiceC>();
  //瞬时
  services.AddTransient<ITestServiceD, TestServiceD>();
  //瞬时
  services.AddTransient<ITestServiceE, TestServiceE>();
   ```
   
 `第一次请求解析`
 
      1.开始构造A类，因为是瞬时生命周期那么第一次请求就会被构造，然后销毁
      2.在构造B类时需要A类，那么会首先构造A类(因为A类瞬时上一次已经被销毁)，然后构造B类为单例
      3.开始构造C，在构造时需要B类，因为B类全局单例，那就会直接构造C为作用域单例
      4.直接构造瞬时D类
      5.开始构造E类，构造式需要C类，因为C类为作用域单例，那么就会直接构造E类为瞬时
 
  `第一次请求结论`
 
      1.输出A
      2.输出A，B
      3.输出C
      4.输出D
      5.输出E
      
 `第二次请求解析`
 
      1.开始构造A类，因为是瞬时生命周期那么第二次请求就会重新被构造，然后销毁
      2.在构造B类时因为在第一次请求时已经构造为单例，所以不再被构造
      3.开始重新构造C，因为C在第二次请求为新的作用域,在构造时需要B类，因为B类全局单例，那就会直接构造C为作用域单例
      4.直接构造瞬时D类
      5.开始构造E类，构造式需要C类，因为C类为作用域单例，那么就会直接构造E类为瞬时
      
`第二次请求结论`
     
      1.输出A
      2.输出C
      3.输出D
      4.输出E
      
        
`如果不想通过构造全部自动注入,能自定义注入`
    
      1.using Microsoft.Extensions.DependencyInjection;
      2.首先注入 IServiceProvider serviceProvider，利用serviceProcider.GetService<ITestServiceA>();生成需要的实例
  
    
  ### 四、利用AutoFac容器替换自带容器
    1、下载Autofac.Extensions.DependencyInjection和Autofac包
    2、在Program网站启动时替换默认IOC工厂实例为Autofac，UseServiceProviderFactory(new AutofacServiceProviderFactory())
    3. 在Startup类中添加方法public void ConfigureContainer(ContainerBuilder services){services.RegisterType<TestServiceE>().As<ITestServiceE>().SingleInstance();}
    
  ```.cs
   //返回值为IServiceProvider
   public IServiceProvider ConfigureContainer(ContainerBuilder services)
   {
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
    });
    services.AddSession();
    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
    //services.RegisterType<TestServiceE>().As<ITestServiceE>().SingleInstance();
     services.RegisterModule<CustomMoudle>();

  }
 ```
    
  ```.cs
  //批量注册模块
 public class CustomAutofacModule : Module
 {
      //重新load 添加注册服务
      //可以实现实例控制
      //接口约束
     protected override void Load(ContainerBuilder containerBuilder)
     {
         containerBuilder.Register(c => new CustomAutofacAop());
        
        //设置为属性注入
         containerBuilder.RegisterType<TestServiceA>().As<ITestServiceA>().SingleInstance().PropertiesAutowired();
         
         containerBuilder.RegisterType<TestServiceC>().As<ITestServiceC>();
         containerBuilder.RegisterType<TestServiceB>().As<ITestServiceB>();
         containerBuilder.RegisterType<TestServiceD>().As<ITestServiceD>();

         containerBuilder.RegisterType<A>().As<IA>().EnableInterfaceInterceptors();
     }
 }
  ```
  

###  五、利用AutoFac实现AOP
`1、基本使用`
 ```.cs
// 1、NuGet安装引用Autofac.Extras.DynamicProxy;
using  Autofac.Extras.DynamicProxy;
//创建类库继承自IInterceptor实现接口
/// 自定义的Autofac的AOP扩展

  //注册，模块
 public class CustomAutofacModule : Module
 {
      //重新load 添加注册服务
      //可以实现实例控制
      //接口约束
     protected override void Load(ContainerBuilder containerBuilder)
     {
         //注册AOP
         containerBuilder.Register(c => new CustomAutofacAop());
         //开启aop扩展
         containerBuilder.RegisterType<A>().As<IA>().EnableInterfaceInterceptors();
     }
 }
 
public class CustomAutofacAop : IInterceptor
{
    public void Intercept(IInvocation invocation)
    {
        Console.WriteLine($"invocation.Methond={invocation.Method}");
        Console.WriteLine($"invocation.Arguments={string.Join(",", invocation.Arguments)}");

        invocation.Proceed(); //继续执行

        Console.WriteLine($"方法{invocation.Method}执行完成了");
    }
}

 ```
   
