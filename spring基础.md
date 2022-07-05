## 数据库文件配置

- **JAVA/second_class/test01**

##### 1. 在db.properties文件中配置postgresql相关信息

```properties
jdbc.driverClass=org.postgresql.Driver
jdbc.jdbcUrl=jdbc:postgresql://localhost/spring
jdbc.user=postgres
jdbc.password=123456
```

##### 1.2 在maven中的pom文件中加入jdbc和postgresql的依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.3.3</version>
</dependency>
```

数据库连接方法有两种方式：

##### 2.1 使用spring自带的jdbc和数据库连接方法，在applicationContext.xml中配置bean

```xml
<!--    spring自带的jdbc数据库连接方法-->
<bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="org.postgresql.Driver"></property>
    <property name="url" value="jdbc:postgresql://localhost/spring"></property>
    <property name="username" value="postgres"></property>
    <property name="password" value="123456"></property>
</bean>
```

##### 2.2 若使用c3p0连接方法(推荐)则在maven中加入c3p0的依赖

```xml
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1</version>
</dependency>
```

然后在applicationContext.xml中配置c3p0文件

```xml
<bean id="datasource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="user" value="${jdbc.user}"/>
    <property name="password" value="${jdbc.password}"/>
    <property name="jdbcUrl" value="${jdbc.jdbcUrl}"/>
    <property name="driverClass" value="${jdbc.driverClass}"/>
</bean>
```

##### 2.3 在applicationContext.xml中配置jdbc模板配置和事务配置

```xml
<!--    jdbc模板配置-->
    <bean id="jdbcTemolate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="datasource"></property>
    </bean>

<!--    事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"></property>
    </bean>
```

3 编写工具类和主函数

```java
//Test01Application.java
package com.example.test01;

import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.TransactionStatus;
import org.springframework.transaction.support.DefaultTransactionDefinition;

import java.util.List;

@SpringBootApplication
public class Test01Application {

    public static void main(String[] args) {
        ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");

        JdbcTemplate jdbcTemplate = (JdbcTemplate) ac.getBean("jdbcTemolate");

        DataSourceTransactionManager transactionManager = 
            (DataSourceTransactionManager) ac.getBean("transactionManager");


        //创建事务的定义类
        DefaultTransactionDefinition definition = new DefaultTransactionDefinition();

        //定义事务的传播行为和隔离级别
        definition.setPropagationBehavior(DefaultTransactionDefinition.PROPAGATION_REQUIRED);
        definition.setIsolationLevel(DefaultTransactionDefinition.ISOLATION_DEFAULT);

        //获得事务状态
        TransactionStatus status = transactionManager.getTransaction(definition);

        //提交事务状态
        transactionManager.commit(status);

        //更新数据
        String sql = "update test set text=? where id=?";
        int count = jdbcTemplate.update(sql,"ccccccc", 1);
        System.out.println(count);

        //查找取出一条数据
        RowMapper<Employee> rowMapper = new BeanPropertyRowMapper<Employee>(Employee.class);
        Employee emp = jdbcTemplate.queryForObject("select text from test where id=1", rowMapper);
        System.out.println(emp.getText());

        //查找取出多条数据
        List<Employee> empList = jdbcTemplate.query("select * from test", rowMapper);
        for(Employee e:empList){
            System.out.println(e.getText() + " " + e.getId());
        }
    }
}
```

```java
//Emplyee.java
package com.example.test01;

public class Employee {

    private Integer id;
    private String text;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getText() {
        return text;
    }

    public void setText(String text) {
        this.text = text;
    }
}
```