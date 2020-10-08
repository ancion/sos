# BeanPostProcessor
reprocessing bean after spring create bean,when you use `getBean()`
could get a proxy Object.

## BeanPostProcessor is a interface
you should add you own reprocessing content in this interface method.
This interface use the principle of dynamic proxy.

```java
    // before Initialization reprocessing,then return bean
    Object postProcessBeforeInitialization(Object bean,String beanName);
    // after Initialization reprocessing bean, then return bean
    Object postProcessAfterInitialization(Objecct bean,String beanName);
``` 


```java
public class MyBeanPostProcessor implements BeanPostProcessor{
    @override
    public Objecct postProcessBeforeInitialization(Object bean,String beanName){
        return bean;
    }
    // This method can be used to every bean that created by Spring
    @override
    public Object postProcessAfterInitialization(Object bean,String beanName){
        // Here must have a constrains of type.
        if(bean instanceof **){
            ....
        }
        return bean;
    }
}
```
**Atention:**
- you could choose one of before and after,but you should return
bean in both method.
- Spring should reprocessing every bean use you override method.
