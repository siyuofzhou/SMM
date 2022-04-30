### 1.1 Bean的依赖注入概念

依赖注入（Dependency Injection）:它是Spring 框架核心IOC的具体实现。

在编写程序时，通过控制依赖反转，把对象的创建交给Spring，但是代码中不可能出现没有依赖的情况。IOC解耦只是降低他们的依赖关系，但不会消除。例如：业务层仍会调用持久层的方法。

这种业务层和持久层的依赖关系，在使用Spring之后，就让Spring来维护了。简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去取。

### 1.2Bean的依赖注入方式

- #### 构造方法

  1. ###### 类中定义构造方法

     ```java
     public UserServiceImpl(UserDao userDao1){
                 this.userDao = userDao1;
         }
     ```

  2. ###### 配置文件中通过`constructor-arg`传入参数对象

     ```xml
     <bean id="userDao" class="com.siyu.impl.UserDaoImpl" scope="singleton"></bean>
     
     <bean id="userService" class="com.siyu.impl.UserServiceImpl">
        <constructor-arg name="userDao1" ref="userDao"></constructor-arg>
     </bean>
     ```

     `constructor-arg`中设置两个属性，

     - `name`设置为构造函数中参数的名称，注意不是参数的类型

     - `ref`设置为传入对象的引用，这里设置为bean对象的id `userDao`

- #### set方法

  1. ###### 类中定义set方法

     ```java
     public class UserServiceImpl implements UserService {
         private X var1;
         public void setVar1(X var1) {
         this.var1 = var1;
         }
         ...
     }
     ```

  2. ###### 配置文件中通过`property`设置传入参数对象

     ```xml
     <bean id="xml_var1" class="com.siyu.impl.UserDaoImpl" scope="singleton"></bean>
     
     <bean id="userService" class="com.siyu.impl.UserServiceImpl">
        <property name="var1" ref="xml_var1"></property>
     </bean>
     ```

     `property`中设置两个属性，

     - `name`设置为set方法名称“setxxxx”中“xxxx”的部分，并且该部分首字母不区分大小写，其他位置区分大小写。

       > 注意：“xxxx”部分的内容不一定就是待设置变量的名称。由于set方法名通常把"xxxx"设置成首字母大写的变量名称，所以会造出是和变量名相同的假象。但name的设置值和变量名没有直接关联。

     - `ref`设置为传入对象的引用，这里设置为bean对象的id `xml_var1`

  3. ###### p命名空间注入

     P命名空间注入本质也是set方法注入，但比起上述的`property`方式注入更加方便，主要体现在配置文件中，如下：

     首先引入P命名空间：

     ```xml
     xmlns:p="http://www.springframework.org/schema/p"xxxxxxxxxx xmlns:p="httxmlns:p="http://www.springframework.org/schema/p""
     ```

     其次，修改注入方式

     ```xml
     <bean id="userService" class="com.siyu.impl.UserServiceImpl" p:userDao-ref="userDao"/>
     ```

### 1.3 Bean的依赖注入的数据类型

Bean依赖注入有三种数据类型

1. 普通数据类型

   ```xml
   <property name="username" value="zhangsan"></property>
   <property name="age" value="18"></property>
   ```

2. 应用数据类型

   ```xml
   <property name="userDao" ref="userDao"></property>
   ```

3. 集合数据类型

   ```xml
   <property name="strList">
       <list>
           <value>aaa</value>
           <value>bbb</value>
           <value>ccc</value>
       </list>
   </property>
   <property name="userMap">
       <map>
           <entry key="key1" value-ref="user1"></entry>
           <entry key="key2" value-ref="user2"></entry>
       </map>
   </property>
   <property name="properties">
       <props>
           <prop key="p1">ppp1</prop>
           <prop key="p2">ppp2</prop>
       </props>
   </property>
   ```

   