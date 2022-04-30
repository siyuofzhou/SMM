### 1.4 抽取jdbc配置文件

applicationContext.xml加载jdbc.properties配置文件获得连接信息。

首先，需要引入context命名空间和约束路径：

- 命名空间：xmlns:context="http://www.springframerwork.org/schema/context"

- 约束路径：

  - http:www.springframework.org/schema/context
  - http:www.springframework.org/schema/context/spring-context.xsd

  ```xml
  <context:property-placeholder location="classpath:jdbc.properties">
  <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
  	<property name="driverClass" value="${jdbc.driver}"/>
  	<property name="jdbcUrl" value="${jdbc.url}"/>
  	<property name="user" value="${jdbc.username}"/>
  	<property name="password" value="${jdbc.password}"/>
  </bean>
  ```

  

