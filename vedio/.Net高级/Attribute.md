# Attribute 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;是一个类，可以用来标记元素，编译时生成到metadata里，平时不影响程序的运行除非主动用反射去查找，可以得到一些额外的信息/操作，然后给提供了更丰富扩展空间

1、特性可以影响编译器。</br>
2、特性可以影响程序运行。</br>
3、特性是生成metadata的在里面可以看到。</br>


####  一、特性的声明和基本方法
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;特性必须直接/间接继承自attribute的类，不能重复修饰同一个内容，约定俗成用attribute结尾,不是强制的，但是规范点比较好，然后声明的时候是可以省略的。

###### 1、声明特性类
``` .cs

 //【AttributeUsage特性修饰使用的特性】
 //第一个参数是表明当前特性能修饰的对象类型，枚举类 ，属性，字段（有16个可以被修饰的枚举对象）
 //第二个参数表示是否被多重修饰，一般很少用少用
 //第三个参数表示是否可以被继承和重写
 [AttributeUsage(AttributeTargets.All, AllowMultiple = false, Inherited = true)]
 public class CustomAttribute : Attribute
 {
        public CustomAttribute(string remark)
        {
            Console.WriteLine("这里是CustomAttribute带参数");
        }
        public string Description { get; set; }
        public void Show()
        {
            Console.WriteLine($"This is {this.GetType().Name}");
        }
 } 
```
###### 2、调用特性
``` .cs
 //调用特性修饰类
  [CustomAttribute("测试特性", Description = "给特性属性赋值"])
  public class Test
  {
     //特性修饰属性
     [CustomAttribute]
     public int Id { get; set; }
  }
```

####  二、特性的使用
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在声明和编译后本身是没用的，由于特性编译后是在metedata里,所以想使用里面的函数，就必须使用到反射。

``` .cs
//实力一个被特性修饰的类对象
Test test =new Test();
//反射获取当前类型
Type type = test.GetType();
//检测有没有这个特性
if (type.IsDefined(typeof(CustomAttribute), true))
{
    //找到修饰当前类所有的特性集合
    object item = type.GetCustomAttributes(typeof(CustomAttribute), true)[0];
    CustomAttribute attribute = item as CustomAttribute;
    //调用特性类中的函数
    attribute.Show();
}
```
