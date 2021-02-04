## How Spring integrate mvc

+ first.create a WebXmlApplicationContext Factory

+ make sure Factory created only once and should be used public 
```java

    ServletContextListen --> can listen the creating of ServletContext,then ServletContextListen 
    could crete this WebApplicationContext.

    // set attribute to ServletContext,so that client use the Factory public.
    ServletContext.setAttribute("*",webXmlApplicationContext);

    // Spring already help us do follows,we only user it 
    ContextLoaderListener implements ServletContextListen,
```
    
```xml
<listener>
    <listener-class>org.sprinframework.web.context.ContextLoaderListener</listener-class>
</listener>
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mvc.xml<param-value>
</context-param>
```

## What controller doing
+ reveice parameter of client
+ call service
+ path switch and return response(like JSON)

### How to integrate 
```xml
1.crete a integrate xml,in this file, use import tag refrence other xml,
when you use the config file,you can use it directly.
    <import reource='classpath:applicationContext-a.xml'/>
    <import reource='classpath:applicationContext-b.xml'/>
    <import reource='classpath:applicationContext-c.xml'/>
    <import reource='classpath:applicationContext-d.xml'/>

2.use `*` char as a random 
    ApplicationContext  ctc = new  ClasspathXmlApplicationContext('applicationContext-*.xml')

```
