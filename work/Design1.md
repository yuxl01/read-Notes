# Mvc控制器工厂创建源码简洁提取

###### `1、定义创建控制器接口`

```.cs
public interface IControllerResolver
	{
		/// <summary>
		/// 根据指定的Controller类型获取对应的Controller实例
		/// </summary>
		/// <param name="controllerType"></param>
		/// <returns></returns>
		IController GetController(Type controllerType);
	}
```
###### `2、虚方法实现接口方便后期重写`

```.cs
internal class DefaultControllerResolver : IControllerResolver
	{
	
		public virtual IController GetController(Type controllerType)
		{
        //
		   return ControllerActivator.Create(requestContext, controllerType);
		}
	}
```

###### `3、创建控制器工场类提供2个方法`
###### `3.1默认使用接口IControllerResolver构造控制器类DefaultControllerResolver`

```.cs
public sealed class ControllerFactory
	{
		private static IControllerResolver s_controllerResolver = new DefaultControllerResolver();

		/// <summary>
		/// 设置IControllerResolver的实例，允许在框架外部控制Controller的实例化过程。
		/// </summary>
		/// <param name="controllerResolver"></param>
		public static void SetResolver(IControllerResolver controllerResolver)
		{
			if( controllerResolver == null )
				throw new ArgumentNullException("controllerResolver");

			// 这个方法通常会在程序初始化时调用，所以暂不考虑线程安全问题。
			s_controllerResolver = controllerResolver;
		}


		internal static object GetController(Type controllerType)
		{
		  	return s_controllerResolver.GetController(controllerType);
		}
	}
```

###### `4、外部可调用重写`

```.cs
//直接调用
 ControllerFactory.GetController(controllerType);
 
 //重写创建控制器类可用其他依赖注入容器实现调用构造控制器
 ControllerFactory.SetResolver(new UnityControllerFactory());
 
```
