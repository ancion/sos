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
----------------------------------------------------------------------------------------
# reuse a defined key 

   name: ${account.id}

# use a random 
   id = ${random.int} # use a random int number
   id = ${random.long}# use a random long number 
   id = ${random.long(200,300)} # use a range (200,300) random long
   id = ${random.uuid} # use random uuid
   id = ${random.value} # use random uuid without ‘-’

# use a default value 

  port: ${PORT:9088} # find $PORT. if not exists,use 9088 as default

# "" and ''
# '' means output as same, "" can explain you expression

# 'family \n name'  ==>  family \n name 
# "family \n name"  ==>  family 
#                        name

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

### Two ways to load value from *.yml file

- @Value("${username}")

- @ConfigurationProperties("prefix")
    + Object should have a getter and setter
    + Object data structure same as `prefix` referenced data structure 

**note:**
- yml mapping Object Field 
    + family-name = familyName = family_name 
    

#### Validate 

- JSR303(规范)
- Hibernate-validator(实现)

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.2.4.Final</version>
</dependency>
```

```java
/* 在需要注入值得字段上需要注解 hibernate-validator 中对应得注解*/
@Min(32)
private Integer age;

@Email(message="请输入正确邮箱")
private String email;

.....
@Pattern(regexp='[a-zA-Z0-9_%+-]+@[a-z0-9.-]{2.4}')

@size(min=6, max=10)

@NotNull  

....

```

#### load other properties as in java code 

# import top on class

```
    @PropertySource(value="classpath:redis.properties",encoding="utf-8")
```








#### Problems

+ collection only handle by el expression
```
in yml
list: 111,222,333....
```
2. SPEL 表达式用法

```

    # employ.age={one: '27', two: '35', three: '68', four: '56'}


    @Value("#{${list}[0]}")

    @Value("#{${employ.age}.two}") == @Value("#{${employ.age}['two']}") # 配置对象的属性

    @Value("#{${employ.age}['five']?:31}") # 取值失败使用默认值

    @Value("#{systemProperties}['java.home']") # 取系统环境变量


    @Value("#{'${list}'.split(',')}")

```
+ Object is configed is too complicated
```java
    
    @Value("account.name")

```
**Note:** 

+ in springBoot use `@ConfigurationProperties` can solve all this problemes.


#### 对配置文件中的敏感信息进行加密

1. 添加相关依赖
```xml
<dependency>
    <groupId>com.github.ulisesbocchio</groupId>
    <artifactId>jasypt-spring-boot-starter</artifactId>
    <version>1.18</version>
</dependency>
```
2. jar包位置执行加密的脚本，输入对应需要加密的内容和密码

+ [加密脚本](encrypt.bat) 

3. 将加蜜好的密文和密钥配置好，Spring启动时会自动解密。

```yml
    # 加密后的密文使用 ENC() 包裹
    username: ENC()
    password: ENC()

    # 配置在配置文件中（不安全）
    jasypt:
        encripy:
            password: Unicorn

    # 启动jar 包的时候传递解密的密钥
    java.jar boot-launch-1.0-jar --jasypt.encryptor.password=Unicorn
```



