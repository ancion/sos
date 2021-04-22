## How Spring integrate mvc (Core Idea)

### Prepared web Factory

1. first.create a WebXmlApplicationContext Factory

 > make sure Factory created only once and should be used public 
```java
    
    1. 为了保证创建的工厂是全局唯一，应用内公用，所以需要将这个工厂放到公共容器ServletContext中
    但是servletContext 创建的时机是什么，我们需要在创建这个对象的时候同时创建我们的工厂并完成设置。

    ServletContextListen --> can listen the creating of ServletContext,then 
    ServletContextListen could crete this WebApplicationContext.

    // set attribute to ServletContext,so that client use the Factory public.
    ServletContext.setAttribute("*",webXmlApplicationContext);

    // Spring already help us do follows,we only user it 
    public class ContextLoaderListener implements ServletContextListen{
        /**
         * Initialize web application context
         */
        @Override
        public void contextInitialized(ServletContextEvent event){
            initWebApplicationContext(event.getServletContext());
        }
    }
```
2. Springweb 给我们创建了工厂，我们怎么使用呢

* web.xml中进行如下配置

```xml
<listener>
    <listener-class>org.sprinframework.web.context.ContextLoaderListener</listener-class>
</listener>

<!-- 指定工厂配置文件的位置,固定配置-->

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mvc.xml<param-value>
</context-param>

```

### What controller doing
+ resolve request of client 
+ reveice parameter of client --> (request.getParameter("name"))
+ call service --> service.method(); 
+ resolve view(.jsp)
+ path jump and return response --> (like JSON,JSP,Freemaker,Thyemeleaf)

### Which framework Could Integrate

+ struts1
+ webwork
+ jsf
+ struts2
+ springMVC  (main)


### How to integrate 
```xml
1. crete a integrate xml,in this file, use import tag refrence other xml,
when you use the config file,you can use it directly.
    <import reource='classpath:applicationContext-a.xml'/>
    <import reource='classpath:applicationContext-b.xml'/>
    <import reource='classpath:applicationContext-c.xml'/>
    <import reource='classpath:applicationContext-d.xml'/>

2. use `*` char match all 
    ApplicationContext  ctc = new  ClasspathXmlApplicationContext('applicationContext-*.xml')

```
