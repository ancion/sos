## Yml file

+ can express Object type and list collections
+ can express hierarchy relationship

```yml
   # 1、file type
    ***.yml  /   ***.yaml
   # 2、
   #basic key-value
   usename: xiaofang
   password: 123456
   # Object type
   account:
     id: 121222
     sum: 89890
   # list
    aliase:
      - 1111
      - 2222
    

```
### How integrate `yml` with spring
+ spring  default not support `yml` fileType 

```java
    @Bean
    public PropertySourcePlaceholderConfigurer configur(){
        YmlPropertiesFactoryBean factorybean = 
            new YmlPropertiesFactoryBean();
        factorybean.setResource(new ClassPathResource("init.yml"));
        Properties properties = YmlPropertiesFactoryBean.getObject();

        PropertySourcePlaceholderConfigurer configur = 
            new PropertySourcePlaceholderConfigurer();
        configur.setPropeties(properties);
        return configur;
    }

```
#### Problems

+ collection only handle by el expression
```
in yml
list: 111,222,333....
```

```java
@Value("#{'${list}'.split(',')}")

```
+ Object is configed is too complicated
```
    @Value("account.name")

```
**Note:** 

+ in springBoot use `@ConfigurationProperties` can solve all this problemes.

list a demo
add a line
add a new line 

