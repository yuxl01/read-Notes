# webapi扩展示例

#### `扩展IHttpControllerSelector`

###### 1、继承默认DefaultHttpControllerSelector
###### 2、重写需要自定义实现的对应方法

``` .cs
public class CustomHttpControllerSelector : DefaultHttpControllerSelector
{
    public CustomHttpControllerSelector(HttpConfiguration configuration) : base(configuration)
    {
    }
    /// <summary>
    /// 获取控制器名
    /// </summary>
    /// <param name="request">请求上下文信息</param>
    /// <returns>控制器名</returns>
    public override string GetControllerName(HttpRequestMessage request) {
        object controllerName;
        IDictionary<string, object> dic = request.GetRouteData().Values;
        dic.TryGetValue("controller", out controllerName);
        return controllerName.ToString();
    }
}

```
###### 3、在AppStart中替换对应重写的服务

``` .cs
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    GlobalConfiguration.Configure(WebApiConfig.Register);
    FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    RouteConfig.RegisterRoutes(RouteTable.Routes);
    BundleConfig.RegisterBundles(BundleTable.Bundles);
    //将默认服务替换为自定义的服务
    {
        HttpConfiguration config = GlobalConfiguration.Configuration;
        config.Services.Replace(typeof(IHttpControllerSelector), new CustomHttpControllerSelector(config));
    }   
}
```


#### `扩展IHttpControllerActivator实现构造控制器`
    
* 同样可以利用其他ioc容器扩展

###### 1、实现IHttpControllerActivator接口

```.cs
public class CustomHttpControllerActivator : IHttpControllerActivator
{
    public IHttpController Create(HttpRequestMessage request, HttpControllerDescriptor controllerDescriptor, Type controllerType)
    {
        IHttpController controller = (IHttpController)Activator.CreateInstance(controllerType, new object[] { new Test() });
        return controller;
    }
}
```
###### 2、AppStstart注册替换重写的接口
``` .cs
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    GlobalConfiguration.Configure(WebApiConfig.Register);
    FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    RouteConfig.RegisterRoutes(RouteTable.Routes);
    BundleConfig.RegisterBundles(BundleTable.Bundles);
    {
        HttpConfiguration config = GlobalConfiguration.Configuration;
        //替换默认DefaultHttpControllerSelector
        config.Services.Replace(typeof(IHttpControllerSelector), new CustomHttpControllerSelector(config));
        //利用CustomHttpControllerActivator实现IHttpControllerActivator
        //替换默认服务DefaultHttpControllerActivator
        config.Services.Replace(typeof(IHttpControllerActivator), new CustomHttpControllerActivator());
    }   
}
```