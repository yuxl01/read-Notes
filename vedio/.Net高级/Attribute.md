# Attribute 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是一个类，可以用来标记元素，编译时生成到metadata里，平时不影响程序的运行除非主动用反射去查找，可以得到一些额外的信息/操作，然后给提供了更丰富扩展空间

1、特性可以影响编译器。</br>
2、特性可以影响程序运行。</br>
3、特性是生成metadata的在里面可以看到。</br>
4、特性本身没有作用，必须要框架支持（反射调用）</br>
5、使用特性实现AOP</br>


####  一、特性的声明和基本方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;特性必须直接/间接继承自attribute的类，不能重复修饰同一个内容，约定俗成用attribute结尾,不是强制的，但是规范点比较好，然后声明的时候是可以省略的。

###### 1、声明特性类
``` .cs

/// <summary>
///【AttributeUsage特性修饰使用的特性】
/// 1、第一个参数是表明当前特性能修饰的对象类型，枚举类 ，属性，字段（有16个可以被修饰的枚举对象）
/// 2、第二个参数表示是否被多重修饰，一般很少用少用
/// 3、第三个参数表示是否可以被继承和重写
/// </summary>

 [AttributeUsage(AttributeTargets.All, AllowMultiple = false, Inherited = true)]
 public class CustomAttribute : Attribute
 {
 } 

```
###### 2、常见的特性修饰
``` .cs

  //特性修饰类
 [CustomAttribute]
 public class Test
 {
     //特性修饰属性
     [CustomAttribute]
     public int Id { get; set; }

     //修饰方法返回值
     [return: CustomAttribute]
     public int result([CustomAttribute] string str) //修饰参数
     {
         return 0;
     }
 }
```


####  二、特性的使用
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在声明和编译后本身是没用的，由于特性编译后是在metedata里,所以想使用里面的函数，就必须使用到反射。
###### 1、使用特性添加附加信息
``` .cs
     /// <summary>
    /// 声明一个权限验证的特性类，特性只能用户枚举和字段，不支持多重修饰，可以继承重写
    /// </summary>
    [AttributeUsage(AttributeTargets.Enum |AttributeTargets.Field, AllowMultiple = false, Inherited = true)]
    public class AuthorityAttribute : Attribute
    {
        public AuthorityAttribute(string description)
        {
            this.Description = description;
        }

        public string Description { get; private set; }

    }
```
``` .cs

    /// <summary>
    /// 创建一个被特性修饰的枚举类
    /// </summary>
    [AuthorityAttribute("用户状态")]
    public enum UserState
    {
        /// <summary>
        /// 正常
        /// </summary>
        [AuthorityAttribute("正常")]
        Normal = 0,
        /// <summary>
        /// 冻结
        /// </summary>
        [AuthorityAttribute("冻结")]
        Frozen = 1,
        /// <summary>
        /// 删除
        /// </summary>
        [AuthorityAttribute("删除")]
        Deleted = 2
    }
```
``` .cs
 /// <summary>
 /// 利用反射调用
 /// </summary>
public  class Program
 {
     static void Main(string[] args)
     {
         //获取枚举类型
         Type type = typeof(UserState);
         //获取字段
         FieldInfo field = type.GetField(UserState.Normal.ToString());
         //判断字段是否被特性修饰
         if (field.IsDefined(typeof(AuthorityAttribute), true))
         {
             //类型转换
             AuthorityAttribute remarkAttribute = (AuthorityAttribute)field.GetCustomAttribute(typeof(AuthorityAttribute));
             Console.WriteLine(remarkAttribute.Description);
         }         
     }
 }
```
###### 2、使用特性做验证操作

``` .cs
/// <summary>
/// 创建一个特性验证的抽象基类
/// </summary>
[AttributeUsage(AttributeTargets.Property, AllowMultiple = false, Inherited = true)]
public abstract class AbstractValidateAttribute : Attribute
{
    public abstract bool Validate(object oValue);
}

/// <summary>
/// 声明一个长度验证的特性类
/// </summary>
public class LongValidateAttribute : AbstractValidateAttribute
{
   private long _lMin = 0;
   private long _lMax = 0;
   public LongValidateAttribute(long lMin, long lMax)
   {
       this._lMin = lMin;
       this._lMax = lMax;
   }

   public override bool Validate(object oValue)
   {
       return this._lMin < (long)oValue && (long)oValue < this._lMax;
   }
}

/// <summary>
/// 声明一个检验值不能为空的特性类
/// </summary>
public class RequirdValidateAttribute : AbstractValidateAttribute
{
   public override bool Validate(object oValue)
   {
       return oValue != null ;
   }
}
/// <summary>
/// 声明实现验证的公共类
/// </summary>
public class DataValidate
 {
     public static string Validate<T>(T t)
     {
         Type type = t.GetType();
         string result = string.Empty;
         foreach (var prop in type.GetProperties())
         {
             if (prop.IsDefined(typeof(AbstractValidateAttribute), true))
             {
                 object item = prop.GetCustomAttributes(typeof(AbstractValidateAttribute), true)[0];
                 AbstractValidateAttribute attribute = item as AbstractValidateAttribute;
                 if (!attribute.Validate(prop.GetValue(t)))
                 {
                     result = "当前"+prop.Name+"值未通过验证";
                     break;
                 }
             }
         }
         return result;
     }
 }
 //调用T为被特性修饰的类
DataValidate.Validate<Student>(new T(){});
```




