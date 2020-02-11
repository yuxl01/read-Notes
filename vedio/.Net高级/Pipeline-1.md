# Pipeline(一)

### 1、Http请求处理流程

##### `思考的问题`
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MVC,WebForm 之类的框架做应用程序的开发之后部署到IIS,用户在浏览器输入url地址之后是怎么到达我们的应用程序？

##### 一、什么是管道模型?
 1、可以理解为是http请求的一个通道抽象出的模型。

##### 二、它的作用是什么?
1、对用户请求Url到达服务器的过程生命周期流程的支撑。

##### 三、Pipeline的请求流程在IIS中如何体现的?
    1、用户发起请求到达DNS,DNS会解析域名找到对应的IP及端口。
    
    2、IIS中HttpSys监听服务接受请求。
    
    3、HttpSys监听服务根据请求类型的后缀,将请求转发到对应的应用程序处理(isapi.dll),IIS中处理程序映射可以实现配置。
    
    4、不同的后缀将到达不同的处理程序.net的请求映射到aspnet.isapi,java或者其他的也可以配置对应的dll。
    
    5、如果MVC请求没有后缀，IIS6会为没有后缀的加一个默认的.axd后缀，IIS7之后就不用了,可以直接映射到HttpRoutingModules。

### 二、HttpApplication事件

![原型图片](https://github.com/yuxl01/read-Notes/blob/master/imag/pipeline-1-1.png)
     
    6、当aspnet.isapi接收到http请求会将转换为HttpWorkRequest对象然后传入HttpRuntime(程序入口)
       完成上下文的初始化为HttpContext.
       
    7、把实例化的上下文HttpContext传入HttpApplicationFactory创建出一个`HttpApplication`的实例.
    
    8、HttpApplication处理和各种请求(权限认证/缓存处理/session处理，每个步骤都需要,但是各不相同)
       利用观察者封装共性部分，留下扩展部分
      
 ###### `HttpApplication`
 
 ```.cs
  public class HttpApplication : IComponent,IDisposable, 
                                 IHttpAsyncHandler, IHttpHandler,
                                 IRequestCompletedNotifier, ISyncContext
{
        //表示处理的开始
        public event EventHandler BeginRequest;
        //验证请求，一般用来取得请求用户的信息
        public event EventHandler AuthenticateRequest;
        //已经获取请求用户的信息
        public event  PostAuthenticateRequest;
        //	授权，一般用来检查用户的请求是否获得权限
        public event  AuthorizeRequest;
        //用户请求已经得到授权
        public event PostAuthorizeRequest;
        //获取以前处理缓存的处理结果，如果以前缓存过，那么不必再进行请求的处理，直接返回缓存结果
        public event ResolveRequestCache;
        //已经完成缓存的获取操作
        public event PostResolveRequestCache;	
        //已经根据用户的请求，创建了处理请求的处理器对象
        public event PostMapRequestHandler;
        //取得请求的状态，一般用于Session
        public event AcquireRequestState;	
        //已经取得了Session
        public event PostAcquireRequestState;
        	//准备执行处理程序
        public event PreRequestHandlerExecute;
        
        //已经执行了处理程序
        public event PostRequestHandlerExecute;	
        //释放请求的状态
        public event ReleaseRequestState;	
        //已经释放了请求的状态
        public event PostReleaseRequestState;
        //更新缓存
        public event UpdateRequestCache;
        //	已经更新了缓存
        public event PostUpdateRequestCache;
        //请求的日志操作
        public event LogRequest;
        //已经完成了请求的日志操作
        public event PostLogRequest;
        本次请求处理完成
        public event EndRequest;	
}
 ```
     1、HttpApplication处理各式各样的请求	
     2、由于处理不尽相同，但是可能都需要，所以使用了事件的模式来扩展。


### 三、HttpModule

     1、HttpModule是HttpApplication一系列事件的扩展。
     2、只要扩展了HttpModule那么所有的上下文请求都会经过,所以比较适合做一些全局的操作。
     3、可以实现"缓存,Url转发"
     
###### 1、创建自定义HttpModule
###### `必须继承自IhttpModule`
    
```.cs
   public class HttpModuleInstance : IHttpModule
    {
        /// <summary>
        /// Init方法仅用于给期望的事件注册方法
        /// </summary>
        /// <param name="httpApplication"></param>
        public void Init(HttpApplication httpApplication)
        {
            //Asp.net处理的第一个事件，表示处理的开始
            httpApplication.BeginRequest += new EventHandler(context_BeginRequest);
            //本次请求处理完成
            httpApplication.EndRequest += new EventHandler(context_EndRequest);
        }

        // 处理BeginRequest 事件的实际代码
        private void context_BeginRequest(object sender, EventArgs e)
        {
            HttpApplication application = (HttpApplication)sender;
            HttpContext context = application.Context;
            string extension = Path.GetExtension(context.Request.Url.AbsoluteUri);
            if (string.IsNullOrWhiteSpace(extension) && !context.Request.Url.AbsolutePath.Contains("Verify"))
            {
                context.Response.Write(string.Format("来自HttpModuleInstance的处理", DateTime.Now.ToString()));
            }
            //处理地址重写
            if (context.Request.Url.AbsolutePath.Equals("/Pipe/Some", StringComparison.OrdinalIgnoreCase))
                context.RewritePath("/Pipe/Handler");
        }

        // 处理EndRequest 事件的实际代码
        private void context_EndRequest(object sender, EventArgs e)
        {
            HttpApplication application = (HttpApplication)sender;
            HttpContext context = application.Context;
            string extension = Path.GetExtension(context.Request.Url.AbsoluteUri);
            if (string.IsNullOrWhiteSpace(extension) && !context.Request.Url.AbsolutePath.Contains("Verify"))
                context.Response.Write(string.Format("HttpModuleInstance请求结束",DateTime.Now.ToString()));
        }

        public void Dispose()
        {

        }
```
###### 2、配置文件配置HttpModule
``` .xml
 <system.webServer>
    <!--VS2013及以后/IIS7.0之后的集成模式-->
    <!--<add name="HttpModuleInstance" type="xllib.Web.Core.HttpModuleInstance,xllib.Web.Core" />-->
    </modules>
    </modules>-->
  </system.webServer>
```
### 四、Global事件

 ##### `暴漏扩展HttpModule`
 `框架约定俗称的事件,类似的有Session实现的HttpModule`
 
 ###### 1、扩展一个HttpModule,然后暴露出一个事件
 ```.cs
 public class GlobalModule : IHttpModule
  {
        public event EventHandler GlobalModuleEvent;
        
        /// <summary>
        /// Init方法仅用于给期望的事件注册方法
        /// </summary>
        /// <param name="httpApplication"></param>
        public void Init(HttpApplication httpApplication)
        {
            //Asp.net处理的第一个事件，表示处理的开始
            httpApplication.BeginRequest += new EventHandler(context_BeginRequest);
        }

        // 处理BeginRequest 事件的实际代码
        void context_BeginRequest(object sender, EventArgs e)
        {
            if (GlobalModuleEvent != null)
                GlobalModuleEvent.Invoke(this, e);
        }
   }  
    
    
 ```
 ###### 2、然后在Global中定义动作或者特殊处理
 ```.cs
  /// <summary>
/// HttpModule注册名称_事件名称
/// 约定的
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
protected void GlobalModule_GlobalModuleEvent(object sender, EventArgs e)
{
    Response.Write("来自Global.asax 的文字GlobalModule_GlobalModuleEvent");
}
 ```
###### Application_Error事件
    可以获取整个网站内的错误，可以做统一错误页面跳转

``` .cs
protected void Application_Error(object sender, EventArgs e)
{
    // 在出现未处理的错误时运行的代码
    var error = Server.GetLastError();
    HttpApplication application = (HttpApplication)sender;
    HttpContext context = application.Context;
    if (context.Request.Url.AbsolutePath.Equals("/Home/Index1", StringComparison.OrdinalIgnoreCase))
    {
        return;
    }
    else
    {
        Server.ClearError();
        context.Response.Write(string.Format("啊哦,出错了", DateTime.Now.ToString()));
    }
   
}
```
    
