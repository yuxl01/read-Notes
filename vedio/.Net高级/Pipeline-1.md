# Pipeline(一)

### 思考的问题
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MVC,WebForm 之类的框架做应用程序的开发之后部署到IIS,用户在浏览器输入url地址之后是怎么到达我们的应用程序？

##### 一、什么是管道模型?
 1、可以理解为是http请求的一个通道抽象出的模型。

##### 二、它的作用是什么?
1、对用户请求Url到达服务器的过程生命周期流程的支撑。

##### 三、Pipeline的请求流程在IIS中如何体现的?
1、用户发起请求到达DNS,DNS会解析域名找到对应的IP及端口。</br>
2、IIS中HttpSys监听服务接受请求。</br>
3、HttpSys监听服务根据请求类型的后缀,将请求转发到对应的应用程序处理(isapi.dll),IIS中处理程序映射可以实现配置。</br>
4、不同的后缀将到达不同的处理程序.net的请求映射到aspnet.isapi,java或者其他的也可以配置对应的dll。</br>
5、如果MVC请求没有后缀，IIS6会为没有后缀的加一个默认的.axd后缀，IIS 7之后就不用了,可以直接映射到HttpRoutingModules。

#### 四、流程图
![原型图片](imag/pipeline-1-1.png)

