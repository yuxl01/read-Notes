# MVC 

### 一、基础知识

###### 1.Application_Start

    1. 网站启动时，响应第一次请求的执行的
    2. 合适做一些初始化
    
###### 2.RouteConfig路由匹配从上往下
        
    1.routes.MapRoute：注册路由
    2.routes.IgnoreRoute：忽略路由
    
    

### 一、 数据传值的多种方式

|ViewData|ViewBag|TempData|model|
|-|-|-|-|
|定义在ControllerBase中的一个ViewDataDictionary类型的字典|定义在ControllerBase中的一个dynamic类型的属性|定义在ControllerBase中的一个TempDataDictionary类型的字典,基于session|框架View提供|
###### 1.ViewData 
    
        是定义在ControllerBase中的一个ViewDataDictionary类型的字典,在传值的时候键对应的值可以是对象也可以是值
    在cshtml中调用必须类型转换,因为是Object类型的属性,ViewBag和ViewData数据相通,在cshtml中ViewData可以取ViewBag中的值。
    
```.cs
 base.ViewData["User1"] = new User()
 {
     Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
 }; 
 
 base.ViewData["doSomting"] = "sw";
```
    
###### 2.ViewBag

     是定义在ControllerBase中的一个dynamic类型的属性,在传值的时候可以是任意类型,并且编译器将不检查
    ViewBag和ViewData数据相通,在cshtml中ViewBag可以取ViewData中的值。
    
 ```.cs
  base.ViewBag.Name = "admin";
  base.ViewBag.User = new User()
  {
      Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
  };         
```   
###### 3.TempData 

     是定义在ControllerBase中的一个TempDataDictionary类型的字典,基于session可以跨控制器action(存在一次)
     
 ```.cs
 base.TempData["User"] = new User()
 {
     Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
 };
``` 
###### 4.model
    1、后台返回view的时候传入一个对象
    2、在cshtml中引用这个对象@model namespace.Models.User
```.cs
//后台返回
return View(new User()
{
     Id = 78acba-saqwq21-sas-21sas2ffl,
     Name = "sa";
});

//前台调用
@model namespace.Models.User
string name = base.Model.Name
```
### 三、razor语法 html扩展控件

###### 1.razor常用语法
    
|单行操作|行内操作|多行操作|
|-|-|-|
|@{ string name = "sa"; string _names = "admin";}|@DateTime.Now.ToString("yyyy-MM-dd")| @{var age = 25;Response.Write("Multi-Line : Age is " + age + "<br />");string name3 = "yoyo";}|

###### 2.分布视图页

  `partialView用于通用局部使用的地方`
```.cshtml
//直接使用前端页面返回的是字符串，放入当前位置
1.@Html.Partial("PartialTest");
//在指定位置添加一个view，返回void 需要放入大括号 
2.@{Html.RenderPartial("PartialTest");}
//返回的是字符串，放入当前位置,需要经过控制器action的处理
3.@Html.Action("Render", "Second", new { name = "Html.Action" })
//在指定位置添加一个view,返回void 需要放入大括号,需要经过控制器action的处理
4.@{Html.RenderAction("Render", "Second", new { name = "Html.RenderAction" });}
```
  `Layout母版页`
  
  
 ###### 3.html扩展控件
 ```.cs
 /// <summary>
/// 自定义一个@html.Submit()
/// </summary>
/// <param name="helper"></param>
/// <param name="value">value属性</param>
/// <param name="defaultClass">预设的class</param>
/// <returns></returns>
public static MvcHtmlString Submit(this HtmlHelper helper, string value, string defaultClass = "btn btn-default")
{
    var builder = new TagBuilder("input");
    builder.MergeAttribute("type", "submit");
    builder.MergeAttribute("value", value);
    builder.MergeAttribute("class", defaultClass);
    builder.ToString(TagRenderMode.EndTag);
    return MvcHtmlString.Create(builder.ToString());
}

 ```
 
 ### 四、ioc和mvc的结合
 
###### 1.利用Unity容器实现控制器注入

##### `Unity初始化一`
```.cs
//1.nuget下载Unity.dll
//2.实现初始化
private static void InitIOCContainer()
{
    ExeConfigurationFileMap fileMap = new ExeConfigurationFileMap();
    fileMap.ExeConfigFilename = Path.Combine(AppDomain.CurrentDomain.BaseDirectory + "CfgFiles\\Unity.Config");//找配置文件的路径
    Configuration configuration = ConfigurationManager.OpenMappedExeConfiguration(fileMap, ConfigurationUserLevel.None);
    UnityConfigurationSection section = (UnityConfigurationSection)configuration.GetSection(UnityConfigurationSection.SectionName);

    IUnityContainer container = new UnityContainer();
    section.Configure(container, "ruanmouContainer");
    IOCContainer = container;
}
public static IUnityContainer IOCContainer
{
    private set;
    get;
}

static IOCConfig()
{
    InitIOCContainer();
}
```
##### `Unity初始化(加入缓存)`
```.cs
   
public class DIFactory
{
    private static object _SyncHelper = new object();
    private static Dictionary<string, IUnityContainer> _UnityContainerDictionary = new Dictionary<string, IUnityContainer>();

    /// <summary>
    /// 根据containerName获取指定的container
    /// </summary>
    /// <param name="containerName">配置的containerName，默认为defaultContainer</param>
    /// <returns></returns>
public static IUnityContainer GetContainer(string containerName = "ruanmouContainer")
{
   if (!_UnityContainerDictionary.ContainsKey(containerName))
   {
       lock (_SyncHelper)
       {
           if (!_UnityContainerDictionary.ContainsKey(containerName))
           {
               //配置UnityContainer
               IUnityContainer container = new UnityContainer();
               ExeConfigurationFileMap fileMap = new ExeConfigurationFileMap();
               fileMap.ExeConfigFilename = Path.Combine(AppDomain.CurrentDomain.BaseDirectory + "CfgFiles\\Unity.Config.xml");
               Configuration configuration = ConfigurationManager.OpenMappedExeConfiguration(fileMap, ConfigurationUserLevel.None);
               UnityConfigurationSection configSection =(UnityConfigurationSection)configuration.GetSection(UnityConfigurationSection.SectionName);
               configSection.Configure(container, containerName);
               _UnityContainerDictionary.Add(containerName, container);
            }
            return _UnityContainerDictionary[containerName];
        }
   }
  
 }
```
##### `替换默认控制器工厂`

```.cs
public class UnityControllerFactoryNew : DefaultControllerFactory
{
    private IUnityContainer UnityContainer
    {
        get
        {
            return IOCConfig.IOCContainer;
        }
    }
    protected override IController GetControllerInstance(RequestContext requestContext, Type controllerType)
    {
       if (null == controllerType)
       {
           return null;
       }
       IController controller = (IController)this.UnityContainer.Resolve(controllerType);
       return controller;
    }
    
    /// <summary>
    /// 释放
    /// </summary>
    /// <param name="controller"></param>
    public override void ReleaseController(IController controller)
    {
        //释放对象
        //this.UnityContainer..Teardown(controller);//释放对象
    }
}
```
