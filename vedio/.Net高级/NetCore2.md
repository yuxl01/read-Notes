# .NetCore(二)

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
   
  ### 三、利用AutoFac容器替换自带容器
    1、重写ConfigureServices将返回值为IServiceProvider
    2、引入对应容器
  
  ```.cs
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
  
 ```.cs
 //返回值为IServiceProvider
 public IServiceProvider ConfigureServices(IServiceCollection services)
 {
  services.Configure<CookiePolicyOptions>(options =>
  {
      options.CheckConsentNeeded = context => true;
      options.MinimumSameSitePolicy = SameSiteMode.None;
  });
  services.AddSession();
  services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);

  
  //1、实例容器
  var containerBuilder = new ContainerBuilder();
  //注册一个模块已减少注册代码
  //也可以直接注册
  //containerBuilder.RegisterType<TestServiceA>().As<ITestServiceA>().SingleInstance()
  containerBuilder.RegisterModule<CustomAutofacModule>();
  
  //替换完容器，构造控制器需要的参数，是autofac做的，但是控制器本身是ServiceCollection做的
  //将services中的服务填充到Autofac中.
  //containerBuilder.Populate(services)的意思是将所有的创建全部交给Autofac
  containerBuilder.Populate(services);
  IContainer container = containerBuilder.Build();
  return new AutofacServiceProvider(container);
}
 ```
###  四、利用AutoFac实现AOP
`1、基本使用`
 ```.cs
// 1、NuGet安装引用Autofac.Extras.DynamicProxy;
using  Autofac.Extras.DynamicProxy;
//创建类库继承自IInterceptor实现接口
/// 自定义的Autofac的AOP扩展


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
   
