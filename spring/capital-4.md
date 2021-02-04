# 第四章 spring构造方法赋值

 - 开发步骤
 1. 需要构建的类(提供必要的构造方法)
 2. 配置文件 
 3. 对象的使用

 ```xml
    <!-- 注意，构造方法来创建对象的时候，必须要注意的是，
        内部constructor-arg 标签的数量，顺序必须与代码
        中提供的数量，顺序要一致
    -->
    <bean>
        <constructor-arg>
            <value>name</value>
        </constructor-arg>
        <constructor-arg>
            <value>age</value>
        </constructor-arg>
    </bean>

 ```

**注意**
  - 放构造参数不同的时候可以通过 constructor-arg 标签数量进行区分
  - 当构造方法中参数相同，类型不同的时候，需要添加区分属性的类型，
  否则可能会造成程序执行语义变化
```
   <constructor-arg type="int"></constructor-arg>

```
