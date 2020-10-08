## Transaction With Spring

- Transaction controled by connection
- Transaction achieved accoring to AOP
```java
    //core class 
    import org.springframework.jdbc.datasource.DataSoureTransactionManager

    /* transaction controled by connection, connection exists in dataSourcePool
    *  so must inject datasource into DataSoureTransactionManager firstly.
    */
    
```
### @Transaction 
- tag on class   ------> means all method in class add transaction manager
- tag on method  ------> means this method add transaction manager

```xml
    <bean id='dataSource'class='**.**'>

    </bean>
    <bean id='transactionManager' class='org.springframework.jdbc.datasource.DataSoureTransactionManager'>
        <property name='datasource' ref='dataSource'/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
```
### Transaction attribute

+ isolation
    - Isolation.COMMITED
    - Isolation.REPEATABLE
    - Isolation.SERIALIZABLE
    - Isolation.DEFAULT(differ in different database)
+ propagation
    - Propagation.REQUIRED(default)  
    - Propagation.SUPPORTS
    - Propagation.REQUIRED_NEW  --->(like log record)
    - Propagation.NOT_SUPPORTED
    - Propagation.NEVER
    - Propagation.MANDATORY
+ readonly
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
**Advization:** 
- update delete insert ==> just @Transaction
- query ==> @Transaction(propagation=Propagation.SUPPORTS,readonly=true)





