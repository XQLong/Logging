## Spring概述

### 一、Spring是什么

Spring是一个开源框架，为例解决企业应用开发的复杂性而创建的。

是一个轻量级的控制反转（IOC）和面向编程（AOP）的容器框架
- 从大小与开销两方面而言Spring都是轻量级的
- 通过控制反转（IOC）的技术达到松耦合的目的
- 提供了面向切面编程的丰富支持，允许通过分离应用业务的业务逻与系统服务进行内聚性的开发
- 包含并管理应用对象的配置和生命周期
- 将简单的组件配置、组合成为复杂的应用，这个意义上是一个框架

### 二、Spring架构

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566025647980-1566025648383.png)

### 三、Spring作用

- 容器
- 提供了多种技术的支持：JMS、MQ支持、UnitTest...
- AOP(事物管理、日志等)
- 提供了众多方便应用的辅助类（JDBC Template等）
- 对主流应用框架（Hibernate等）提供了良好的支持

### 四、IOC

#### 接口及面向接口编程

#### 什么是IoC

控制反转（IOC）：控制权的转移，应用程序本身不负责依赖对象的创建和维护，而是由外部容器负责创建和维护。
依赖注入（DI）：是控制反转的一种实现方式，目的是创建对象并且组装对象之间的关系。就是有IOC容器在运行期间，动态地将某种依赖注入到对象当中。

#### Spring注入

Spring注入是指在启动Spring容器加载Beans的时候，完成对变量的赋值行为。常用的注入方式有两种：

![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566027508169-1566027508177.png)


![title](https://raw.githubusercontent.com/XQLong/Image-Hosting/master/gitnote/2019/08/17/1566027527480-1566027527485.png)

- 自动装配（Autowiring）


### AOP

AOP：Aspect Oriented Programming的缩写，意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。日志记录，性能统计，安全控制，事物处理，异常处理等。

实现方式：
- 预编译，Aspect
- 运行期动态代理（JDK动态代理、CGLib动态代理）SPringAOP，JBossAOP







