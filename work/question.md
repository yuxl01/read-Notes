
#### 1、动态生成复杂报表,并且导出（2017-10-15）
![原型图片](https://github.com/yuxl01/read-Notes/blob/master/imag/20171015-1.png)

###### * 分析及实现思路
分析报表结构：A列包含N个B列,B列包含N个C和D列,C,D列下面由N个E列,得出数据结构呈树形，因为要在数据库利用SQL或者存储过程查询实现有一定难度,所以选择利用C#代码实现</br>
</br>
1、首先根据结构制定存储的数据结构
```
//制定最终需要展示的数据结构模型
public class Result
{
    public IList<A> a_Detail{get;set;}
}
//制定A列的数据结构模型
public class A
{
   public IList<B> b_Detail{get;set;}
}
//制定B列的数据结构模型
public class B
{
   public IList<C> c_Detail{get;set;}
}
//制定C列的数据结构模型
public class C
{
   public IList<E> E_Detail{get;set;}
}
//制定E列的数据结构模型
public class E
{
   public string attrbute1{get;set;}
   public string attrbute2{get;set;}
   public string attrbute3{get;set;}
}
```
2、再根据从数据库查询出的结果,利用Linq或者Lambda进行不同维度的分组，最后将结果遍历插入Result对象中由json对象传到前端</br>
</br>
3、在前端js中遍历对应的json对象,绑定到Html页面
###### * 存在的弊端及优化思路
1、由于需要转换成指定格式和计算小计合计后端代码会涉及多次循环遍历,可以在存储过程中将预先需要分组的求和的数据查询好,将代码中计算的过程降到最小</br>
</br>
2、由于后端返回的Json格式原因,那么前端也将会有大量的循环遍历实现html绑定(Result结果包含多个A列,依次到E，那么会产生3层循环,从而时间复杂度为O(N3))，比较影响效率,大量循环为了防止页面空白可以将js放置在html的最末端加载,并且加入等待效果,(还可以后端查询一行,前端读取一行获取并加载，但是这样会有多次请求,相当于前端轮询后端数据，轮询请求比起循环读取内存消耗的时间还是用同步循环靠谱，理论上来说读取内存的速度远远快于网络的速度,如果加入异步,利用消息推送的机制应该可以，但是没有实验过)
