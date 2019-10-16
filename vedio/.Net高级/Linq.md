# Linq to Sql And Linq to Object

#### 一、匿名类，匿名方法

    1、静态类型，不能直接调用，因为静态类型决定了没有Id属性（c# 3.0）。
    2、动态类型dynamic，实行动态编译，实例是只读,可以避开编译器检查（c# 4.0）。
    3、匿名类型()在编译时会生成一个泛型的匿名类，而且类中的属性都是只读的。
    4、匿名方法是一个语法糖，其实经过编译器在编译之后有一个实际完整方法存在。
    5、var 是一个语法糖，由编译器根据右边的类型推算出实际类型(配合匿名类型使用，复杂类型的使用)。
    
``` .cs

//静态类型3.0
object model = new
{
    Id = 2,
};

//动态类型4.0
dynamic dModel = new
{
    Id = 2,
};

//匿名类型 3.0
var varModel = new
{
     Id = 2,
 };
 
//匿名方法
Action act1 = new Action(delegate ()
{
    Console.WriteLine("匿名方法");
});

```


#### 二、扩展方法
###### ```1、扩展方法的定义:```
      
      扩展方法是C#3.0时引入的新特性，最常见的是在LINQ中的使用，扩展方法是一种特殊类型的静态方法
      扩展方法可以在不改变该类型源码的前提下为它的实例提供新的成员。
      
###### ```2、构建扩展方法的步骤：```
    
     1、创建一个静态类,在类中创建一个静态方法。
     2、为这个静态方法添加至少一个参数，并在第一个参数前加上this关键字，这个关键字会告诉编译器当前方法是一个扩展方法。
        而这个方法将成为第一个参数所属类型的新成员。

``` .cs
/// <summary>
/// 扩展方法：静态类里面的静态方法
/// </summary>
public static class ExtendMethod
{
    /// <summary>
    /// 第一个参数类型前面加上this
    /// </summary>
    /// <param name="list"></param>
    /// <returns></returns>
    public static List<string> extendInstance(this List<string> list)
    {
        return list;
    }
}
```
      
###### ```1、扩展方法使用需要注意:```
    1、假如扩展了方法又有相同的实例方法以实例方法为准，大量使用的话需要注意。
    2、this修饰的类型如果权限过大,其子类都能使用整个方法,如扩展方法参数修饰为object,最终影响代码可读性。
    3、不要对基础类型进行扩展。

#### 三、Linq to Object

        继承自IEnumable接口,所有的操作都是已经在内存中的数据，可以说速度是很快的。
        
###### `一、linq to object(查询表达式)`    
 `1、简单实现lambda的小栗子`
``` .cs
public static class ExtendMethod
{
    // 实现list Where的lambda表达式
    public static List<int> Where(this List<int> list, Func<int, bool> predicate)
    {
        List<int> result = new List<int>();
        foreach (var item in list)
        {
            if (predicate.Invoke(item))
            {
                result.Add(item);
            }
        }
        return list;
    }
}
List<int> list = new List<int>(){1,2,3,4,5,6,7,8};
//调用，已经得到结果
var result = list.Where(t=>t>3);
//输出4，5，6，7，8

```
 `2、加入IEnumerable和Yield之后实现的lambda(迭代器) `
        yield:是一个状态机,可以理解为只是记录整个操作,并没有及时计算，实现延迟获取，用到的时候才去查询。

``` .cs

// 实现list结合IEnumerable和yield Where的lambda表达式
public static IEnumerable<T> Where<T>(this IEnumerable<T> tList, Func<T, bool> predicate)
{
    if (tList == null || predicate == null)
    {
        throw new Exception("can't be null");
    }
    foreach (var item in tList)
    {    
        if (predicate.Invoke(item))
        {
            yield return item;
        }
    }
}

List<int> list = new List<int>(){1,2,3,4,5,6,7,8};
//因为有了yield return 调用时候并没有计算出结果，如果想直接得到结果可以ToList转换一下。
var result = list.Where(t=>t>3);
//用的时候才回去执行计算
foreach(var i in result){}
```

###### `二、linq to Object (查询运算符)`
``` .cs
 List<int> list = new List<int>(){1,2,3,4,5,6,7,8};
 //查找list中大于3的数字
 var list = from s in list where s>3 select s;
 
 //查找list中大于3的数字投影到一个新的匿名类型中
 var list = from s in list where s>3 select new{
       a = s+1,
       b = s+2
 };
 

```
