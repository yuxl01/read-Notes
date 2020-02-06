# Pipeline(一)

### 1、基础概念

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

#### 四、HttpApplication的事件

![原型图片](https://github.com/yuxl01/read-Notes/blob/master/imag/pipeline-1-1.png)
     
    6、当aspnet.isapi接收到http请求会将转换为HttpWorkRequest对象然后传入HttpRuntime(程序入口)
       完成上下文的初始化为HttpContext.
       
    7、把实例化的上下文HttpContext传入HttpApplicationFactory创建出一个`HttpApplication`的实例.
      
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


### 二、Http请求处理流程
### 二、HttpApplication事件
### 三、HttpModule
### 四、Global事件

