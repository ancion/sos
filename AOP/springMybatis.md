# mybatis

### Spring integrate with Dao Framework

```
    JDBC--
        |-JDBCTemplate
    Hibernate-->(JPA)
        |-HibernateTemplate
    Mybatis
        |-SqlSessionFactory
```
## Mybatis code step
- 1.POJO entity
- 2.entity TypeAlias
- 3.database-table
- 4.Dao interface
- 5.mapper.xml file
- 6.registry mapper.xml file
- 7.Mybatis API call to create SqlSessionFactory and execute sql 
```java
public class TestMybatis{

    public static void main(String[] args){
        // get configuration file InputStream 
        InputStream in = Resource.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory sqlFactory = new SqlSessionFactoryBuilder().build(in);
        
        // get sqlSession
        SqlSession session = sqlFactory.openSession();
        *Dao *dao = (*Dao)session.getMapper(*Dao.class);
        
        // execute sql
        *dao.save();
        
        // commit
        session.commit();
    }
}
```
### Integrate with Spring

 - 1.entity POJO
 - 2.database-table
 - 3.Dao interface
 - 4.Mapper.xml file

| xml                |                        |
| --------           | --------               |
| mybatis-config.xml | applicationContext.xml |
```xml
<Configuration>
    <typeAliases>
        <typeAlias alias='user' type='com.edu.entity.User'/>
        .........

    </typeAliases>
    <!-- dataSource -->
    <environments default='mysql'>
        <environment id='mysql'>
            <transactionManager type='JDBC'></transactionManager>
            <dataSource type='POOLED'>
                <property name='driverClassName' value='com.mysql.jdbc.Driver'>
                <property name='url' value='jdbc:mysql://localhost:3306/.....'>
                <property name='username' value='root'>
                <property name='password' value='root'>
            </dataSource>
        </environment>
    </environments>
    <!-- mapper registry -->
    <mappers>
        <mapper resource='classpath:/mapper/userMapper.xml'></mapper>
    </mappers>
</Configuration>
```

#### applicationContext.xml
+ integrate mybatis-config.xml into spring config file(applicationContext.xml)
```xml
<!-- dataSource -->
<bean id='dataSource' class='com.alibaba.druid.DruidDataSource'>
    <property name='driverClassName' value='com.mysql.jdbc.Driver'>
    <property name='url' value='jdbc:mysql://localhost:3306/.....'>
    <property name='username' value='root'>
    <property name='password' value='root'>
</bean>
<!-- SqlSessionFactory 
        InputStream in = Resource.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactory sqlFactory = new SqlSessionFactoryBuilder().build(in);
-->
<bean id='sqlSessionFactoryBean' class='org.mybatis.spring.SqlSessionFactoryBuilderBean'>
    <property name='dataSource' ref='dataSource'/>
    <property name='typeAliasesPackage' value='com.edu.entity'/>
    <property name='mapperLocation' value='classpath:/mapper/*.xml'/>
</bean>
<!-- MapperScannerConfigurer 
        SqlSession session = sqlFactory.openSession();
        *Dao *dao = (*Dao)session.getMapper(*Dao.class);
-->
<bean id='scanner' class='org.mybatis.spring.mapper.MapperScannerConfigurer'>
    <property name='sqlSessionFactoryBeanName' value="sqlSessionFactoryBean" />
    <property name='basePackage' value='com.edu.dao'/>
</bean>
```
**Atention:** 
- default:mybatis's own dataSource  Connection.setAutoCommit(false)
- druidDataSource or C4p0 don't change the setAutoCommit(true),will commit transaction
automatic.
