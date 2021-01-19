# 第一章 引言

1、EJB (Entweprise Java Bean) 问题

- 运行环境苛刻 （特殊的运行环境，websphere[IBM],weblogic[oracle]..）
- 代码移植性差  (实现框架的特殊运行接口，环境改变，代码需要改变)
    
   总结：EJB是一个重量级别框架

2、什么是spring

- spring 是一个轻量级的JavaEE解决方案，整合了众多优秀的设计模式
        
     + 解决javaEE开发中的分层开发的所有层次中的问题--> 是一个完整的解决方案
     + 不需要实现任何服务器的环境接口，对运行环境要求低，
        * 可以在目前主流的所有web服务器（tomcat,resion,jetty）中运行
        * 开源的代码，可以根据需要进行定制化开发
     + 整合了许多优秀的设计模式：
        * 工厂（IOC）
        * 代理（AOP）
        * 模板 
        * 策略


3、设计模式
    
    * 广义上：面向对象开发过程中，解决特定问题的经典代码
    * 狭义上：GOF4人帮定义的23种设计模式：工厂，适配器，装饰器，门面，代理，模板...
    

4、工厂设计模式
    
- 概念：通过工厂，创建对象
```
    User user = new User();
```

- 好处：解耦合
    
    * 耦合：指定是代码间的强关联关系，一方的改变会影响到另一方 
    * 问题：不利于代码的维护，--> 简单理解就是将接口实现类硬编码在代码中



4.1 简单工厂
```java
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import com.edu.service.UserService;

public class BeanFactory {

    private static Properties env = new Properties();
    /** 将IO 加载等内容应该集中放在一起，在程序一启动的时候就加载好， 
     * 在这äª程序中最适合的方式就是放在静态代码块中 *   
     *
     */
     static {
        try {
        // 获取文件的IOInputStream 
         inputStream = BeanFactory.class.getResourceAsStream("/applicationContext.properties");// 加载文件内容            
         env.load(inputStream);
         inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    /**
     *  对象的创建方式： 
     *  1、直接调用构造方法进行创建 
     *     UserService userService = new UserServiceImpl(); 
     *  2、使用反射的方式，创建对象，解耦合 
     *    Class clazz = Class.forName("com.edu.service.impl.UserServiceImpl"); 
     *     UserService userService = clazz.newInstance(); 
     */
     public static UserService getUserService() {
        UserService userService = null;
        try {
            Class clazz = Class.forName(env.getProperty("userService"));
            userService = (UserService)clazz.newInstance();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        return userService;
    }
}
```

4.2 通用工厂

将创建对象的共性代码提取出来，减少冗余的代码重复，就衍生出了通用的对象工厂

```java
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;
import com.edu.service.UserService;

public class BeanFactory {

    private static Properties env = new Properties();
     static {
        try {
            inputStream = BeanFactory.class.getResourceAsStream("/applicationContext.properties");// 加载文件内容            
            env.load(inputStream);
            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    /**
     *  将创建对象的共性代码提取出来，减少冗余的代码重复，
     *  就衍生出了通用的对象工厂
     */
    public static Object getBean(String key) {
        Object bean = null;
        try {
            Class clazz = Class.forName(env.getProperty(key));
            bean = clazz.newInstance();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
        return bean;
    }
}
```
4.3 通用工厂的使用方式

- 需要被创建的类 **.java
- 准备applicationContext.properties文件：key=value
- 通过工厂方法来获取对应的对象 ** =  (**)BeanFactory.getBean(key)


5、总结

spring的本质：
    - 工厂：ApplicationContext
    - 配置文件：applicationContext.xml








