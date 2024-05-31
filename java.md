#  类变量

# 方法区、永久代、元空间的区别

- 方法区， 是 《JVM 规范》 定义的，所有虚拟机必须有的。

- `PermGen space` 则是 HotSpot 虚拟机基于《 JVM 规范》 对 `方法区` 的一个落地实现。

- 针对 HotSpot 虚拟机 ，
  JDK7及之前， `PermGen space` 就是 方法区。
  [JDK8](https://so.csdn.net/so/search?q=JDK8&spm=1001.2101.3001.7020)及之后， `PermGen space` 被移除， 换成 `Metaspace`（元空间），也是对 `方法区` 的新的实现

  

# 异常

## 	处理NullPointerException

​			使用空字符串`""`而不是默认的`null`可避免很多`NullPointerException`，编写业务逻辑时，用空字符串`""`表示未填写比`null`安全得多

## 	断言

​			Java断言的特点是：断言失败时会抛出`AssertionError`，导致程序结束退出。因此，断言不能用于可恢复的程序错误，只应该用于开发和测试阶段。

# jdk8和jdk9新特性

- 接口中声明的静态方法只能通过接口调用
- 接口中声明的默认方法可以通过普通方法实现接口调用
- 类实现了两个接口，而两个接口中定义了同名同参数的默认方法 ，则实现类在没有重写此两个接口的情况下----》会接口冲突
- 类优先原则—— 子类继承了父类，并实现了接口，父类和接口中声明了同名同参数的方法（默认情况下，子类在没有重写此方法的情况下，调用的是父类中的方法；
- jdk9 ----接口中可以定义私有方法

# 反射

## 	**反射是框架设计的灵魂**

​	•JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

# Maven

## 	如何配置本工程坐标信息

​		

```xml
<!--怎么配置本地工程的坐标信息
	项目：
		groupId: 域名反过来  com.baidu
		artifactId:  项目名：OA
	模块： OA(前台、后台、common)
		groupId:域名反过来  com.baidu.oa
   		artifactID: 模块名
-->
```

```xml
<!--
	设置打包方式 默认为jar包 (聚合工程pom表示不是一个具体包，管理子项目)
 <packaging>war</packaging>
-->
```

<img src="C:\Users\20916\AppData\Roaming\Typora\typora-user-images\image-20240320203509201.png" alt="image-20240320203509201" style="zoom:50%;" />

## 依赖传递

​	作用访问是provided和test不会传递

​	

```xml
<!--
<optional>false</optional>表示不传递
-->
```

```xml
<!-- 排除依赖(添加依赖的不同版本也会覆盖(排除))
<exclusions>
	<exclusion>
  		<groupId></groupId>
  		<artifactId></artifactId>
	</exclusion>
</exclusions>
-->  

```

## 聚合工程

聚合工程pom表示不是一个具体包，管理子项目)
 <packaging>pom</packaging>

模块modules

```xml
<modules>
	<module>Projectname</module>
</modules>
```

## maven继承

​	当采用继承方式groupId,version都会继承父maven

```xml
<parent>
	<groupId></groupId>
  	<artifactId></artifactId>
    <version></version>
</parent>
```

​	若在父工程中添加的依赖不是全部子工程所需要到(dependencyManagement屏蔽，在需要的项目的xml配置文件中单独覆写，表示继承此依赖（版本自动继承）)

```xml

<dependencyManagement>
	<dependencies>
    	<dependency>
            
        </dependency>
    </dependencies>
<dependencyManagement>

```

## 版本统一管理

​	

```xml
<properties>
	<依赖名>版本号</依赖名>
</properties>
```

再来从dependency中用${}来代替

# Spring

## 	在resource中通过xml文件创建对象及其分析

​		

```xml
<!--在resource中通过xml文件创建User类对象-->
	<bean id="user" class="com.llc.User(类全路径)"></bean>
<!--然后在从测试类中进行调用-->

```

```java
package com.llc;
import org.junit.jupiter.api.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestUser {
    @Test
    public void TestUser(){
        //加载spring配置文件，对象创建
        ApplicationContext context=new ClassPathXmlApplicationContext("bean.xml");
        //获取创建对象
        User usr=(User)context.getBean("user");
        System.out.println("1"+usr);
        //调用方法
        System.out.println("2");
        usr.add();
    }
}

```

### 分析

​	原理：反射

​	过程：1加载bean.xml配置文件

​				2  对xml文件进行解析操作 

​				3获取xml文件bean标签属性值 id属性值和class属性值

​				4使用反射根据类全路径创建对象

​				

```java
    //反射创建对像
    @Test
    public void testUserproject1() throws Exception {
        //获取类的字节码文件
        Class<?> clazz=Class.forName("com.llc.User");
        //调用方法出创建对象
//        Object o=clazz.newInstance();
        User user = (User) clazz.getDeclaredConstructor().newInstance();
        System.out.println(user);
    }
```

​			
