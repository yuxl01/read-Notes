# Reflection (一)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;反射是.Net Framework提供的一个帮助类库，可以访问dll和exe的metadata，并且使用它，可以在代码中将对具体类型的以来转化成会字符串的以来，实现程序解耦。</br>

    1、metadata是dll和exe的描述文件。
    2、dll是应用程序扩展。
    3、exe是可执行程序。
    4、pdb是调试文件,供vs工具使用。

#### 一、反射的基本使用
###### 1、基本使用
``` .CS
//加载dll到反射实例(命名空间+类名)1、不存在的dll会报错,2、使用时缺少加载的dll的依赖项也会报错
Assembly assemly = Assembly.Load("`nameSpace.class`"); 

//获取模块信息
assemly.GetModules();

//获取类型的数组集合
assemly.GetTypes();

//获取类型下的方法集合
assemly.GetTypes()[0].GetMethods();

#region 加载类型并创建实例
//利用反射加载指定的类型
Type type = assemly.GetType("Class");
//调用无参构造函数创建一个运行对象
object instance = Activator.CreateInstance(type);
调用有参构造函数（按顺序）创建一个运行对象
object instance1 = Activator.CreateInstance(type,new object[] { "parmeter"});
#endregion
```
###### 2、操作方法`Method`
``` .cs
#region 调用方法
//加载类型
Type testType = assemly.GetType("Class");
//构造对象
object oTest = Activator.CreateInstance(testType);
//获取指定函数
MethodInfo method = testType.GetMethod("methods",new Type[] {"typeof(int)函数参数类型，有重载的情况" });
//调用函数
method.Invoke(oTest, null);
//获取方法参数
method.GetParameters()
//获取方法返回值
method.ReturnParameter


//调用私有方法,其实没啥意义
MethodInfo method = testType.GetMethod("method4", BindingFlags.Instance | BindingFlags.NonPublic);
method.Invoke(oTest,null);

//调用泛型方法
Type genericType = assemly.GetType("GenericMethod");
object oGeneric = Activator.CreateInstance(genericType);
//获取方法
MethodInfo method = genericType.GetMethod("Show");
//指定方法参数类型
MethodInfo methodNew = method.MakeGenericMethod(typeof(int), typeof(string), typeof(int));
//调用函数
methodNew.Invoke(oGeneric, new object[] { 1, "jeck", 0 });
#endregion
``` 
###### 3、操作属性 `Property`
 ``` .cs
 //获取类型
Type type = typeof(Class);
object instance = Activator.CreateInstance(type);

//获取所有的属性集合并开始遍历
foreach (var item in type.GetProperties())
{
    var name = item.Name; //获取属性名
   
    item.SetValue(instance, "oVal"); //动态设置对象instance的属性的值
   
    item.GetValue(oPeople); //获取当前属性值
}
//获取所有的字段集合并开始遍历
 foreach (var item in type.GetFields())
 {
     var name = item.Name; //获取字段名
     item.SetValue(instance, "str"); //动态设置对象instance的字段的值
     item.GetValue(oPeople); //获取当前字段的值
 }

 ```

#### 二、反射调用泛型类
``` .cs
//调用泛型（有几个参数就在类型后加`3,代表3个参数）
Type genericType = assemly.GetType("namespace.GenericClass`3");
//指定类型
Type genericNewType = genericType.MakeGenericType(typeof(int), typeof(string), typeof(Program));
//创建一个运行时对象
object oGeneric = Activator.CreateInstance(genericNewType);


```

#### 三、反射能做哪些看起来`很厉害`的东西？
    1、动态创建对象，降低程序耦合性
    2、反射+配置文件+简单工厂 = IOC
    3、反射+方法 =MVC
    4、反射+属性 =ORM

#### 四、反射的`优点`和`缺点`
    1、动态，实现解耦，可配置。
    2、避开编译器的检查。
    3、性能问题，但是绝对值小，不会影响项目性能，可以使用泛型缓存优化。



