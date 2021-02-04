# Aop(Aspect Oriented Programming)

## dynamic proxy is the base of Aop
+ Dynamic proxy use a specific module to generic clazz code in JVM There's
no `.java` or `.class` file as a instance,just exists in memory when JVM run
then should be destroy with JVM close.
### 1、MethodBeforeAdvice
+ There are four step to program a code to implements function of dynamic
proxy using Spring framework.
+ With Spring, you will get the proxy Object with `getBean()` to target Object

- 1.You must have a bean as a target Object to handle you primary work
```xml
<bean id="**service" class="**.**.**"></bean>
```
- 2.Then,defined a Object to insert you aspect and add your extra code
```java
public Before implements MethodBeforeAdvice{
    /*
     *  Add extra code before you primary work
     *
     *  Method:   Method the before method executed (target method)
     *  Object[]: Target Object method argments
     *  Object:   Object which you extra code add(target Object)
     *
     */

    @override
    public void before(Method method, Object[] args, Object target){

    }
}
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
### 2、MethodInterceptor
+ This interface could let you add extra method where target method start
end or around, even though throw exception as you needed.
```java
public class Around implements MethodInterceptor{
    /*
     *  MethodInvocation:(method)==> target method
     *
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
+ invoke method can change target method returning

### Pointcut

#### Pointcut expression
```xml
(* *(..))  means --> all method

*  => any access modifier and any return value
*  => any methodname
() => argments list
.. => any (number,type) of argument

----------------------------------------
* login(String,String)
String login(com.edu.proxy.User,String)
1、when you write you expression you just give a type of return value or
argment type.
2、Types aren't in `java.lang.**`,you must write with all package name.
-----------------------------------------------------------------------

==> Whole expression--method-pointcut

accessModifier-returnValue package.class.method(argument-Type, argument-Type )
<==
-----------------------------------------------------------------------
==> Whole expression--class-pointcut

* package.class.*(..) # specific package
* *.class.*(..)  # only one layer package
* *..class.*(..) # one layer or multiple layer
<==
-----------------------------------------------------------------------
==> Whole expression--package-pointcut

* com.edu.proxy.*.*(..) # include all in pakeage proxy exclude subdirection
* com.edu.proxy..*.*(..) # include all in pakeage proxy and subdirection
<==
```

#### Pointcut Function
+ execute pointcut expression
    - execution -----------  # aim at all
    - args ----------------  # aim at method-args
    ```
    --> args(String,String) = execution(* *(String,String))
    ```
    - within --------------  # aim at package-class
    ```
    --> execution(* *..class.*(..)) = within(*..class)
    --> execution(* com.edu.proxy..*) = within(com.edu.proxy..*)
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

### 1.JDK dynamic proxy

- Have a public interface
- Extra function
- Target Object
    ```java
    Proxy.newProxyInstance(classLoader, Interface, invocationhandler)
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
                Object res = method.invoke(**Service,args);// let target methosd to run
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

### Base annotaion program AOP

+ target class
+ Aspect class
    - extra function
    - pointcut
    ```xml
    <bean id= "around" class="com.edu.aspect.**Aspect"
    <!-- config Annotation program AOP -->
    <aop:aspectj-autoProxy/>

    ```    


    ```java
    @Aspect
    public class **Aspect{
        @Around("execution(* *(..))")
        public Object around(ProceedingJoinPoint PJ){
            ......
            Object ret = PJ.process();
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
    @Pointcut("execution(* *(..))")
    public void doPointcut(){}

    @Around("doPointcut()")
    public Object around(ProceedingJoinPoint PJ){
        ......
        Object ret = PJ.process();
        ......
        return ret;
    }
}

```
