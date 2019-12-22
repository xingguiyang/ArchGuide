# Tomcat是一个Servlet容器？
作者：鲁班学院-讲师周瑜

“Tomcat是一个Servlet容器”，这句话对于2019年的程序员应该是耳熟能详的。

单纯的思考一下这句话，我们可以抽象出来这么一段代码：

```java
class Tomcat {
	List<Servlet> sers;
}
```

如果Tomcat就长这样，那么它肯定是不能工作的，所以，Tomcat其实是这样：

```java
class Tomcat {
    Connector connector; // 连接处理器
	List<Servlet> sers;
}
```

我们这里先不考虑Connector的底层实现，我们只需知道Connector是负责处理请求的。

我们还是来想想**容器**。

<a name="0DDtZ"></a>
## Context
顾名思义，Servlet容器就是用来装载存储Servlet的。

一个Servlet表示一个运行在服务端的程序（servlet = server + applet）。用户想要使用这种程序，需要向该程序发送请求以及获取该程序的响应，也就是Servlet规范中的ServletRequest、ServletResponse。

所以Servlet其实就是Java中用来处理请求的一种规范，所以我们的项目中通常都会有一个或多个Servlet，由它来负责接收请求，或者将请求转交给其他业务逻辑。

所以我们的Spring MVC、Spring Boot都存在一个DispatcherServlet（类似功能的一个Servlet，负责接收请求）。

所以，通常Servlet是属于一个应用程序（项目）的，换句话说，我们的一个应用包含多个Servlet，所以这是**第二层Servlet容器--应用，也就是Tomcat中的Context（应用上下文）**。那么第一层Servlet容器呢？

## Wrapper
Wrapper就是第一层Servlet容器，Wrapper表示Servlet的包装者，所以它是最接近Servlet的，那么为什么需要Wrapper呢？

我们通常认为Wrapper是这样的：
```java
class Wrapper {
	Servlet servlet;
}
```

一个Wrapper对应一个Servlet，这么来想的话，确实不需要Wrapper，但是我们还要考虑一些其他的情况：

- 比如Filter，一个Filter是可以对应一个Servlet的。
- 比如ServletPool，通常的Servlet是所有请求线程公用的，但是在Servlet中支持每一个请求线程单独使用独立的Servlet实例。

所以在Wrapper中，不仅仅只包括一个Servlet，还包括过滤器和Servlet池，所以**Wrapper是第一层Servlet容器**。


## Host
在我们现实生活中，一个应用都是部署在一个主机上的，所以，一个主机可以包含多个应用，一个应用包含多个Servlet，所以，**Host是第三层容器。**<br />**<br />在Tomcat中，Host表示虚拟主机，Tomcat在处理请求时，可以根据请求的域名进入到相应的Host中进行处理。<br />**

## Engine
Host管理Context，Context管理Wrapper，Wrapper管理Servlet，而Engine就是用来管理Host的。**所以Engine是第四层容器。**<br />**<br />**

## 最后
肯定有人有疑问，那么Engine之上不需要容器了吗？不需要了？举个例子：

我们的钱（Servlet）要放在钱包(Wrapper)里，钱包要放在书包(Context)里，书包要放在行李箱(Host)里，行李箱要放在飞机(Engine)上。

所以，如果你问我“Engine放哪？”就相当于问我“飞机放哪？”

答案是**不再需要更高层次的容器了，因为没有必要了**。


## 总结
在Tomcat中，容器分为：

1. Wrapper
1. Context
1. Host
1. Engine






