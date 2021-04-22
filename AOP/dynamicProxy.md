# Aop(Aspect Oriented Programming)

## dynamic proxy is the base of Aop

+ Dynamic proxy use a specific module to generic clazz code in JVM There's
  no `.java` or `.class` file as a instance,just exists in memory when JVM run
  then should be destroy with JVM close.

  > Third Frame of Dynamic class
    - ASM
    - Javassist
    - CGlib

### 1、MethodBeforeAdvice
> There are four step to program a code to implements function of dynamic
proxy using Spring framework.

- 1.You must have a bean as a target Object to handle you primary work
```xml
<bean id=" *.*.service " class=" **.**.** "></bean>
```
- 2.Then,defined a Object to insert you aspect and add your extra code
   > this is a Advice ececute before you core Code, you need to implements
     MethodBeforeAdvice and override before method
```java
public Before implements MethodBeforeAdvice{
    /*
     *  Add extra code before you primary function
     *  Method:   Method  which the before method executed (target method)
     *  Object[]: Target Object method argments
     *  Object:   Object which you extra code add(target Object)
     *  --------------------------------------------------------
     *  这些参数可以根据需求进行使用，未必会全部用到，也可能完全不使用kk
     */
    @override
    public void before(Method method, Object[] args, Object target){
    }
}
```
   > add you aspect to config file 
```
    <bean id="log" class="**.**.**.Before">
```
- 3.Defined the pointcut which decide where you extra code execute
```xml
<aop:config>
<aop:pointcut id="log" expression="execution(* *(..))"/>
</aop:config>
```
- 4.config you pointcut with the target Object
```xml
<aop:config>
<aop:pointcut id="log" execution="* *(..)"/>
<aop:advisor advice-ref="before" pointcut-ref="log"/>
</aop:config>
```

**Attention**
  1. With Spring, you will get the proxy Object with `getBean()` other than 
     target Object
  2. 不管是目标类还是代理类，都是属于相同接口的实现，这样我们就可以使用接口
     来存储这个对象的引用，完成方法调用。
     

### 2、MethodInterceptor
+ This interface could let you add extra method where target method start,
  end or around, even though throw exception as you needed.
```java
public class Around implements MethodInterceptor{
    /*
     *  MethodInvocation:(method)==> target method
     *      - MethodInvacation 实际上对原始方法的进一步的封装
     *  invocation.proceed()=> let target method run,
     *  indecate you can control the time the target method run
     *
     *  Object : target method return value
     */

    @override
    public Object invoke(MethodInvocation invocation){
        try{
            // let target method run
            invocation.proceed();
        }catch(Exception e){
            System.out.println("Exception occur")
            e.printStackTrace()
        }
    }
}
```
**Attention**
+ invoke method can change target method returning
    - just change invoke method's retuan value rather than return invocation.proceed();
+ we coudle execute our extra function when the target method exception occured
    - Transaction

### Pointcut

#### Pointcut expression
```xml
(* *(..))  means --> all method

*  => any access modifier and any return value
*  => any methodname
() => argments list
.. => any (number,type) of argument

----------------------------------------
1. login(String,String)
    * when you write you expression you just give a type of return value or
   argment type.

2. String login(com.edu.proxy.User,String)
    * Types aren't in `java.lang.**`,you need write with all package name.

3. String login(String,..)
    * means fisrt arguments must be Strig, second argument no requirement.
   have or not,even more.
-----------------------------------------------------------------------

==> Whole expression  -->  method-pointcut

accessModifier-returnValue package.class.method(argument-Type, argument-Type )
<==
-----------------------------------------------------------------------
==> Whole expression  -->  class-pointcut

* package.class.*(..) # specific package
* *.class.*(..)       # only one layer package
* *..class.*(..)      # one layer or multiple layer package
<==
-----------------------------------------------------------------------
==> Whole expression  -->  package-pointcut

* com.edu.proxy.*.*(..)  # include all in pakeage proxy exclude subdirection
* com.edu.proxy..*.*(..) # include all in pakeage proxy and subdirection
<==
```

#### Pointcut Function
+ execute pointcut expression

    - execution -----------  # aim at all, could do all type of follow,other just simplify
    - args ----------------  # aim at method-args
    ```
    --> args(String,String) = execution(* *(String,String))
    ```
    - within --------------  # aim at package-class
    ```
    --> execution(* *..class.*(..)) = within(*..class)
    --> execution(* com.edu.proxy..*(..)) = within(com.edu.proxy..*)
    ```
    - @Annotation ---------- # aim at specific annotation
    ```
    --> @annotation(com.edu.proxy.Log)
    ```
- combined two pointcut function (and, or)
    ```
    execution(* login(String,String))=execution(* login(..)) and arg(String,String)
    ```
    **Atention** : aside of logic symbol(and) must be different type

## AOP --- how to work in source code

+ POP(Process Oriented Programming)
    - 以过程为基本单位的程序开发，通过过程之间的相互协同，调用,完成程序的构建

+ OOP (Object Oriented Programming)
    - 以对象为基本单位的程序开发，通过对象之间的相互调用，完成程序的构建

+ Aspect oriented Programming
    - 以切面为基本单位的程序开发，通过切面之间的协调，调用，完成程序的构建
    - AOP 是对OOP的补充。
    - 底层使用的是动态代理

    - 切面 = 切入点 + 额外功能

### 1.JDK dynamic proxy

- Have a public interface
- Extra function
- Target Object
    ```java
    Proxy.newProxyInstance(classLoader, Interfaces, invocationhandler)
    /*
     *  dynamic proxy use class genericed in memory,JVM don't give a classLoader
     *  to this generitic class. Must  borrow a classLoader from others.
     *   --**service.class.getClassLoader();
     */

    /*
     * interface----the public interface,counld get from target Object
     *   --**service.getClass().getInterface();
     */

    /*
     * invocationHandler interface used to add extra function (before,after,around,throwing)
     * override invoke method
     */
    InvocationHandler handler = new InvocationHandler(){
        @override
        public Object invoke(Object proxy, Method method,Object[] args){
            /*
             * proxy: proxy object
             * method: target method
             * Object[]: target method args
             */
            .....
                Object res = method.invoke(**Service,args);// let target method to run
            .....
                return res; //the target method return value
        }
    }
    ```

### 2.CGlib dynamic proxy

- Extends target class
- Extra function

```java

    /*
    *  Enhancer.setClassLoader();
    *  Enhancer.setSuperClass();
    *  Enhancer.setCallback() --> MethodInterceptor(cglib)
    *  Enhancer.create()
    */
    // Target class
    **service service  = new **service();
    Enhancer enhancer = new Enhancer();
    enhancer.setClassLoader(*Service.class.getClassLoader());
    enhancer.setSuperClass(*service.getClass());

    MethodInterceptor interceptor = new MethodInterceptor(){
        // the same as MethodInvocation  -- invoke
        @override
        public Object intercept(Object o，Object[] args,MethodProxy proxy){
            ....
            Object ret = method.invoke(*service,args);
            ....
            return ret;
        }
    }

    enhancer.setCallback(intercept);
    // enhancer can get the proxy Object
    *serviceProxy proxy = (*service)enhancer.create();
    // call target method for youself
    proxy.Targetmethod();
```

### 3.How to switch JDK to CGlib

- 1.Tranditional

```xml
<aop:config proxy-target-class="true">
    <aop:pointcut id="log" execution="* *(..)"/>
    <aop:advisor advice-ref="before" pointcut-ref="log"/>
</aop:config>
```
   2.Annotation
```xml
<aop:aspectj-autoProxy proxy-target-class='true'/>
```    

in springboot 2.*.* default use CGlib，when you want change to JDK should config this in application.yml
```yml
spring:
    aop:
       proxy-target-class: false

```

### Base annotaion program AOP

+ target class
+ Aspect class
    - extra function
    - pointcut
    ```xml
    <bean id= "around" class="com.edu.aspect.**Aspect"
    <!-- config Annotation program AOP -->
    <aop:aspectj-autoproxy/>

    ```    


    ```java
    @Aspect
    public class **Aspect{
        @Around("execution(* *(..))")
        public Object around(ProceedingJoinPoint PJ){
            ......
            Object ret = PJ.proceed();
            ......
            return ret;
        }
    }
    ```
### Pointcut reuse
```java
    
@Aspect
public class **Aspect{
    // extract pointcut top on method then counld be reuse in same case
    /*
    * - public 
    * - void
    */

    @Pointcut("execution(* *(..))")
    public void doPointcut(){}

    // value = 切入点函数名称
    @Around("doPointcut()")
    public Object around(ProceedingJoinPoint PJ){
        ......
        Object ret = PJ.process();
        ......
        return ret;
    }
}

```

### Spring AOP 中的注意事项


* 当我们在一个Service 实现类中使用this 来进行同类中方法调用的时候，此时，由于this是
  当前类对象，并不是spring 进行了代理的对象.
    - 这时候需要注入代理类,
    - 或者实现ApplicationContextAware 注入工厂对象的引用，进而获取到代理后的对象
```java
    public class UserServieImpl implements UserServive,ApplicationContextAware{
       
        private ApplicationContext ctx = null;

        @Override
        public void setApplicationContext(
        ApplicationContext applicationContext) throws BeansException {
            this.ctx = applicationContext;
        }
    }
```

