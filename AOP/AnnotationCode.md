# Annotation Programming
Add a tag like `@Component` top on class or method,can do some specific function 
or help you to do something with some rule.

## annotation  action 
+ replace *.xml file 
+ simplified configuration
+ The contractual of interface  

### Component

+ config Component scan
```xml
<context:component-scan basePacage="com.edu"/>
```
+ exclude filter
```xml
<!--  start Component scan in spring 
    could scan this package and subpackage
-->
<context:component-scan basePacage="com.edu">
    <!-- could exclude some class not need -->
    <!-- exclude specified type-->
    <context:exclude-filter type='assignabele' expression='*.*.user'/>
    <!-- exclude specified annotaion-->
    <context:exclude-filter type='annotaion' expression='*.*.@Log'/>
    <!-- exclude specified aspect (only user expression are class or package)-->
    <context:exclude-filter type='aspectj' expression='com.edu..*'/>
    <!-- exclude regex-->
    <context:exclude-filter type='regesx' expression=''/>
    <!-- exclude cusotm -->
    <context:exclude-filter type='custom' expression=''/>
</context:component-scan>
     
```
+ Include filer

```xml
<context:component-scan basePacage="com.edu" use-default-filter='false'>
    <!-- could exclude some class not need -->
    include specified type
    <context:include-filter type='assignabele' expression='*.*.user'/>
    include specified annotaion
    <context:include-filter type='annotaion' expression='*.*.@Log'/>
    include aspect (only use expression are class or package)
    <context:include-filter type='aspectj' expression='com.edu..*'/>
    include exclude regex
    <context:include-filter type='regesx' expression=''/>
    include cusotm
    <context:include-filter type='custom' expression=''/>
</context:component-scan>
```
**Atention:**  
+ both exclude and include can be used superposited
+ Following annotation are same as @Component,just distinct different function module
+ @Service
+ @Controller
+ @Repository

### Autowired 

+ could be set at Constructor,Setter,or Field,set at private feild(use reflect)
+ default,inject accorjding to the type of class
    - same type(self,subclass,interfaceImpl)
```java
    @Autowired
    private UserDao userDao
```
+ combind with qualifier(basee on name)
    - specified with name(same type have several bean)
```java 
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao
```
+ same function annotation in EJB250
```
    // when you not specify the beanName,then could be injected by type
    @Resource
    private UserDao userDao

    @Resource(name='userDaoImpl')
    private UserDao userDao
```
### JDK Simple type inject with annotation

+ @PropertSource("classpath:/*.properties") 
    - This annotation could replace following spring-configurer context
```xml
    <!-- lead in property file -->
    <context:property-placeholder location="classpath:*.properties">
```
+ In code
```
    @Value("${Key}")
    private Integer age;
```
**Atention:** 
+ @Value not work on `static` field.
+ @Value can't inject value for Collections type

### annotation bean can be used in xml file
```java
@Repository
public class UserDaoImpl implements UserDao{

}
```
use this bean in xml file

```xml
<bean id='userService' class='com.edu.service.UserServiceImpl'>
    <property name='userDao' ref='userDaoImpl'/>
</bean>
```
### @Configuration

same as @Component
```java

    ApplicationContext ctx = new AnnotationConfigApplicationContext();
    // specify the configBean class
    ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    // specify the package of configclass
    ApplicationContext ctx = new AnnotationConfigApplicationContext("com.edu.config");
```


### integrate log
+ annotation programming can't intergrade log4j,
+ integrate logback
    - Slf4j
    - logback
+ prepared logback.xml file in root directory
```xml
<?xml version='1.0',encoding='utf-8'?>
<configuration>
    <!-- defined path output -->
    <appender name='STOUT' class='ch.qos.logback.core.ConsoleAppender'>
        <encoder>
            <!-- format output log, 
                %d : means date, 
                %thread : means thread-name, 
                %-5level : means log level show 5 char width from left
                %msg : means log content
                %n : means end of a line
            -->
            <parttern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</parttern>
        </encode>
    </appender>
    <root level="DEBUG">
        <appender-ref ref='STOUT'/>
    </root>
</configuration>
```


### Bean

* using this @Bean annotation to create configBean
* function return value as bean
* beanName default is functionName(first char lowercase)
* @Bean("beanName") could give a custom name 
* @Scope("Singleton"|"protoType") could control number of instance 

### @ComponentScan

* exludeFilters
```java
@ComponentScan(basePacage = 'com.edu',
               excludeFilters = {@ComponentScan.Fliter(type=FilterType.ANNOTATION,value={org.springframework.sterotype.Service'}),
                                 @ComponentScan.Filter(type=FilterType.ASPECTJ,pattern="*..User")})
type=FilterType.ANNOTATION          value
               .ASSIGNABLE_TYPE     value
               .ASPECTJ             pattern
               .REGEX               pattern
               .CUSTOM              value
```
* includeFilters
```java
@ComponentScan(basePacage = 'com.edu',useDefaultFilters = false,
               includeFilters = {@ComponentScan.Fliter(type=FilterType.ANNOTATION,value={org.springframework.sterotype.Service'}),
                                 @ComponentScan.Filter(type=FilterType.ASPECTJ,pattern="*..User")})
type=FilterType.ANNOTATION          value
               .ASSIGNABLE_TYPE     value
               .ASPECTJ             pattern
               .REGEX               pattern
               .CUSTOM              value
```

### @Import

+ Spring framework using to integrate other framework
+ multiple bean integrate 

### configuration priority

* @Component(and same type) < @Bean < Bean Tag in *.xml file
* If you  want a bean to override another using a prioriter configuration
must use same `id`.

### integrate multiple configuration
```java
// 1、definde a package as config 
ApplicationContext ctx = new  AnnotationConfigApplicationContext("com.edu.config")
ApplicationContext ctx = 
    new  AnnotationConfigApplicationContext(AppConfig1.class,AppConfig2.class,...)
// 2、@Import integrate other config into main configclass

    @Configuration
    @Import(AppConfig2.class)
    public class AppConfig{

    }
// 3、@ImportResource integrate annotation configuration and *.xml file
    @ImportResource("applicationContext.xml")
    @Configuration
    public class AppConfig{

    }
```
### four side in one box

+ 1、schema
    ```xml
        <context:property-placeholder location="classpath:log.properties"/>
    ```
+ 2、specific functional Annotation【recommend】
    ```java
        @PropertSource("classpath:log.properties")
    ```
+ 3、<bean> tag
    ```xml
        <bean id='propertyholder' class='org.springframework.support.PropertySourcesPlaceholderConfigurer'>
            <property name='location' value='classpath:log.properties'>
        </bean>
    ```
+ 4、@Bean annotation
    ```java
        @Bean
        public PropertySourcesPlaceholderConfigurer configurer(){
            PropertySourcesPlaceholderConfigurer configurer = 
                new PropertySourcesPlaceholderConfigurer();
                configurer.setLocation(new ClassPathResource("log.properties"));
            return configurer;
        }
    ```


>>>>>>> 029f287ac45de6365e8793ef3b3f7955b77a5379
