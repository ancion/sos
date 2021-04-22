## Transaction With Spring

- Transaction controled by connection
- Transaction achieved accoring to AOP
```java
    //core class 
    import org.springframework.jdbc.datasource.DataSoureTransactionManager

    /* 
    * transaction controled by connection, connection exists in dataSourcePool
    *  so need inject datasource into DataSoureTransactionManager firstly.
    */
    
```
### @Transaction 
- tag on class   ------> means all method in class add transaction manager
- tag on method  ------> means this method add transaction manager

```xml
    <bean id='dataSource' class='**.**'>

    </bean>
    <bean id='transactionManager' class='org.springframework.jdbc.datasource.DataSoureTransactionManager'>
        <property name='datasource' ref='dataSource'/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
```
### Transaction attribute

+ isolation (此属性主要解决的是事务的并发问题)
    - 事务并发中可能出现的问题：
        + 脏读，（读取了未提交的数据）
        + 不可重复读，（一个事务中多次读取结果不一致，解决方式是行锁）
        + 幻读 （争对单个表的多次统计操作数据不一致，解决方式是表级锁）
    - Isolation.COMMITED (mysql,oracle 都是支持的)
        + 
    - Isolation.REPEATABLE_READ
        + (mysql支持，oracle 不支持，但是自身可通过版本比对的方式实现此功能)
    - Isolation.SERIALIZABLE (mysql,oracle 都是支持的)
        + 串行化操作，最为安全，并发效率是最低的，实际操作就是操作时将表锁起来，单次一个用户访问，无并发
    - Isolation.DEFAULT
        + differ in different database, spring 整合的时候自动根据数据自己的默认值变化
        + mysql 默认的是repeatable, 查看的命令是 'select @@tx_isoaation'
        + oracle 默认的是commited
        + 实际使用的时候，默认的隔离级别已经可以应付绝大部分的场景，针对并发场景
          应该使用对应的乐观锁计数实现，不会提高数据库事务的隔离级别，损害性能。
+ propagation (此属主要解决的是事务的嵌套问题)
    - Propagation.REQUIRED(default)
        + 外部没有事务的时候，开启事务；外部有事务，取消自己的事务，融入外部事务
        + 适用于增，删，改操作，是@Transational 注解的默认值
    - Propagation.SUPPORTS
        + 外部没有事务的时候，不开启事务；外部有事务的时候，直接融入事务
        + 适用于读 操作，
    - Propagation.REQUIRED_NEW  --->(like log record)
        + 外部没有事务，开启事务；外部有事务，挂起外部事务，开启新事务自己事务结束后恢复外部事务
        + 应用在一些特殊的场景，比如业务日志记录
    - Propagation.NOT_SUPPORTED
        + 不开启事务，挂起外部事务，
    - Propagation.NEVER
        + 不开启事务，外部有事务，抛出异常
    - Propagation.MANDATORY
        + 抛出异常，融合到外部事务中
+ readonly (主要应用于查询操作的效率问题，可以使得事务的串行化变成并行化)
    - true
    - false(default)
+ timeout(s)
    - -1   (default--->How long actually decided by database)
+ rollbackFor
    - default (only rollback for RuntimeException(or inherited RuntimeException))
    - rollbackFor={java.lang.Exception.class,...}
+ norollbackFor
    - default (commit for Exception(or inherited Exception))
    - noRollbackFor={java.lang.RuntimeException.class,...}
```
@Transaction(isolation=,propagation=,timeout=,rollbackFor=,noRollbackFor=)
```
**Advisation:** 
- update delete insert ==> just @Transaction
- query ==> @Transaction(propagation=Propagation.SUPPORTS,readonly=true)





