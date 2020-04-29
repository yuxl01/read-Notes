# MVC(二)

### 一、AuthorizeAttribute 
         可以在控制器和Action以及全局使用。
###### 1.实现
  
```.cs
public override void OnAuthorization(AuthorizationContext filterContext)
{
    if (filterContext.ActionDescriptor.IsDefined(typeof(AllowAnonymousAttribute), true)
    || filterContext.ActionDescriptor.ControllerDescriptor.IsDefined(typeof(AllowAnonymousAttribute), true))
    {
        return;//表示支持控制器、action的AllowAnonymousAttribute
    }
    var sessionUser = HttpContext.Current.Session["CurrentUser"];//使用session
    //var memberValidation = HttpContext.Current.Request.Cookies.Get("CurrentUser");//使用cookie
    //也可以使用数据库、nosql等介质
    if (sessionUser == null || !(sessionUser is CurrentUser))
    {
        HttpContext.Current.Session["CurrentUrl"] = filterContext.RequestContext.HttpContext.Request.RawUrl;
        filterContext.Result = new RedirectResult(this._LoginPath);
    }
}
//控制器和Action添加[AllowAnonymous]
//全局文件添加configFiter中加入filters.Add(new Web.Core.Filter.AuthorityFilterAttribute());
```
 ###### 2.取消认证
    使用[AllowAnonymous]特性
   

### 二、HandleErrorAttribute

### 三、IActionFilter

### 四、IResultFilter

### 五、生成写入验证码
