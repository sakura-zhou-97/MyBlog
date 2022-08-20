# Spring

## IOC和AOP

### IOC（Inverse of Control）

> 控制反转就是把创建和管理 bean 的过程转移给了Spring IoC Container，对于 IoC 来说，最重要的就是**容器**。
>
> 容器负责创建、配置和管理 bean，也就是它管理着 bean 的生命，控制着 bean 的依赖注入。

因为项目中每次创建对象是很麻烦的，所以我们使用 Spring IoC 容器来管理这些对象，需要的时候你就直接用，不用管它是怎么来的、什么时候要销毁，只管用就好了。

Bean 其实就是包装了的 Object，无论是控制反转还是依赖注入，它们的主语都是 object，而 bean 就是由第三方包装好了的 object。

#### IoC 容器

那么 Spring 如何设计容器的呢？

使用 `ApplicationContext`，它是 `BeanFactory` 的子类，更好的补充并实现了 `BeanFactory` 的。

`BeanFactory` 简单粗暴，可以理解为 HashMap：

- Key - bean name
- Value - bean object

但它一般只有 get, put 两个功能，所以称之为“低级容器”。

而 `ApplicationContext` 多了很多功能，因为它继承了多个接口，可称之为“高级容器”。

`ApplicationContext` 的里面有两个具体的实现子类，用来读取配置配件的：

- `ClassPathXmlApplicationContext` - 从 class path 中加载配置文件，更常用一些；
- `FileSystemXmlApplicationContext` - 从本地文件中加载配置文件，不是很常用

当我们点开 `ClassPathXmlApplicationContext` 时，发现它并不是直接继承 `ApplicationContext` 的，它有很多层的依赖关系，每层的子类都是对父类的补充实现。

而再往上找，发现最上层的 class 回到了 `BeanFactory`，所以它非常重要。

#### 深入理解 IoC

#### 几个关键问题：

**何为控制，控制的是什么？**

```
答：是 bean 的创建、管理的权利，控制 bean 的整个生命周期。
```

**何为反转，反转了什么？**

```
答：把这个权利交给了 Spring 容器，而不是自己去控制，就是反转。由之前的自己主动创建对象，变成现在被动接收别人给我们的对象的过程，这就是反转。
```

#### 依赖注入

`IoC is also known as DI`.

 `dependency injection` - 依赖注入。

**何为依赖，依赖什么？**

```
程序运行需要依赖外部的资源，提供程序内对象的所需要的数据、资源。
```

**何为注入，注入什么？**

配置文件把资源从外部注入到内部，容器加载了外部的文件、对象、数据，然后把这些资源注入给程序内的对象，维护了程序内外对象之间的依赖关系。

所以说，控制反转是通过依赖注入实现的。它们是有差别的，像是`「从不同角度描述的同一件事」`：

> - IoC 是设计思想，DI 是具体的实现方式；
> - IoC 是理论，DI 是实践；
>
> **当然，IoC 也可以通过其他的方式来实现，而 DI 只是 Spring 的选择。**

IoC 和 DI 也并非 Spring 框架提出来的，Spring 只是应用了这个设计思想和理念到自己的框架里去。

#### 为何

答：解耦。

它把对象之间的依赖关系转成用配置文件来管理，由 Spring IoC Container 来管理。

在项目中，底层的实现都是由很多个对象组成的，对象之间彼此合作实现项目的业务逻辑。但是，很多很多对象紧密结合在一起，一旦有一方出问题了，必然会对其他对象有所影响，所以才有了解耦的这种设计思想。

![image-20210314220604179](picture/框架.assets/image-20210314220604179.png)

如上图所示，本来 ABCD 是互相关联在一起的，当加入第三方容器的管理之后，每个对象都和第三方法的 IoC 容器关联，彼此之间不再直接联系在一起了，没有了耦合关系，全部对象都交由容器来控制，降低了这些对象的亲密度，就叫“解耦”。

#### IOC使用的技术？

>  bean是如何创建---  工厂模式
>
> 数据是如何注入-------反射

### AOP（Aspect Oriented Programming）

> 面向切面编程，在我们的应用中，经常需要做一些事情，但是这些事情与核心业务无关，比如，要记录所有update方法的执行时间时间，操作人等等信息，记录到日志，
>
> 通过spring的AOP技术，就可以在不修改update的代码的情况下完成该需求。
>
> 只需要专注核心业务

## SpringBoot注解

### 1、@SpringBootApplication

> 这是 Spring Boot 最最最核心的注解，用在 Spring Boot 主类上，标识这是一个 Spring Boot 应用，用来开启 Spring Boot 的各项能力。
>
> 其实这个注解就是 @SpringBootConfiguration、@EnableAutoConfiguration、@ComponentScan 这三个注解的组合，也可以用这三个注解来代替 @SpringBootApplication 注解。

### 2、@EnableAutoConfiguration

> 允许 Spring Boot 自动配置注解，开启这个注解之后，Spring Boot 就能根据当前类路径下的包或者类来配置 Spring Bean。
>
> 如：当前类路径下有 Mybatis 这个 JAR 包，MybatisAutoConfiguration 注解就能根据相关参数来配置 Mybatis 的各个 Spring Bean。

### 3、@Configuration

> 这是 Spring 3.0 添加的一个注解，用来代替 applicationContext.xml 配置文件，所有这个配置文件里面能做到的事情都可以通过这个注解所在类来进行注册。

### 4、@SpringBootConfiguration

> 这个注解就是 @Configuration 注解的变体，只是用来修饰是 Spring Boot 配置而已，或者可利于 Spring Boot 后续的扩展。

### 5、@ComponentScan

> 用来代替配置文件中的 component-scan 配置，开启组件扫描，即自动扫描包路径下的 @Component 注解进行注册 bean 实例到 context 中。

### 6、@Conditional

> 用来标识一个 Spring Bean 或者 Configuration 配置文件，当满足指定的条件才开启配置。

### 7、@ConditionalOnBean

> 组合 @Conditional 注解，当容器中有指定的 Bean 才开启配置。

### 8、@ConditionalOnMissingBean

> 组合 @Conditional 注解，和 @ConditionalOnBean 注解相反，当容器中没有指定的 Bean 才开启配置。

### 9、@ConditionalOnClass

> 组合 @Conditional 注解，当容器中有指定的 Class 才开启配置。

### 10、@ConditionalOnMissingClass

> 组合 @Conditional 注解，和 @ConditionalOnMissingClass 注解相反，当容器中没有指定的 Class 才开启配置。

### 11、@ConfigurationProperties

> 用来加载额外的配置（如 .properties 文件），可用在 @Configuration 注解类，或者 @Bean 注解方法上面。
>
> ![img](picture/框架.assets/2019-07-24-16-24-34@2x.png)

### 12、@EnableConfigurationProperties

> 一般要配合 @ConfigurationProperties 注解使用，用来开启对 @ConfigurationProperties 注解配置 Bean 的支持。
>
> ![img](picture/框架.assets/2019-07-24-16-29-42@2x.png)

### 13、@AutoConfigureAfter

用在自动配置类上面，表示该自动配置类需要在另外指定的自动配置类配置完之后。

如 Mybatis 的自动配置类，需要在数据源自动配置类之后。

```kotlin
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
public class MybatisAutoConfiguration {
```

### 14、@AutoConfigureBefore

> 这个和 @AutoConfigureAfter 注解使用相反，表示该自动配置类需要在另外指定的自动配置类配置之前。

### 15、@Import

> 用来导入一个或者多个 @Configuration 注解修饰的类

### 16、@ImportResource

> 用来导入一个或者多个 Spring 配置文件，这对 Spring Boot 兼容老项目非常有用，因为有些配置无法通过 Java Config 的形式来配置就只能用这个注解来导入。

## Spring Boot自动配置原理

Spring Boot的出现，得益于“[**习惯优于配置**]()”的理念，这是基于Spring 4.x提供的按条件配置Bean的能力。

Spring Boot的配置文件
初识Spring Boot时我们就知道，Spring Boot有一个全局配置文件：application.properties或application.yml。

我们的各种属性都可以在这个文件中进行配置，最常配置的比如：server.port、logging.level.* 等等

那么问题来了：这些配置是如何在Spring Boot项目中生效的呢？

### 工作原理剖析

Spring Boot关于自动配置的源码在spring-boot-autoconfigure-x.x.x.x.jar中：

![img](picture/框架.assets/20181107144449611.png)

@EnableAutoConfiguration

![img](picture/框架.assets/20181107115331414.png)

![img](picture/框架.assets/20181107125035592.png)

 而这个注解也是一个派生注解，其中的关键功能由@Import提供，[**其导入的AutoConfigurationImportSelector的selectImports()方法通过SpringFactoriesLoader.loadFactoryNames()扫描所有具有META-INF/spring.factories的jar包。**]()spring-boot-autoconfigure-x.x.x.x.jar里就有一个这样的spring.factories文件。

这个spring.factories文件也是一组一组的key=value的形式，[**其中一个key是EnableAutoConfiguration类的全类名，而它的value是一个xxxxAutoConfiguration的类名的列表，这些类名以逗号分隔**]()，如下图所示：

![img](picture/框架.assets/20181107130442565.png)

这个@EnableAutoConfiguration注解通过@SpringBootApplication被间接的标记在了Spring Boot的启动类上。在SpringApplication.run(...)的内部就会执行selectImports()方法，找到所有JavaConfig自动配置类的全限定名对应的class，然后将所有自动配置类加载到Spring容器中。

### 自动配置生效

每一个XxxxAutoConfiguration自动配置类都是在某些条件之下才会生效的，这些条件的限制在Spring Boot中以注解的形式体现，常见的条件注解有如下几项：

> @ConditionalOnBean：当容器里有指定的bean的条件下。
>
> @ConditionalOnMissingBean：当容器里不存在指定bean的条件下。
>
> @ConditionalOnClass：当类路径下有指定类的条件下。
>
> @ConditionalOnMissingClass：当类路径下不存在指定类的条件下。
>
> @ConditionalOnProperty：指定的属性是否有指定的值，比如@ConditionalOnProperties(prefix=”xxx.xxx”, value=”enable”, matchIfMissing=true)，代表当xxx.xxx为enable时条件的布尔值为true，如果没有设置的情况下也为true。
>

以`ServletWebServerFactoryAutoConfiguration`配置类为例，解释一下全局配置文件中的属性如何生效，比如：server.port=8081，是如何生效的（当然不配置也会有默认值，这个默认值来自于org.apache.catalina.startup.Tomcat）。

<img src="picture/框架.assets/20181107133910432.png" alt="img" style="zoom:67%;" />

在`ServletWebServerFactoryAutoConfiguration`类上，有一个`@EnableConfigurationProperties`注解：开启配置属性，而它后面的参数是一个`ServerProperties`类，这就是习惯优于配置的最终落地点。

<img src="picture/框架.assets/20181107134400306.png" alt="img" style="zoom:67%;" />

在这个类上，我们看到了一个非常熟悉的注解：`@ConfigurationProperties`，它的作用就是从配置文件中绑定属性到对应的bean上，而`@EnableConfigurationProperties`负责导入这个已经绑定了属性的bean到spring容器中。

那么所有其他的和这个类相关的属性都可以在全局配置文件中定义，也就是说，真正“限制”我们可以在全局配置文件中配置哪些属性的类就是这些XxxxProperties类，它与配置文件中定义的prefix关键字开头的一组属性是唯一对应的。

至此，我们大致可以了解。

> 1.在全局配置的属性如：server.port等，通过`@ConfigurationProperties`注解，绑定到对应的XxxxProperties配置实体类上封装为一个bean，然后再通过`@EnableConfigurationProperties`注解导入到Spring容器中。
>
> 2.而诸多的XxxxAutoConfiguration自动配置类，就是Spring容器的JavaConfig形式，作用就是为Spring 容器导入bean，而所有导入的bean所需要的属性都通过xxxxProperties的bean来获得。

通过一张图标来理解一下这一繁复的流程：

![img](picture/框架.assets/how-spring-boot-autoconfigure-works.png)

## Spring如何解决循环依赖

### 什么是循环依赖？ 

<img src="picture/框架.assets/20180330094859991" alt="img" style="zoom:50%;" />

### Spring中循环依赖的场景？where？

  ①构造器的循环依赖。（这个Spring解决不了）

StudentA有参构造是StudentB。StudentB的有参构造是StudentC，StudentC的有参构造是StudentA ，这样就产生了一个循环依赖的情况。

   ②setter循环依赖

field属性的循环依赖(setter方式 单例，默认方式-->通过递归方法找出当前Bean所依赖的Bean，然后提前缓存(会放入Cache中)起来。通过提前暴露 -->暴露一个exposedObject用于返回提前暴露的Bean。

setter方式注入：

![img](picture/框架.assets/20180331212327518)

图中前两步骤得知：Spring是先将Bean对象实例化【依赖无参构造函数】--->再设置对象属性的

 这就不会报错了：

原因：Spring先用构造器实例化Bean对象----->将实例化结束的对象放到一个Map中，并且Spring提供获取这个未设置属性的实例化对象的引用方法。结合我们的实例来看，当Spring实例化了StudentA、StudentB、StudentC后，紧接着会去设置对象的属性，此时StudentA依赖StudentB，就会去Map中取出存在里面的单例StudentB对象，以此类推，不会出来循环的问题喽。

### 如何检测是否有循环依赖？

可以 Bean在创建的时候给其打个标记，如果递归调用回来发现正在创建中的话--->即可说明循环依赖。

### 怎么解决的？ 

 Spring的循环依赖的理论依据其实是基于Java的引用传递，当我们获取到对象的引用时，对象的field属性是可以延后设置的(但是构造器必须是在获取引用之前)。

   Spring的单例对象的初始化主要分为三步： 

<img src="picture/框架.assets/20180330100217757" alt="img" style="zoom: 50%;" />


    ①：createBeanInstance：实例化，其实也就是 调用对象的构造方法实例化对象
    
    ②：populateBean：填充属性，这一步主要是多bean的依赖属性进行填充
    
    ③：initializeBean：初始化，调用spring xml中的init() 方法。

从上面讲述的单例bean初始化步骤我们可以知道，循环依赖主要发生在第一、第二步。

那么我们要解决循环引用也应该从初始化过程着手，对于单例来说，在Spring容器整个生命周期内，有且只有一个对象，所以很容易想到这个对象应该存在Cache中，Spring为了解决单例的循环依赖问题，使用了三级缓存。


调整配置文件，将构造函数注入方式改为 属性注入方式 即可

3.源码怎么实现的？ how？

 （1）三级缓存源码主要 指：

```java
/** Cache of singleton objects: bean name --> bean instance */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);
 
/** Cache of singleton factories: bean name --> ObjectFactory */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);
 
/** Cache of early singleton objects: bean name --> bean instance */
private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);
```

这三级缓存分别指：

| 源码                  | 级别     | 描述                                                         |
| --------------------- | -------- | ------------------------------------------------------------ |
| singletonObjects      | 一级缓存 | 用于存放完全初始化好的 bean，从该缓存中取出的 bean 可以直接使用 |
| earlySingletonObjects | 二级缓存 | 存放原始的 bean 对象（尚未填充属性），用于检测循环引用，与singletonFactories互斥 |
| singletonFactories    | 三级缓存 | 存放 bean 工厂对象，主要是生产 Bean，存放到二级缓存中。      |

我们在创建bean的时候，首先想到的是从cache中获取这个单例的bean，这个缓存就是singletonObjects。主要调用方法就是：

```java
protected Object getSingleton(String beanName, boolean allowEarlyReference) {
    Object singletonObject = this.singletonObjects.get(beanName);
    // 第一级缓存为null，且bean正在创建中，也就是没有初始化完成
    if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
        synchronized (this.singletonObjects) {
            // 二级缓存获取bean
            singletonObject = this.earlySingletonObjects.get(beanName);
            // 二级缓存为空，且允许从三级缓存中获取对象
            if (singletonObject == null && allowEarlyReference) {
                ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
                if (singletonFactory != null) {
                    singletonObject = singletonFactory.getObject();
                    this.earlySingletonObjects.put(beanName, singletonObject);
                    this.singletonFactories.remove(beanName);
                }
            }
        }
    }
    return (singletonObject != NULL_OBJECT ? singletonObject : null);
}
```

上面的代码需要解释两个参数：

- isSingletonCurrentlyInCreation()判断当前单例bean是否正在创建中，也就是没有初始化完成(比如A的构造器依赖了B对象所以得先去创建B对象， 或者在A的populateBean过程中依赖了B对象，得先去创建B对象，这时的A就是处于创建中的状态。)
- allowEarlyReference 是否允许从singletonFactories中通过getObject拿到对象

从上面三级缓存的分析，我们可以知道，Spring解决循环依赖的诀窍就在于singletonFactories这个三级cache。这个cache的类型是ObjectFactory，定义如下：

```java
public interface ObjectFactory<T> {
    T getObject() throws BeansException;
}
```

这个接口在下面被引用

```java
protected void addSingletonFactory(String beanName, ObjectFactory<?> singletonFactory) {
    Assert.notNull(singletonFactory, "Singleton factory must not be null");
    synchronized (this.singletonObjects) {
        // 一级缓存不存在
        if (!this.singletonObjects.containsKey(beanName)) {
            // 在三级缓存放入bean
            this.singletonFactories.put(beanName, singletonFactory);
            // 二级缓存删除
            this.earlySingletonObjects.remove(beanName);
            this.registeredSingletons.add(beanName);
        }
    }
}
```


这里就是解决循环依赖的关键，**这段代码发生在createBeanInstance之后**，也就是说单例对象此时已经被创建出来，虽然还不完美（还没有进行第二步和第三步）。所以Spring此时将这个对象提前曝光出来让大家认识，让大家使用。

这样做有什么好处呢？让我们来分析一下“A的某个field或者setter依赖了B的实例对象，同时B的某个field或者setter依赖了A的实例对象”这种循环依赖的情况。

[因为加入singletonFactories三级缓存的前提是执行了构造器，所以构造器的循环依赖没法解决构造器循环依赖]()

### 为什么要使用三级缓存

**解决代理对象（如aop）循环依赖的问题。**

例： a依赖b,b依赖a，同时a,b都被aop增强。

首先明确aop的实现是通过 BeanpostProcess后置处理器，在初始化之后做代理操作的。

为什么使用三级缓存原因：

**1.只使用二级缓存，且二级缓存缓存的是一个不完整的bean**

> 这个时候a在设置属性的过程中去获取b（这个时候a还没有被aop的后置处理器增强），创建b的过程中，b依赖a，b去缓存中拿a拿到的是没有经过代理的a。就有问题。

**2.使用二级缓存，且二级缓存是一个工厂方法的缓存**

> 如果二级缓存是一个工厂的缓存，在从缓存中获取的时候获取到经过aop增强的对象。
>
> <img src="picture/框架.assets/image-20210405121115552.png" alt="image-20210405121115552" style="zoom:25%;" />
>
> a依赖b，b依赖a，c。c又依赖a。
>
> 1. a实例化，放入工厂缓存。设置b。
>
> 2. b实例化，设置属性，拿到a,此时从工厂缓存中拿到代理后的a。由于a没加载完毕，不会放入一级缓存。这个时候b开始设置c。
>
> 3. c实例化，设置属性a,又去工厂缓存中拿对象a。
>
> 这个时候拿到的a和b从工厂缓存不是一个对象。出现问题。

**3.使用二级缓存，二级缓存缓存的是增强后的bean**

这个与spring加载流程不符合。

spring加载流程是：实例化，设置属性，初始化，增强。

在有循环引用的时候，之前的bean并不会增强后放入到二级缓存。

三级缓存的实现

![img](picture/框架.assets/5bcb0897632640ffbb4194ba1c449175~tplv-k3u1fbpfcp-watermark.image)

二级缓存的实现

![img](picture/框架.assets/3a2677f87c904d29ada221b3fe1ea994~tplv-k3u1fbpfcp-watermark.image)

> 上面两个流程的唯一区别在于为A对象创建代理的时机不同，在使用了三级缓存的情况下为A创建代理的时机是在B中需要注入A的时候，而不使用三级缓存的话在**A实例化后就需要马上为A创建代理然后放入到二级缓存中去**。
>
> 上述这种情况下，差别就是在哪里创建代理。如果不用三级缓存，使用二级缓存，违背了Spring在结合`AOP`跟Bean的生命周期的设计！如果出现了循环依赖，那没有办法，只有给Bean先创建代理，但是没有出现循环依赖的情况下，[**设计之初就是让Bean在生命周期的最后一步完成代理而不是在实例化后就立马完成代理。**]()

**综上1，2，3 可知二级缓存解决不了有aop的循环依赖。spring采用了三级缓存。**

## Spring中的Bean作用域类型

>  **singleton** ：在Spring IOC容器中仅存在一个Bean实例，Bean以单实例的方式存在;
>
> **prototype** ：每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行new XxxBean()的操作;
>
> **request：** 每次HTTP请求都会创建一个新的Bean。该作用域仅适用于WebApplicationContext环境.
>
> **session：** 同一个HTTP Session共享一个Bean，不同于HTTP Session使用不同的Bean。该作用域仅使用于WebApplicationContext环境.
>
> **globalSession** ：同一个全局Session共享一个Bean，一般用于Portlet应用环境，该作用域仅适用于WebApplicationContext环境.

## Spring 的 Bean 的加载过程

大致流程：

> 1. 解析需要spring管理的类为beanDefinition
> 2. 通过反射实例化对象
> 3. 反射设置属性
> 4. 初始化，调用initMethod等。（postConstruct也是在这执行）

### 1. 概述

Spring 作为 Ioc 框架，实现了依赖注入，由一个中心化的 Bean 工厂来负责各个 Bean 的实例化和依赖管理。各个 Bean 可以不需要关心各自的复杂的创建过程，达到了很好的解耦效果。

我们对 Spring 的工作流进行一个粗略的概括，主要为两大环节：

- **解析**，读 xml 配置，扫描类文件，从配置或者注解中获取 Bean 的定义信息，注册一些扩展功能。
- **加载**，通过解析完的定义信息获取 Bean 实例。

![img](https:////upload-images.jianshu.io/upload_images/460263-7da93f65daed431a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

Spring总体流程

我们假设所有的配置和扩展类都已经装载到了 ApplicationContext 中，然后具体的分析一下 Bean 的加载流程。

思考一个问题，抛开 Spring 框架的实现，假设我们手头上已经有一套完整的 Bean Definition Map，然后指定一个 beanName 要进行实例化，需要关心什么？即使我们没有 Spring 框架，也需要了解这两方面的知识：

- **作用域**。单例作用域或者原型作用域，单例的话需要全局实例化一次，原型每次创建都需要重新实例化。
- **依赖关系**。一个 Bean 如果有依赖，我们需要初始化依赖，然后进行关联。如果多个 Bean 之间存在着循环依赖，A 依赖 B，B 依赖 C，C 又依赖 A，需要解这种循环依赖问题。

Spring 进行了抽象和封装，使得作用域和依赖关系的配置对开发者透明，我们只需要知道当初在配置里已经明确指定了它的生命周期和依赖了谁，至于是怎么实现的，依赖如何注入，托付给了 Spring 工厂来管理。

Spring 只暴露了很简单的接口给调用者，比如 `getBean` ：

```cpp
ApplicationContext context = new ClassPathXmlApplicationContext("hello.xml");
HelloBean helloBean = (HelloBean) context.getBean("hello");
helloBean.sayHello();
```

那我们就从 `getBean` 方法作为入口，去理解 Spring 加载的流程是怎样的，以及内部对创建信息、作用域、依赖关系等等的处理细节。

### 2. 总体流程

![img](https:////upload-images.jianshu.io/upload_images/460263-74d88a767a80843a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

上面是跟踪了 getBean 的调用链创建的流程图，为了能够很好地理解 Bean 加载流程，省略一些异常、日志和分支处理和一些特殊条件的判断。

从上面的流程图中，可以看到一个 Bean 加载会经历这么几个阶段（用绿色标记）：

- **获取 BeanName**，对传入的 name 进行解析，转化为可以从 Map 中获取到 BeanDefinition 的 bean name。
- **合并 Bean 定义**，对父类的定义进行合并和覆盖，如果父类还有父类，会进行递归合并，以获取完整的 Bean 定义信息。
- **实例化**，使用构造或者工厂方法创建 Bean 实例。
- **属性填充**，寻找并且注入依赖，依赖的 Bean 还会递归调用 `getBean` 方法获取。
- **初始化**，调用自定义的初始化方法。
- **获取最终的 Bean**，如果是 FactoryBean 需要调用 getObject 方法，如果需要类型转换调用 TypeConverter 进行转化。

整个流程最为复杂的是对循环依赖的解决方案，后续会进行重点分析。

### 3. 细节分析

#### 3.1. 转化 BeanName

而在我们解析完配置后创建的 Map，使用的是 beanName 作为 key。见 DefaultListableBeanFactory：

```dart
/** Map of bean definition objects, keyed by bean name */
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<String, BeanDefinition>(256);
```

`BeanFactory.getBean` 中传入的 name，有可能是这几种情况：

- **bean name**，可以直接获取到定义 BeanDefinition。
- **alias name**，别名，需要转化。
- **factorybean name**, **带 `&` 前缀**，通过它获取 BeanDefinition 的时候需要去除 & 前缀。

为了能够获取到正确的 BeanDefinition，需要先对 name 做一个转换，得到 beanName。

<img src="https:////upload-images.jianshu.io/upload_images/460263-ffc69491d898b4e4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom: 50%;" />

name转beanName

见 `AbstractBeanFactory.doGetBean`：

```dart
protected <T> T doGetBean ... {
    ...
    
    // 转化工作 
    final String beanName = transformedBeanName(name);
    ...
}
```

如果是 **alias name**，在解析阶段，alias name 和 bean name 的映射关系被注册到 SimpleAliasRegistry 中。从该注册器中取到 beanName。见 `SimpleAliasRegistry.canonicalName`：

```csharp
public String canonicalName(String name) {
    ...
    resolvedName = this.aliasMap.get(canonicalName);
    ...
}
```

如果是 **factorybean name**，表示这是个工厂 bean，有携带前缀修饰符 `&` 的，直接把前缀去掉。见 `BeanFactoryUtils.transformedBeanName` :

```tsx
public static String transformedBeanName(String name) {
    Assert.notNull(name, "'name' must not be null");
    String beanName = name;
    while (beanName.startsWith(BeanFactory.FACTORY_BEAN_PREFIX)) {
        beanName = beanName.substring(BeanFactory.FACTORY_BEAN_PREFIX.length());
    }
    return beanName;
}
```

#### 3.2. 合并 RootBeanDefinition

我们从配置文件读取到的 BeanDefinition 是 **GenericBeanDefinition**。它记录了一些当前类声明的属性或构造参数，但是对于父类只用了一个 `parentName` 来记录。

```java
public class GenericBeanDefinition extends AbstractBeanDefinition {
    ...
    private String parentName;
    ...
}
```

接下来会发现一个问题，在后续实例化 Bean 的时候，使用的 BeanDefinition 是 **RootBeanDefinition** 类型而非 **GenericBeanDefinition**。这是为什么？

答案很明显，GenericBeanDefinition 在有继承关系的情况下，定义的信息不足：

- 如果不存在继承关系，GenericBeanDefinition 存储的信息是完整的，可以直接转化为 RootBeanDefinition。
- 如果存在继承关系，GenericBeanDefinition 存储的是 **增量信息** 而不是 **全量信息**。

**为了能够正确初始化对象，需要完整的信息才行**。需要递归 **合并父类的定义**：

<img src="https:////upload-images.jianshu.io/upload_images/460263-802f258b1dba771d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom: 50%;" />

合并BeanDefinition

见 `AbstractBeanFactory.doGetBean` ：

```java
protected <T> T doGetBean ... {
    ...
    
    // 合并父类定义
    final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);
        
    ...
        
    // 使用合并后的定义进行实例化
    bean = getObjectForBeanInstance(prototypeInstance, name, beanName, mbd);
        
    ...
}
```

在判断 `parentName` 存在的情况下，说明存在父类定义，启动合并。如果父类还有父类怎么办？递归调用，继续合并。

见`AbstractBeanFactory.getMergedBeanDefinition` 方法：

```java
    protected RootBeanDefinition getMergedBeanDefinition(
            String beanName, BeanDefinition bd, BeanDefinition containingBd)
            throws BeanDefinitionStoreException {

        ...
        
        String parentBeanName = transformedBeanName(bd.getParentName());

        ...
        
        // 递归调用，继续合并父类定义
        pbd = getMergedBeanDefinition(parentBeanName);
        
        ...

        // 使用合并后的完整定义，创建 RootBeanDefinition
        mbd = new RootBeanDefinition(pbd);
        
        // 使用当前定义，对 RootBeanDefinition 进行覆盖
        mbd.overrideFrom(bd);

        ...
        return mbd;
    
    }
```

每次合并完父类定义后，都会调用 `RootBeanDefinition.overrideFrom` 对父类的定义进行覆盖，获取到当前类能够正确实例化的 **全量信息**。

#### 3.3. 处理循环依赖

#### 3.4. 创建实例

获取到完整的 RootBeanDefintion 后，就可以拿这份定义信息来实例具体的 Bean。

具体实例创建见 `AbstractAutowireCapableBeanFactory.createBeanInstance` ，返回 Bean 的包装类 BeanWrapper，一共有三种策略：

- **使用工厂方法创建**，`instantiateUsingFactoryMethod` 。
- **使用有参构造函数创建**，`autowireConstructor`。
- **使用无参构造函数创建**，`instantiateBean`。

使用工厂方法创建，会先使用 getBean 获取工厂类，然后通过参数找到匹配的工厂方法，调用实例化方法实现实例化，具体见`ConstructorResolver.instantiateUsingFactoryMethod` ：

```kotlin
public BeanWrapper instantiateUsingFactoryMethod ... (
    ...
    String factoryBeanName = mbd.getFactoryBeanName();
    ...
    factoryBean = this.beanFactory.getBean(factoryBeanName);
    ...
    // 匹配正确的工厂方法
    ...
    beanInstance = this.beanFactory.getInstantiationStrategy().instantiate(...);
    ...
    bw.setBeanInstance(beanInstance);
    return bw;
}
```

使用有参构造函数创建，整个过程比较复杂，涉及到参数和构造器的匹配。为了找到匹配的构造器，Spring 花了大量的工作，见 `ConstructorResolver.autowireConstructor` ：

```kotlin
public BeanWrapper autowireConstructor ... {
    ...
    Constructor<?> constructorToUse = null;
    ...
    // 匹配构造函数的过程
    ...
    beanInstance = this.beanFactory.getInstantiationStrategy().instantiate(...);
    ...
    bw.setBeanInstance(beanInstance);
    return bw;
}           
```

使用无参构造函数创建是最简单的方式，见 `AbstractAutowireCapableBeanFactory.instantiateBean`:

```cpp
protected BeanWrapper instantiateBean ... {
    ...
    beanInstance = getInstantiationStrategy().instantiate(...);
    ...
    BeanWrapper bw = new BeanWrapperImpl(beanInstance);
    initBeanWrapper(bw);
    return bw;
    ...
}
```

我们发现这三个实例化方式，最后都会走 `getInstantiationStrategy().instantiate(...)`，见实现类 `SimpleInstantiationStrategy.instantiate`：

```kotlin
public Object instantiate ... {
    if (bd.getMethodOverrides().isEmpty()) {
        ...
        return BeanUtils.instantiateClass(constructorToUse);
    }
    else {
        // Must generate CGLIB subclass.
        return instantiateWithMethodInjection(bd, beanName, owner);
    }
}
```

虽然拿到了构造函数，并没有立即实例化。因为用户使用了 replace 和 lookup 的配置方法，用到了动态代理加入对应的逻辑。如果没有的话，直接使用反射来创建实例。

创建实例后，就可以开始注入属性和初始化等操作。

但这里的 Bean 还不是最终的 Bean。返回给调用方使用时，如果是 FactoryBean 的话需要使用 getObject 方法来创建实例。见 `AbstractBeanFactory.getObjectFromBeanInstance` ，会执行到 `doGetObjectFromFactoryBean` ：

```kotlin
private Object doGetObjectFromFactoryBean ... {
    ...
    object = factory.getObject();
    ...
    return object;
}
```

#### 3.5. 注入属性

实例创建完后开始进行属性的注入，如果涉及到外部依赖的实例，会自动检索并关联到该当前实例。

Ioc 思想体现出来了。正是有了这一步操作，Spring 降低了各个类之间的耦合。

属性填充的入口方法在`AbstractAutowireCapableBeanFactory.populateBean`。

```java
protected void populateBean ... {
    PropertyValues pvs = mbd.getPropertyValues();
    
    ...
    // InstantiationAwareBeanPostProcessor 前处理
    for (BeanPostProcessor bp : getBeanPostProcessors()) {
        if (bp instanceof InstantiationAwareBeanPostProcessor) {
            InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
            if (!ibp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
                continueWithPropertyPopulation = false;
                break;
            }
        }
    }
    ...
    
    // 根据名称注入
    if (mbd.getResolvedAutowireMode() == RootBeanDefinition.AUTOWIRE_BY_NAME) {
        autowireByName(beanName, mbd, bw, newPvs);
    }

    // 根据类型注入
    if (mbd.getResolvedAutowireMode() == RootBeanDefinition.AUTOWIRE_BY_TYPE) {
        autowireByType(beanName, mbd, bw, newPvs);
    }

    ... 
    // InstantiationAwareBeanPostProcessor 后处理
    for (BeanPostProcessor bp : getBeanPostProcessors()) {
        if (bp instanceof InstantiationAwareBeanPostProcessor) {
            InstantiationAwareBeanPostProcessor ibp = (InstantiationAwareBeanPostProcessor) bp;
            pvs = ibp.postProcessPropertyValues(pvs, filteredPds, bw.getWrappedInstance(), beanName);
            if (pvs == null) {
                return;
            }
        }
    }
    
    ...
    
    // 应用属性值
    applyPropertyValues(beanName, mbd, bw, pvs);
}
```

可以看到主要的处理环节有：

- 应用 InstantiationAwareBeanPostProcessor 处理器，在属性注入前后进行处理。**假设我们使用了 @Autowire 注解，这里会调用到 AutowiredAnnotationBeanPostProcessor 来对依赖的实例进行检索和注入的**，它是 InstantiationAwareBeanPostProcessor 的子类。
- 根据名称或者类型进行自动注入，存储结果到 PropertyValues 中。
- 应用 PropertyValues，填充到 BeanWrapper。这里在检索依赖实例的引用的时候，会递归调用 `BeanFactory.getBean` 来获得。

#### 3.6. 初始化

##### 3.6.1. 触发 Aware

如果我们的 Bean 需要容器的一些资源该怎么办？比如需要获取到 BeanFactory、ApplicationContext 等等。

Spring 提供了 Aware 系列接口来解决这个问题。比如有这样的 Aware：

- BeanFactoryAware，用来获取 BeanFactory。
- ApplicationContextAware，用来获取 ApplicationContext。
- ResourceLoaderAware，用来获取 ResourceLoaderAware。
- ServletContextAware，用来获取 ServletContext。

Spring 在初始化阶段，如果判断 Bean 实现了这几个接口之一，就会往 Bean 中注入它关心的资源。

见 `AbstractAutowireCapableBeanFactory.invokeAwareMethos` :

```java
private void invokeAwareMethods(final String beanName, final Object bean) {
    if (bean instanceof Aware) {
        if (bean instanceof BeanNameAware) {
            ((BeanNameAware) bean).setBeanName(beanName);
        }
        if (bean instanceof BeanClassLoaderAware) {
            ((BeanClassLoaderAware) bean).setBeanClassLoader(getBeanClassLoader());
        }
        if (bean instanceof BeanFactoryAware) {
            ((BeanFactoryAware) bean).setBeanFactory(AbstractAutowireCapableBeanFactory.this);
        }
    }
}
```

##### 3.6.2. 触发 BeanPostProcessor

在 Bean 的初始化前或者初始化后，我们如果需要进行一些增强操作怎么办？

这些增强操作比如打日志、做校验、属性修改、耗时检测等等。Spring 框架提供了 BeanPostProcessor 来达成这个目标。比如我们使用注解 @Autowire 来声明依赖，就是使用  `AutowiredAnnotationBeanPostProcessor` 来实现依赖的查询和注入的。接口定义如下：



```tsx
public interface BeanPostProcessor {

    // 初始化前调用
    Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;

    // 初始化后调用
    Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;

}
```

**实现该接口的 Bean 都会被 Spring 注册到 beanPostProcessors 中，**见 `AbstractBeanFactory` :

```php
/** BeanPostProcessors to apply in createBean */
private final List<BeanPostProcessor> beanPostProcessors = new ArrayList<BeanPostProcessor>();
```

只要 Bean 实现了 BeanPostProcessor 接口，加载的时候会被 Spring 自动识别这些 Bean，自动注册，非常方便。

然后在 Bean 实例化前后，Spring 会去调用我们已经注册的 beanPostProcessors 把处理器都执行一遍。

```kotlin
public abstract class AbstractAutowireCapableBeanFactory ... {
        
    ...
    
    @Override
    public Object applyBeanPostProcessorsBeforeInitialization ... {

        Object result = existingBean;
        for (BeanPostProcessor beanProcessor : getBeanPostProcessors()) {
            result = beanProcessor.postProcessBeforeInitialization(result, beanName);
            if (result == null) {
                return result;
            }
        }
        return result;
    }

    @Override
    public Object applyBeanPostProcessorsAfterInitialization ... {

        Object result = existingBean;
        for (BeanPostProcessor beanProcessor : getBeanPostProcessors()) {
            result = beanProcessor.postProcessAfterInitialization(result, beanName);
            if (result == null) {
                return result;
            }
        }
        return result;
    }
    
    ...
}
```

这里使用了责任链模式，Bean 会在处理器链中进行传递和处理。当我们调用 `BeanFactory.getBean` 的后，执行到 Bean 的初始化方法 `AbstractAutowireCapableBeanFactory.initializeBean` 会启动这些处理器。

```tsx
protected Object initializeBean ... {   
    ...
    wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
    ...
    // 触发自定义 init 方法
    invokeInitMethods(beanName, wrappedBean, mbd);
    ...
    wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);
    ...
}
```

##### 3.6.3. 触发自定义 init

自定义初始化有两种方式可以选择：

- 实现 InitializingBean。提供了一个很好的机会，在属性设置完成后再加入自己的初始化逻辑。
- 定义 init 方法。自定义的初始化逻辑。

见 `AbstractAutowireCapableBeanFactory.invokeInitMethods` ：

```tsx
    protected void invokeInitMethods ... {

        boolean isInitializingBean = (bean instanceof InitializingBean);
        if (isInitializingBean && (mbd == null || !mbd.isExternallyManagedInitMethod("afterPropertiesSet"))) {
            ...
            
            ((InitializingBean) bean).afterPropertiesSet();
            ...
        }

        if (mbd != null) {
            String initMethodName = mbd.getInitMethodName();
            if (initMethodName != null && !(isInitializingBean && "afterPropertiesSet".equals(initMethodName)) &&
                    !mbd.isExternallyManagedInitMethod(initMethodName)) {
                invokeCustomInitMethod(beanName, bean, mbd);
            }
        }
    }
```

#### 3.7. 类型转换

Bean 已经加载完毕，属性也填充好了，初始化也完成了。

在返回给调用者之前，还留有一个机会对 Bean 实例进行类型的转换。见 `AbstractBeanFactory.doGetBean` ：

```kotlin
protected <T> T doGetBean ... {
    ...
    if (requiredType != null && bean != null && !requiredType.isInstance(bean)) {
        ...
        return getTypeConverter().convertIfNecessary(bean, requiredType);
        ...
    }
    return (T) bean;
}
```

### 4. 总结

抛开一些细节处理和扩展功能，一个 Bean 的创建过程无非是：

获取完整定义 -> 实例化 -> 依赖注入 -> 初始化 -> 类型转换。

作为一个完善的框架，Spring 需要考虑到各种可能性，还需要考虑到接入的扩展性。

所以有了复杂的循环依赖的解决，复杂的有参数构造器的匹配过程，有了 BeanPostProcessor 来对实例化或初始化的 Bean 进行扩展修改。

先有个整体设计的思维，再逐步击破针对这些特殊场景的设计，整个 Bean 加载流程迎刃而解。

## Bean的生命周期

![img](picture/框架.assets/13150128-bb5c9389cd0acc6c.png)

下面的图红框之下是初始化方法前后的操作

![img](file://C:\Users\sakura\Desktop\文档笔记\日常学习\picture\Excellent.assets\1027015-20190722165455745-357028185.png?lastModify=1616572863)

## BeanFactory和ApplicationContext的区别

**描述**

**BeanFactory：**

> 是Spring里面最低层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能；

**ApplicationContext：**

> 应用上下文，继承BeanFactory接口，它是Spring的一各更高级的容器，提供了更多的有用的功能；
>
> 1) 国际化（MessageSource）
>
> 2) 访问资源，如URL和文件（ResourceLoader）
>
> 3) 载入多个（有继承关系）上下文 ，使得每一个上下文都专注于一个特定的层次，比如应用的web层  
>
> 4) 消息发送、响应机制（ApplicationEventPublisher）
>
> 5) AOP（拦截器）

**两者装载bean的区别**

**BeanFactory：**

> BeanFactory在启动的时候不会去实例化Bean，当有从容器中拿Bean的时候才会去实例化；

**ApplicationContext：**

> ApplicationContext在启动的时候就把所有的Bean全部实例化了。它还可以为Bean配置lazy-init=true来让Bean延迟实例化； 

**我们该用BeanFactory还是ApplicationContent**

延迟实例化的优点：（**BeanFactory**）

> 应用启动的时候占用资源很少；对资源要求较高的应用，比较有优势； 

不延迟实例化的优点： （**ApplicationContext**）

> 1. 所有的Bean在启动的时候都加载，系统运行的速度快； 
>
> 2. 在启动的时候所有的Bean都加载了，我们就能在系统启动的时候，尽早的发现系统中的配置问题 
>
> 3. 建议web应用，在启动的时候就把所有的Bean都加载了。（把费时的操作放到系统启动中完成） 

## BeanFactory和FactoryBean的区别

###  1.BeanFactory

​             BeanFactory是IOC最基本的容器，负责生产和管理bean，它为其他具体的IOC容器提供了最基本的规范，例如DefaultListableBeanFactory,XmlBeanFactory,ApplicationContext 等具体的容器都是实现了BeanFactory，再在其基础之上附加了其他的功能。

 BeanFactory源码         

```java
package org.springframework.beans.factory;  
import org.springframework.beans.BeansException;  
public interface BeanFactory {  
    String FACTORY_BEAN_PREFIX = "&";  
    Object getBean(String name) throws BeansException;  
    <T> T getBean(String name, Class<T> requiredType) throws BeansException;  
    <T> T getBean(Class<T> requiredType) throws BeansException;  
    Object getBean(String name, Object... args) throws BeansException;  
    boolean containsBean(String name);  
    boolean isSingleton(String name) throws NoSuchBeanDefinitionException;  
    boolean isPrototype(String name) throws NoSuchBeanDefinitionException;  
    boolean isTypeMatch(String name, Class<?> targetType) throws NoSuchBeanDefinitionException;  
    Class<?> getType(String name) throws NoSuchBeanDefinitionException;  
    String[] getAliases(String name);  
}  
```

### 2.FactoryBean

​             FactoryBean是一个接口，当在IOC容器中的Bean实现了FactoryBean后，通过getBean(String BeanName)获取到的Bean对象并不是FactoryBean的实现类对象，而是这个实现类中的getObject()方法返回的对象。要想获取FactoryBean的实现类，就要getBean(&BeanName)，在BeanName之前加上&。

 FactoryBean源码

```java
package org.springframework.beans.factory;  
public interface FactoryBean<T> {  
    T getObject() throws Exception;  
    Class<?> getObjectType();  
    boolean isSingleton();  
} 
```

下面是一个应用FactoryBean的例子

```xml
 <bean id="student" class="com.spring.bean.Student">  
  <property name="name" value="zhangsan" />  
 </bean>  
 
 <bean id="school" class="com.spring.bean.School">  
 </bean> 
 
 <bean id="factoryBeanPojo" class="com.spring.bean.FactoryBeanPojo">  
    <property name="type" value="student" />
 </bean> 
```

FactoryBean的实现类

```java
public class FactoryBeanPojo implements FactoryBean{
	private String type;
 
	@Override
	public Object getObject() throws Exception {
		if("student".equals(type)){
			return new Student();			
		}else{
			return new School();
		}
		
	}
 
	@Override
	public Class getObjectType() {
		return School.class;
	}
 
	@Override
	public boolean isSingleton() {
		return true;
	}
 
	public String getType() {
		return type;
	}
 
	public void setType(String type) {
		this.type = type;
	}
	
}
```

普通的bean

```java
public class School {
	private String schoolName;
	private String address;
	private int studentNumber;
	public String getSchoolName() {
		return schoolName;
	}
	public void setSchoolName(String schoolName) {
		this.schoolName = schoolName;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public int getStudentNumber() {
		return studentNumber;
	}
	public void setStudentNumber(int studentNumber) {
		this.studentNumber = studentNumber;
	}
	@Override
	public String toString() {
		return "School [schoolName=" + schoolName + ", address=" + address
				+ ", studentNumber=" + studentNumber + "]";
	}
}
```

```java
public class FactoryBeanTest {
	public static void main(String[] args){
		String url = "com/spring/config/BeanConfig.xml";
		ClassPathXmlApplicationContext cpxa = new ClassPathXmlApplicationContext(url);
		Object school=  cpxa.getBean("factoryBeanPojo");
		FactoryBeanPojo factoryBeanPojo= (FactoryBeanPojo) cpxa.getBean("&factoryBeanPojo");
		System.out.println(school.getClass().getName());
		System.out.println(factoryBeanPojo.getClass().getName());
	}
}
```

```
com.spring.bean.Student
com.spring.bean.FactoryBeanPojo
```

> 从结果上可以看到当从IOC容器中获取FactoryBeanPojo对象的时候，用getBean(String BeanName)获取的确是Student对象，可以看到在FactoryBeanPojo中的type属性设置为student的时候，会在getObject()方法中返回Student对象。
>
> 所以说从IOC容器获取实现了FactoryBean的实现类时，返回的却是实现类中的getObject方法返回的对象，要想获取FactoryBean的实现类，得在getBean(String BeanName)中的BeanName之前加上&,写成getBean(String &BeanName)。

### 3.BeanFactory和FactoryBean的区别

​           BeanFactory是提供了IOC容器最基本的形式，提供了实例化和获取bean的方法。

​			FactoryBean可以说为IOC容器中Bean的实现提供了更加灵活的方式，FactoryBean在IOC容器的基础上给Bean的实现加上了一个简单工厂模式和装饰模式，我们可以在getObject()方法中灵活配置。

## Spring事务管理之几种方式实现事务

### 事务的传播特性 

 事务传播行为就是多个事务方法调用时，如何定义方法间事务的传播。Spring定义了7中传播行为：（四种支持（必须有事务，可以非事务，不可以非事务，新事务），两种不支持（挂起事务，抛出异常），一种特殊支持（存在嵌套，不存在就新建））

 1.PROPAGATION_REQUIRED：如果存在一个事务，则支持当前事务。如果没有事务则开启（默认）

> 解释：当A.methodA()和B.methodB()都打上REQUIRED的事务标志，执行A.methodA()方法的时候，看到上下文没有事务，会新建一个事务，当执行到b.methodB()的时候，发现上下文已经有事务了，则不会新建事务，用A.methodA()的那个事务。
>
> 如果b.methodB()执行成功，a.methodA()执行失败，那么b.methodB()和a.methodA()都会回滚（用的都是a.methodA()的事务）
>

2.PROPAGATION_SUPPORTS：如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行

> 解释：当B.methodB()打上PROPAGATION_SUPPORTS的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候，会检查上下文有没有事务。
>
> 如果A.methodA()有事务，则b.methodB()沿用该事务，反之b.methodB()就以非事务的方式执行

3.PROPAGATION_MANDATORY:如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常

> 解释：当B.methodB()打上PROPAGATION_MANDATORY的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候，会检查上下文有没有事务。
>
> 如果A.methodA()有事务，则b.methodB()沿用该事务，如果没有，则会抛出异常

4.PROPAGATION_REQUIRES_NEW：总是开启一个新的事务。如果一个事务已经存在，则将这个存在的事务挂起

> 解释：当B.methodB()打上PROPAGATION_REQUIRES_NEW的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候，会检查上下文有没有事务。
>
> 如果A.methodA()有事务，则会挂起A.methodA()的事务，新建一个属于b.methodB()，当b.methodB()的事务执行结束的时候，则会唤醒b.methodB()的事务。
>
> 和PROPAGATION_REQUIRED的差别在于回滚，当b.methodB()的事务提交后，A.methodA()执行失败，只会回滚A.methodA不会回滚b.methodB()，当b.methodB()执行失败，异常被A.methodA()方法catch到的话，A.methodA()事务不会回滚

5.PROPAGATION_NOT_SUPPORTED：总是非事务地执行，并挂起任何存在的事务

> 解释：当B.methodB()打上PROPAGATION_NOT_SUPPORTED的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候，会检查上下文有没有事务。
>
> 如果A.methodA()有事务，则会挂起A.methodA()的事务，当执行完b.methodB()方法的时候，A.methodA()方法继续以事务的方式执行

6.PROPAGATION_NEVER： 总是非事务地执行，如果存在一个活动事务，则抛出异常

> 解释：当B.methodB()打上PROPAGATION_NEVER的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候，会检查上下文有没有事务。
>
> 如果有事务，则抛出异常，如果没有则以非事务执行

7.PROPAGATION_NESTED：如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, PROPAGATION_REQUIRED 属性执行

> 解释：当B.methodB()打上PROPAGATION_NOT_SUPPORTED的事务标志，执行A.methodA()方法，当执行到b.methodB()的时候。
>
> 如果A.methodA()方法有事务，则会用当前事务，如果 b.methodB()执行失败，只会回滚 b.methodB()，不会回滚A.methodA()，只有当A.methodA()执行完成后才会提交b.methodB()的事务。如果A.methodA()方法没有事务，就会新建一个事务；

### 事务几种实现方式

 （1）编程式事务管理，需要在代码中调用beginTransaction()、commit()、rollback()等事务管理相关的方法

 （2）基于 TransactionProxyFactoryBean的声明式事务管理

 （3）基于 @Transactional 的声明式事务管理

 （4）基于Aspectj AOP配置事务

## Spring Security+JWT

### 什么是JWT

> Json web token (JWT), 是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（[(RFC 7519](https://link.jianshu.com?t=https://tools.ietf.org/html/rfc7519)).该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。
>
> JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。

### 起源

说起JWT，我们应该来谈一谈基于token的认证和传统的session认证的区别。

#### 传统的session认证

我们知道，http协议本身是一种无状态的协议，而这就意味着如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次请求时，用户还要再一次进行用户认证才行，因为根据http协议，我们并不能知道是哪个用户发出的请求，所以为了让我们的应用能识别是哪个用户发出的请求，我们只能在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器，告诉其保存为cookie,以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了,这就是传统的基于session认证。

但是这种基于session的认证使应用本身很难得到扩展，随着不同客户端用户的增加，独立的服务器已无法承载更多的用户，而这时候基于session认证应用的问题就会暴露出来.

#### 基于session认证所显露的问题

**Session**: 每个用户经过我们的应用认证之后，我们的应用都要在服务端做一次记录，以方便用户下次请求的鉴别，通常而言session都是保存在内存中，而随着认证用户的增多，[**服务端的开销会明显增大**]()。

**扩展性**: 用户认证之后，服务端做认证记录，如果认证的记录被保存在内存中的话，**这意味着用户下次请求还必须要请求在这台服务器上,这样才能拿到授权的资源，[这样在分布式的应用上，相应的限制了负载均衡器的能力]()。这也意味着限制了应用的扩展能力。**

**CSRF**: 因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。

> ​    **CSRF攻击攻击原理及过程如下：**
>
> ​    1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；
>
> ​    2.在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；
>
> ​    3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；
>
> ​    4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；
>
>    5.浏览器在接收到这些攻击性代码后，根据网站 B 的请求，在用户不知情的情况下携带 Cookie 信息，向网站 A 发出请求。网站 A 并不知道该请求其实是由 B 发起的，所以会根据用户 C 的 Cookie 信息以 C 的权限处理该请求，导致来自网站 B 的恶意代码被执行。 

#### 基于token的鉴权机制

基于token的鉴权机制类似于http协议也是无状态的，它不需要在服务端去保留用户的认证信息或者会话信息。这就意味着**基于token认证机制的应用不需要去考虑用户在哪一台服务器登录了，这就为应用的扩展提供了便利。**

流程上是这样的：

- 用户使用用户名密码来请求服务器
- 服务器进行验证用户的信息
- 服务器通过验证发送给用户一个token
- 客户端存储token，并在每次请求时附送上这个token值
- 服务端验证token值，并返回数据

这个token必须要在每次请求时传递给服务端，它应该保存在请求头里， 另外，服务端要支持`CORS(跨来源资源共享)`策略，一般我们在服务端这么做就可以了`Access-Control-Allow-Origin: *`。

那么我们现在回到JWT的主题上。

### JWT长什么样？

JWT是由三段信息构成的，将这三段信息文本用`.`链接一起就构成了Jwt字符串。就像这样:

```css
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

### JWT的构成

第一部分我们称它为头部（header),第二部分我们称其为载荷（payload, 类似于飞机上承载的物品)，第三部分是签证（signature).

#### header（类型和算法）

jwt的头部承载两部分信息：

- 声明类型，这里是jwt
- 声明加密的算法 通常直接使用 HMAC SHA256

完整的头部就像下面这样的JSON：

```bash
{
  'typ': 'JWT',
  'alg': 'HS256'
}
```

然后将头部进行base64加密（该加密是可以对称解密的),构成了第一部分.

```undefined
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
```

#### playload（声明：用户，签发者）

载荷就是存放有效信息的地方。这个名字像是特指飞机上承载的货品，这些有效信息包含三个部分

- 标准中注册的声明
- 公共的声明
- 私有的声明

**标准中注册的声明** (建议但不强制使用) ：

- **iss**: jwt签发者
- **sub**: jwt所面向的用户
- **aud**: 接收jwt的一方
- **exp**: jwt的过期时间，这个过期时间必须要大于签发时间
- **nbf**: 定义在什么时间之前，该jwt都是不可用的.
- **iat**: jwt的签发时间
- **jti**: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

**公共的声明** ：
 公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.

**私有的声明** ：
 私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。

定义一个payload:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

然后将其进行base64加密，得到Jwt的第二部分。

```undefined
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
```

#### signature（base64加密）

jwt的第三部分是一个签证信息，这个签证信息由三部分组成：

- header (base64后的)
- payload (base64后的)
- secret（相当于是私钥）

这个部分需要base64加密后的header和base64加密后的payload使用`.`连接组成的字符串，然后通过header中声明的加密方式进行加盐`secret`组合加密，然后就构成了jwt的第三部分。

```csharp
// javascript
var encodedString = base64UrlEncode(header) + '.' + base64UrlEncode(payload);

var signature = HMACSHA256(encodedString, 'secret'); // TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

将这三部分用`.`连接成一个完整的字符串,构成了最终的jwt:

```css
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

**注意：secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。**

### 如何应用

一般是在请求头里加入`Authorization`，并加上`Bearer`标注：

```bash
fetch('api/user/1', {
  headers: {
    'Authorization': 'Bearer ' + token
  }
})
```

服务端会验证token，如果验证通过就会返回相应的资源。整个流程就是这样的:

<img src="https:////upload-images.jianshu.io/upload_images/1821058-2e28fe6c997a60c9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom:50%;" />

### 总结

#### 优点

- 因为json的通用性，所以JWT是可以进行跨语言支持的，像JAVA,JavaScript,NodeJS,PHP等很多语言都可以使用。
- 因为有了payload部分，所以JWT可以在自身存储一些其他业务逻辑所必要的非敏感信息。
- 便于传输，jwt的构成非常简单，字节占用很小，所以它是非常便于传输的。
- 它不需要在服务端保存会话信息, 所以它易于应用的扩展

#### 安全相关

- 不应该在jwt的payload部分存放敏感信息，因为该部分是客户端可解密的部分。
- 保护好secret私钥，该私钥非常重要。
- 如果可以，请使用https协议

### 用户获取用户数据的token被窃取了怎么办

1. 用https协议
2. 设置token有效期
3. 每次请求后刷新token

https://www.jianshu.com/p/5b9f1f4de88d

## Spring、SpringMvc、SpringBoot和SpringCloud的联系与区别

2. springMvc主要解决WEB开发的问题，是基于Servlet 的一个MVC框架，通过XML配置，统一开发前端视图和后端逻辑；

3. 由于Spring的配置非常复杂，各种XML、JavaConfig、servlet处理起来比较繁琐，为了简化开发者的使用，从而创造性地推出了springBoot框架。默认优于配置，简化了springMvc的配置流程；但区别于springMvc的是，springBoot专注于单体微服务接口开发，和前端解耦，虽然springBoot也可以做成springMvc前后台一起开发，但是这就有点不符合springBoot框架的初衷了；

3. 对于springCloud框架来说，它和springBoot一样，注重的是微服务的开发，但是springCloud更关注的是全局微服务接口的整合和管理，相当于管理多个springBoot框架的单体微服务；

# Spring Mvc

## SpringMVC 处理请求的一个流程?

![image-20210311111652464](picture/框架.assets/image-20210311111652464.png)

	1.客户端请求提交到DispatcherServlet
	2.由DispatcherServlet控制器查询一个或多个HandlerMapping，找到处理请求的Controller
	3.DispatcherServlet将请求提交到Controller
	4.Controller调用业务逻辑处理后，返回ModelAndView
	5.DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图
	6.视图负责将结果显示到客户端

```
DispatcherServlet请求到HandlerMapping。
HandlerMapping返回一个处理器执行链到DispatcherServlet。
DispatcherServlet再根据处理器执行链到HandlerAdaptor处理器适配器。
HandlerAdaptor处理器适配器再请求到Controller。
```

```
DispatcherServlet接口：
Spring提供的前端控制器，所有的请求都有经过它来统一分发。在DispatcherServlet将请求分发给Spring Controller之前，需要借助于Spring提供的HandlerMapping定位到具体的Controller。
HandlerMapping接口：
能够完成客户请求到Controller映射。
Controller接口：
需要为并发用户处理上述请求，因此实现Controller接口时，必须保证线程安全并且可重用。
Controller将处理用户请求，这和Struts Action扮演的角色是一致的。一旦Controller处理完用户请求，则返回ModelAndView对象给DispatcherServlet前端控制器，ModelAndView中包含了模型（Model）和视图（View）。
从宏观角度考虑，DispatcherServlet是整个Web应用的控制器；从微观考虑，Controller是单个Http请求处理过程中的控制器，而ModelAndView是Http请求过程中返回的模型（Model）和视图（View）。
ViewResolver接口：
Spring提供的视图解析器（ViewResolver）在Web应用中查找View对象，从而将相应结果渲染给客户。
```

## Servlet 的监听器和拦截器?

# SpringCloud框架

## 1.概念

SpringCloud的**基础功能**：

- 服务治理：Spring  Cloud Eureka
- 客户端负载均衡：Spring Cloud Ribbon
- 服务容错保护：Spring  Cloud Hystrix  
- 声明式服务调用：Spring  Cloud Feign
- API网关服务：Spring Cloud Zuul
- 分布式配置中心：Spring Cloud Config

SpringCloud的高级功能(本文不讲)：

- 消息总线：Spring  Cloud Bus
- 消息驱动的微服务：Spring Cloud Stream
- 分布式服务跟踪：Spring  Cloud Sleuth

### Eureka

那会出现什么问题呢？？首当其冲的就是子系统之间的**通讯**问题。子系统与子系统之间不是在同一个环境下，那就需要**远程调用**。远程调用可能就会想到httpClient，WebService等等这些技术来实现。

既然是远程调用，就必须知道ip地址，我们可能有以下的场景。

- 功能实现一：A服务需要调用B服务

- - 在A服务的代码里面调用B服务，**显式通过IP地址调用**：`http://123.123.123.123:8888/java3y/3`

- 功能实现二：A服务调用B服务，B服务调用C服务，C服务调用D服务

- - 在A服务的代码里面调用B服务，显式通过IP地址调用：`http://123.123.123.123:8888/java3y/3`，(同样地)B->C，C->D

- 功能实现三：D服务调用B服务，B服务调用C服务

- - 在D服务的代码里面调用B服务，显式通过IP地址调用：`http://123.123.123.123:8888/java3y/3`，(同样地)B->C

- …..等等等等

万一，我们**B服务的IP地址变了**，想想会出现什么问题：A服务,D服务(等等)需要**手动更新**B服务的地址

- 在服务多的情况下，手动来维护这些静态配置就是噩梦！

> 为了解决微服务架构中的**服务实例维护问题(ip地址)**， 产生了大量的**服务治理**框架和产品。这些框架和产品的实现都围绕着**服务注册与服务发现机制**来完成对微服务应用实例的**自动化管理**。

在SpringCloud中我们的服务治理框架一般使用的就是Eureka。

我们的问题：

- 现在有A、B、C、D四个服务，它们之间会互相调用(而且IP地址很可能会发生变化)，一旦某个服务的IP地址变了，那服务中的代码要跟着变，手动维护这些静态配置(IP)非常麻烦！

Eureka是这样解决上面所说的情况的：

- 创建一个E服务，将A、B、C、D四个服务的信息都**注册**到E服务上，E服务维护这些已经注册进来的信息

![image-20210314222700968](picture/框架.assets/image-20210314222700968.png)

A、B、C、D四个服务都可以**拿到**Eureka(服务E)那份**注册清单**。A、B、C、D四个服务互相调用不再通过具体的IP地址，而是**通过服务名来调用**！

- 拿到注册清单--->注册清单上有服务名--->自然就能够拿到服务具体的位置了(IP)。
- 其实简单来说就是：代码中通过**服务名找到对应的IP地址**(IP地址会变，但服务名一般不会变)

下面是Eureka的治理机制：

- 服务提供者

- - **服务注册：**启动的时候会通过发送REST请求的方式将**自己注册到Eureka Server上**，同时带上了自身服务的一些元数据信息。
  - **服务续约：**在注册完服务之后，**服务提供者会维护一个心跳**用来持续告诉Eureka Server:  "我还活着 ” 
  - **服务下线：**当服务实例进行正常的关闭操作时，它会**触发一个服务下线的REST请求**给Eureka Server, 告诉服务注册中心：“我要下线了 ”。

- 服务消费者

- - **获取服务：**当我们**启动服务消费者**的时候，它会发送一个REST请求给服务注册中心，来获取上面注册的服务清单
  - **服务调用：**服务消费者在获取服务清单后，通过**服务名**可以获得具体提供服务的实例名和该实例的元数据信息。在进行服务调用的时候，**优先访问同处一个Zone中的服务提供方**。

- Eureka Server(服务注册中心)：

- - **失效剔除：**默认每隔一段时间（默认为60秒） 将当前清单中超时（默认为90秒）**没有续约的服务剔除出去**。
  - **自我保护：**EurekaServer 在运行期间，会统计心跳失败的比例在15分钟之内是否低于85%(通常由于网络不稳定导致)。Eureka Server会将当前的**实例注册信息保护起来**， 让这些实例不会过期，尽可能**保护这些注册信息**。

![image-20210314222910528](picture/框架.assets/image-20210314222910528.png)

### RestTemplate和Ribbon

通过Eureka服务治理框架，我们可以通过服务名来获取具体的服务实例的位置了(IP)。一般在使用SpringCloud的时候**不需要自己手动创建**HttpClient来进行远程调用。

可以使用Spring封装好的**RestTemplate**工具类，使用起来很简单：

```java
    // 传统的方式，直接显示写死IP是不好的！
    //private static final String REST_URL_PREFIX = "http://localhost:8001";

    // 服务实例名
    private static final String REST_URL_PREFIX = "http://MICROSERVICECLOUD-DEPT";

    /**
     * 使用 使用restTemplate访问restful接口非常的简单粗暴无脑。 (url, requestMap,
     * ResponseBean.class)这三个参数分别代表 REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。
     */
    @Autowired
    private RestTemplate restTemplate;

    @RequestMapping(value = "/consumer/dept/add")
    public boolean add(Dept dept) {
        return restTemplate.postForObject(REST_URL_PREFIX + "/dept/add", dept, Boolean.class);
    }
```

为了实现服务的**高可用**，我们可以将**服务提供者集群**。比如说，现在一个秒杀系统设计出来了，准备上线了。在11月11号时为了能够支持高并发，我们开多台机器来支持并发量。

现在想要这三个秒杀系统**合理摊分**用户的请求(专业来说就是负载均衡)，可能你会想到nginx。

其实SpringCloud也支持的负载均衡功能，只不过它是**客户端的负载均衡**，这个功能实现就是Ribbon！

负载均衡又区分了两种类型：

- 客户端负载均衡(Ribbon)

- - 服务实例的**清单在客户端**，客户端进行负载均衡算法分配。
  - (从上面的知识我们已经知道了：客户端可以从Eureka Server中得到一份服务清单，在发送请求时通过负载均衡算法，**在多个服务器之间选择一个进行访问**)

- 服务端负载均衡(Nginx)

- - 服务实例的**清单在服务端**，服务器进行负载均衡算法分配

所以，我们的图可以画成这样：

![image-20210314223051401](picture/框架.assets/image-20210314223051401.png)

### Hystrix

到目前为止，我们的服务看起来好像挺好的了：能够根据服务名来远程调用其他的服务，可以实现客户端的负载均衡。

![image-20210314223146684](picture/框架.assets/image-20210314223146684.png)

但是，如果我们在**调用多个远程服务时，某个服务出现延迟**，会怎么样？？

![image-20210314223217730](picture/框架.assets/image-20210314223217730.png)

在**高并发**的情况下，由于单个服务的延迟，可能导致**所有的请求都处于延迟状态**，甚至在几秒钟就使服务处于负载饱和的状态，资源耗尽，直到不可用，最终导致这个分布式系统都不可用，这就是“雪崩”。

![image-20210314223239183](picture/框架.assets/image-20210314223239183.png)

针对上述问题， Spring Cloud Hystrix实现了**断路器、线程隔离**等一系列服务保护功能。

- Fallback(失败快速返回)：当某个服务单元发生故障（类似用电器发生短路）之后，通过断路器的故障监控（类似熔断保险丝）， **向调用方返回一个错误响应， 而不是长时间的等待**。这样就不会使得线程因调用故障服务被长时间占用不释放，**避免**了故障在分布式系统中的**蔓延**。
- 资源/依赖隔离(线程池隔离)：它会为**每一个依赖服务创建一个独立的线程池**，这样就算某个依赖服务出现延迟过高的情况，也只是对该依赖服务的调用产生影响， 而**不会拖慢其他的依赖服务**。

Hystrix提供几个熔断关键参数：`滑动窗口大小（20）、 熔断器开关间隔（5s）、错误率（50%）`

- 每当20个请求中，有50%失败时，熔断器就会打开，此时再调用此服务，将会**直接返回失败**，不再调远程服务。
- 直到5s钟之后，重新检测该触发条件，**判断是否把熔断器关闭，或者继续打开**。

![image-20210314223517193](picture/框架.assets/image-20210314223517193.png)

### Feign

上面已经介绍了Ribbon和Hystrix了，可以发现的是：他俩作为基础工具类框架**广泛地应用**在各个微服务的实现中。我们会发现对这两个框架的**使用几乎是同时出现**的。

为了**简化**我们的开发，Spring Cloud Feign出现了！它基于 Netflix Feign 实现，**整合**了 Spring Cloud Ribbon 与 Spring Cloud Hystrix,  除了整合这两者的强大功能之外，它还提
供了**声明式的服务调用**(不再通过RestTemplate)。

> Feign是一种**声明式、模板化的HTTP客户端。**
>
> 在Spring Cloud中使用Feign, 我们可以做到使用HTTP请求远程服务时能与调用本地方法一样的编码体验，开发者完全感知不到这是远程方法，更感知不到这是个HTTP请求。

下面就简单看看Feign是怎么优雅地实现远程调用的：

服务绑定：

```java
// value --->指定调用哪个服务
// fallbackFactory--->熔断器的降级提示
@FeignClient(value = "MICROSERVICECLOUD-DEPT", fallbackFactory = DeptClientServiceFallbackFactory.class)
public interface DeptClientService {


    // 采用Feign我们可以使用SpringMVC的注解来对服务进行绑定！
    @RequestMapping(value = "/dept/get/{id}", method = RequestMethod.GET)
    public Dept get(@PathVariable("id") long id);

    @RequestMapping(value = "/dept/list", method = RequestMethod.GET)
    public List<Dept> list();

    @RequestMapping(value = "/dept/add", method = RequestMethod.POST)
    public boolean add(Dept dept);
}
```

Feign中使用熔断器：

```java
/**
 * Feign中使用断路器
 * 这里主要是处理异常出错的情况(降级/熔断时服务不可用，fallback就会找到这里来)
 */
@Component // 不要忘记添加，不要忘记添加
public class DeptClientServiceFallbackFactory implements FallbackFactory<DeptClientService> {
    @Override
    public DeptClientService create(Throwable throwable) {
        return new DeptClientService() {
            @Override
            public Dept get(long id) {
                return new Dept().setDeptno(id).setDname("该ID：" + id + "没有没有对应的信息,Consumer客户端提供的降级信息,此刻服务Provider已经关闭")
                        .setDb_source("no this database in MySQL");
            }

            @Override
            public List<Dept> list() {
                return null;
            }

            @Override
            public boolean add(Dept dept) {
                return false;
            }
        };
    }
}
```

### Zuul

基于上面的学习，我们现在的架构很可能会设计成这样：

![image-20210314223842335](picture/框架.assets/image-20210314223842335.png)

这样的架构会有两个比较麻烦的问题：

1. **路由规则与服务实例的维护间题**：外层的负载均衡(nginx)需要**维护**所有的服务实例清单(图上的OpenService)
2. **签名校验、 登录校验冗余问题**：为了保证对外服务的安全性， 我们在服务端实现的微服务接口，往往都会有一定的**权限校验机制**，但我们的服务是独立的，我们**不得不在这些应用中都实现这样一套校验逻辑**，这就会造成校验逻辑的冗余。

还是画个图来理解一下吧：

![image-20210314224002083](picture/框架.assets/image-20210314224002083.png)

每个服务都有自己的IP地址，Nginx想要正确请求转发到服务上，就必须**维护着每个服务实例的地址**！

- 更是灾难的是：这些服务实例的IP地址还有可能会变，服务之间的划分也很可能会变。

```
http://123.123.123.123

http://123.123.123.124

http://123.123.123.125

http://123.123.123.126

http://123.123.123.127
```

购物车和订单模块都需要用户登录了才可以正常访问，基于现在的架构，只能在**购物车和订单模块都编写校验逻辑**，这无疑是冗余的代码。

为了解决上面这些常见的架构问题，**API网关**的概念应运而生。在SpringCloud中了提供了基于Netfl ix Zuul实现的API网关组件**Spring Cloud Zuul**。

Spring Cloud Zuul是这样解决上述两个问题的：

- SpringCloud Zuul通过与SpringCloud Eureka进行整合，将自身注册为Eureka服务治理下的应用，同时从Eureka中获得了所有其他微服务的实例信息。**外层调用都必须通过API网关**，使得**将维护服务实例的工作交给了服务治理框架自动完成**。
- 在API网关服务上进行统一调用来**对微服务接口做前置过滤**，以实现对微服务接口的**拦截和校验**。

Zuul天生就拥有线程隔离和断路器的自我保护功能，以及对服务调用的客户端负载均衡功能。也就是说：**Zuul也是支持Hystrix和Ribbon**。

关于Zuul还有很多知识点(由于篇幅问题，这里我就不细说了)：

- 路由匹配(动态路由)
- 过滤器实现(动态过滤器)
- 默认会过滤掉Cookie与敏感的HTTP头信息(额外配置)

既然Zuul已经能够实现了，那我们的Feign还有必要吗？服务间也有调用

### SpringCloud Config

随着业务的扩展，我们的服务会越来越多，越来越多。每个服务都有自己的配置文件。

既然是配置文件，给我们配置的东西，那**难免会有些改动**的。

比如我们的Demo中，每个服务都写上**相同**的配置文件。万一我们有一天，配置文件中的密码需要更换了，那就得**三个都要重新更改**。

![image-20210314224339424](picture/框架.assets/image-20210314224339424.png)

> 在分布式系统中，某一个基础服务信息变更，都**很可能**会引起一系列的更新和重启

Spring Cloud Config项目是一个解决分布式系统的配置管理方案。它包含了Client和Server两个部分，**server提供配置文件的存储、以接口的形式将配置文件的内容提供出去，client通过接口获取数据、并依据此数据初始化自己的应用**。

- 简单来说，使用Spring Cloud Config就是将配置文件放到**统一的位置管理**(比如GitHub)，客户端通过接口去获取这些配置文件。
- 在GitHub上修改了某个配置文件，应用加载的就是修改后的配置文件。

SpringCloud Config其他的知识：

- 在SpringCloud Config的服务端， 对于配置仓库的默认**实现采用了Git**，我们也可以配置SVN。
- 配置文件内的信息**加密和解密**
- 修改了配置文件，希望不用重启来**动态刷新配置**，配合Spring  Cloud Bus 使用~

使用SpringCloud Config可能的疑问：application.yml和 bootstrap.yml区别

# Spring源码

## 1.IOC

![img](picture/框架.assets/4634752-6a2a930c71a96eb1.png)

![img](picture/框架.assets/4634752-5a43ff6040f0bbe8.png)

### 1.什么是 IOC

​        控制反转（Inversion of Control，缩写为IOC），是面向对象编程中的一种设计原则。其中最常见的方式叫做依赖注入（Dependency Injection，简称DI）

   所谓**[控制反转，就是把原先我们代码里面需要实现的对象创建、依赖的代码，反转过来让容器帮我们实现这个过程。目的是为了降低代码之间的耦合度]()**那么我们则需要创建一个容器，同时需要一种描述来让容器知道我们需要创建的对象，以及每个对象之间的关系。那么这个描述，最具体的表现就是我们可配置的文件（Spring 中 为 applicationContext.xml）(也可以是注解配置)

​    对象与对象之间的关系，我们可以通过 xml、properties等配置文件来表示。那么这些文件存放的位置该怎么描述？我们可以通过classpath、filesystem或者URL地址等来获取这些配置文件。

### 2.IOC 容器的功能

​        BOP 编程：Spring 是基于 Bean 来开发的，即：一切都是以 Bean 为主，这就是所谓的 BOP（Bean Oriented Programming）编程。

​    Spring 在项目启动阶段，会通过读取配置文件的方式，通过预设规则，去顺序的加载或识别需要对接的 Bean（反射，通过类全名字符串可以找到并创建一个Bean的实例）。然后将生成的 Bean 对象存储在 IOC 容器中。所以：IOC容器，主要是用来存放由容器帮我们生成的Java Bean对象。Spring 中 IOC 的实现，是[通过ConcurrentHashMap的方式]()实现的。

 [!!!注意:]()Map<String, BeanDefinition> beanDefinitionMap是读取配置文件数据存储位置【中间过程】，默认大小为 256。真正的 IOC容器是 DI 依赖注入后的对象，即 Map<String, Object> factoryBeanObjectCach，默认大小为16。

```java
/** Map of bean definition objects, keyed by bean name */
//存储注册信息的BeanDefinition
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);

/** Cache of singleton objects created by FactoryBeans: FactoryBean name --> object */
//依赖注入之后，返回 Map<String,Object>， factoryBeanObjectCache就是真正的IOC容器
private final Map<String, Object> factoryBeanObjectCache = new ConcurrentHashMap<>(16);
```

### 3.源码分析从何入手

​    我们知道： Spring 是通过加载配置文件的方式启动。 IOC 实现原理：主要是获取配置文件，解析配置文件中信息，然后根据配置信息来帮助我们完成 Bean 对象的创建。所以我们应该从 IOC如何读取配置文件入手。

我们在学习 Spring 的使用时，都是通过如下一段代码，开启 Spring 的学习之路。

```java
ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");

AnnotationConfigApplicationContext annotationConfigApplicationContext = new AnnotationConfigApplicationContext();

User user1 = (User)annotationConfigApplicationContext.getBean("user");
User user2 = (User)applicationContext.getBean("user");
```


所以我们便可以从ClassPathXmlApplicationContext类入手，开启我们学习 Spring 源码的大门。


### 4.BeanFactory接口

​        [Spring Bean 的创建，就是典型的工厂模式]()。字面意思，它就是用来为我们创建 Bean 类的。工厂模式的使用，从而满足 IOC 容器为开发者管理对象间的依赖关系提供了很多便利和基础服务。在 Spring 中的实现，有许多 IOC 容器的实现供我们用户选择和使用。如下为BeanFactory的关系结构图：


<img src="picture/框架.assets/20200309172752547.png" alt="在这里插入图片描述" style="zoom: 67%;" />

​    [BeanFactory 作为顶级接口，它定义了 IOC 容器的最基本规范。]()从结构图我们可以看到：BeanFactory 有三个子类，分别是HierarchicalBeanFactory、AutowireCapableBeanFactory和ListableBeanFactory。但是[他们最终的实现类工厂都是 DefaultListableBeanFactory。]()

​    既然所有的工作都由 DefaultListableBeanFactory 来完成，那为什么还要定义这么多层的接口呢？其实每个接口都有它的使用的场合，主要是为了区分在 Spring 内部操作过程中对象的传递和转化过程中，对对象的数据访问所做的限制。

例如：①ListableBeanFactory 接口表示这些 Bean 是可列表的；②HierarchicalBeanFactory 表示的是这些 Bean 是有继承关系的，也就是每个 Bean 有可能有父 Bean；③AutowireCapableBeanFactory 接口定义 Bean 的自动装配规则 。这几个接口就定义了Bean的集合、Bean之间关系、以及 Bean的行为。

```java
public interface BeanFactory {

	//对FactoryBean的转义定义，因为如果使用bean的名字检索FactoryBean得到的对象是工厂生成的对象，
	//如果需要得到工厂本身，需要转义
	String FACTORY_BEAN_PREFIX = "&";

	//根据bean的名字，获取在IOC容器中得到bean实例
	Object getBean(String name) throws BeansException;

	//根据bean的名字和Class类型来得到bean实例，增加了类型安全验证机制。
	<T> T getBean(String name, @Nullable Class<T> requiredType) throws BeansException;

	//提供对bean的检索，看看是否在IOC容器有这个名字的bean
	boolean containsBean(String name);

	//根据bean名字得到bean实例，并同时判断这个bean是不是单例
	boolean isSingleton(String name) throws NoSuchBeanDefinitionException;

	//得到bean实例的Class类型
	@Nullable
	Class<?> getType(String name) throws NoSuchBeanDefinitionException;

	//得到bean的别名，如果根据别名检索，那么其原名也会被检索出来
	String[] getAliases(String name);
}
```

> 在 BeanFactory 接口中它只对 IOC 容器作了定义，并不关心 Bean 是如何定义怎样加载的。 
>
> 正如我们只关心工厂里得到什么的产品对象，至于工厂是怎么生产这些对象的，这个基本的接口并不关心。

​    而要知道工厂是如何产生对象的，我们需要看具体的 IOC 容器实现，Spring 提供了许多 IOC 容器的 实现。比如 XmlBeanFactory，ClasspathXmlApplicationContext 等。此处我们通过 ClasspathXmlApplicationContext 来分析，这个 IOC 容器可以读取 XML 文件定义的 BeanDefinition（XML 文件中对 bean 的描述）。

### 5.BeanDefinition 接口

​       Spring IOC 容器管理了我们定义的各种 Bean 对象，以及Bean对象之间的相互关系。Bean对象之间的关系，我们是通过 xml 文件的方式来配置的。IOC 既然要帮助我们完成对象的创建以及每个对象之间的关系，那么 IOC 容器则需要通过读取 xml 配置文件来获取具体配置的信息。

​    Spring源码中，对 xml 文件中 <bean> 标签的配置解析，使用的是 BeanDefinition 类来表示的。

​    Bean 类的解析过程相对来说比较复杂，它会对 Spring 的 xml 配置文件中所有配置进行一一解析，包括常见的<import>、<alias>、<beans>、<bean>等标签进行解析，还会对标签中的所有属性进行解析。针对每个标签以及部分属性等，都是单独定义一个方法来完成这个操作，功能上来说，分的比较细致，因为它要保证 Spring 框架的可扩展性、灵活性。

### 6.IOC容器初始化

​        IOC容器的初始化，分为以下三个过程：[1.定位   2.加载   3.注册]() ，接下来详细介绍一下 IOC 容器初始化的这几个过程。

#### 1 定位

   1.什么是定位?

> 即：通过用户配置的信息，使用 classpath、filesystem、url 链接等加载文件方式，获取到资源，最终将资源解析成一个 Resource 类型文件的过程

   2.如何定位？

>   XmlBeanDefinitionReader 通过调用其父类 DefaultResourceLoader 的 getResource() 方法获取要加载的资源的过程

   3.定位实现的简单步骤

> 1.获取配置文件名称
> 2.通过不同类型的 resourceLoader 加载器加载文件
> 3.调用 DefaultResourceLoader 类中的 getSource() 方法定位 Resource
> 4.获取Resource类型文件(内容实际是获取自己配置 xml 的信息)

#### 2.加载

   1.什么是加载?

> ​    即：通过对定位获取到的 xml 文件，进行每一步细致的解析，然后获取到 BeanDefinition 类的过程

   2.什么时候加载？

> ​     Spring IOC 容器对 Bean 定义资源的载入，是从 refresh() 函数开始的，refresh()是一个模板方法

   3.加载实现的简单步骤

> 1.使用 resourceLoader.getResource(location) 获取要加载的资源
> 2.使用 JAXP 将 Resource 类型的 xml 文件流转换成为 Document 对象
> 3.根据 Document 对象获取所有的 node 子节点
> 4.根据子节点来判断，是否是 <import> 标签、<alias> 标签、<bean> 标签、<beans>标签
> 5.针对每个不同的标签以及标签中的不同属性参数，使用不同的方法做处理
> 6.将 xml 文件中所有的配置的 bean 信息设置到 BeanDefination 类中进行保存

#### 3.注册

   1.什么是注册?

> 即：将 BeanDefinition 类根据 key (key可以是xml文件中配置的 id、name、alias等配置的属性，只要唯一即可)，put 到 Map 的过程。
>
> 从注册这个过程，你会发现：IOC容器其实就是一个Map（实际上是一个ConcurrentMap）

   2.注册实现的简单步骤？

> 1.注册的过程中，使用了 synchronized 锁，保持线程同步，从而保证数据的一致性
> 2.将 BeanDefinition 类中的 id、name、alias属性(只要唯一即可)等来充当key，将BeanDefinition 类 put到 Map中，即表示注册完成。

### 7.IOC容器一定是单例的

​        IOC容器初始化，定位完成后。会读取配置文件，此时会调用一个refresh()方法，来保证 IOC 容器必须是唯一的。

refresh()方法的作用：

> refresh()方法使用 synchronized 修饰。
>
> 在创建 IOC 容器前，如果已经有容器存在，则需要把已有的容器销毁和关闭，以保证在 refresh 之后使用的是新建立起来的 IOC 容器。

​    refresh 的作用类似于：

> ①对 IOC 容器的重启；
>
> ②在新建立好的容器中对容器进行初始化，对 Bean 定义资源进行载入。

### 8.源码

#### 1.refresh() 方法解析

```java
//1.容器初始化的过程，读入Bean定义资源
public void refresh() throws BeansException, IllegalStateException {
    synchronized (this.startupShutdownMonitor) {
        //调用容器准备刷新的方法，获取容器的当时时间，同时给容器设置同步标识
        prepareRefresh();

        //告诉子类启动 refreshBeanFactory()方法，Bean 定义资源文件的载入从子类的 refreshBeanFactory()方法启动
        //注意：此处使用委派模式，委派子类完成对IOC容器唯一性判断
        ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

        //为 BeanFactory 配置容器特性，例如类加载器、事件处理器等
        prepareBeanFactory(beanFactory);

        try {
            //为容器的某些子类指定特殊的 BeanPost 事件处理器
            postProcessBeanFactory(beanFactory);

            //调用所有注册的 BeanFactoryPostProcessor 的 Bean
            //getBean()方法就是在此处调用，来完成bean的初始化操作的
            invokeBeanFactoryPostProcessors(beanFactory);

            //为 BeanFactory 注册 BeanPost 事件处理器.BeanPostProcessor 是 Bean 后置处理器，用于监听容器触发的事件
            registerBeanPostProcessors(beanFactory);

            //初始化信息源，和国际化相关.
            initMessageSource();

            //初始化容器事件传播器.
            initApplicationEventMulticaster();

            //调用子类的某些特殊 Bean 初始化方法
            onRefresh();

            //为事件传播器注册事件监听器.
            registerListeners();

            //初始化所有剩余的单例 Bean.
            finishBeanFactoryInitialization(beanFactory);

            //初始化容器的生命周期事件处理器，并发布容器的生命周期事件
            finishRefresh();
        }catch (BeansException ex) {
            //销毁已经创建的单例Bean
            destroyBeans();

            //取消 refresh 操作，重置容器的同步标识.
            cancelRefresh(ex);

            // Propagate exception to caller.
            throw ex;
    	}
    }
}
```

#### 2.obtainFreshBeanFactory()方法

​         AbstractApplicationContext 的 obtainFreshBeanFactory() 方法调用子类容器的 refreshBeanFactory() 方法,在 obtainFreshBeanFactory() 方法中实现启动容器载入BeanDefinition的过程，代码如下：

```java
protected ConfigurableListableBeanFactory obtainFreshBeanFactory() {
    //这里使用了委派设计模式，父类定义了抽象的 refreshBeanFactory()方法，具体实现调用子类容器的refreshBeanFactory()方法
    refreshBeanFactory();
    ConfigurableListableBeanFactory beanFactory = getBeanFactory();
    if (logger.isDebugEnabled()) {
        logger.debug("Bean factory for " + getDisplayName() + ": " + beanFactory);
    }
    return beanFactory;
}
```

#### 3.refreshBeanFactory()方法

​       `AbstractApplicationContext` 类中只抽象定义了 `refreshBeanFactory()`方法，容器真正调用的是子类 `AbstractRefreshableApplicationContext` 实现的 `refreshBeanFactory()`方法，源码如下：

```java
@Override
protected final void refreshBeanFactory() throws BeansException {
    if (hasBeanFactory()) {{//如果已经有容器，销毁容器中的 bean，关闭容器
        destroyBeans();
        closeBeanFactory();
    }
    try {
        //创建 IOC 容器
        DefaultListableBeanFactory beanFactory = createBeanFactory();
        beanFactory.setSerializationId(getId());
        //对 IOC 容器进行定制化，如设置启动参数，开启注解的自动装配等
        customizeBeanFactory(beanFactory);
        //*******************重要******************************
        //***********此处是真正的载入BeanDefinition的地方*********
        //调用载入 Bean 定义的方法，主要这里又使用了一个委派模式，在当前类中只定义了抽象的 loadBeanDefinitions 方法，具体的实现调用子类容器
        loadBeanDefinitions(beanFactory);
      
        synchronized (this.beanFactoryMonitor) {
            this.beanFactory = beanFactory;
        }
    }catch (IOException ex) {
        throw new ApplicationContextException("I/O error parsing bean definition source for " + getDisplayName(), ex);
    }
}
```

## 2.DI

<img src="picture/框架.assets/20180820100617885" alt="img" style="zoom:67%;" />

### 1.依赖注入介绍

​        依赖注入，即 Spring 中的 DI（Dependency Injection）

​    在我们学习 Spring 的使用时，我们最熟悉它的特点是：IOC 控制反转 和 DI 依赖注入。Spring 通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中。

> 依赖注入，就是指对象是被动的接受依赖类而不是自己主动去寻找。也就是说对象不是从IOC容器中查找它的依赖类，而是在容器实例化对象的时候，已经将该对象所依赖的类注入给了该对象。

### 2.何时进行依赖注入

​       当 Spring IOC 容器启动时完成定位、加载、注册操作，此时 IOC容器已经获取到 applicationContext.xml 配置文件中的全部配置，并以 BeanDefinition类的形式保存在一个名为：beanDefinitionMap 的 ConcurrentHashMap 中。如下所示：

```java
//存储注册信息的BeanDefinition
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);
```

​    此时 IOC容器只是对这些 BeanDefinition 类进行存储，而并没有真正的创建实例对象，也就是说并没有进行依赖注入操作。那么何时完成依赖注入操作？？

何时依赖注入，此处就[涉及到了 lazy-init 属性]()的使用。lazy-init 属性是 Spring 中用来延迟加载 bean 的。使用在用户定义的标签中，默认为false。使用实例如下：

```java
//lazy-init默认为 false,可省略
<bean id="testBean" class="com.eacxzm.TestBean">
 
//lazy-init为 true时
<bean id="testBean" class="com.eacxzm.TestBean" lazy-init="true">
```

 第一：如果你使用BeanFactory作为Spring Bean的工厂类，则所有的bean都是在第一次使用该Bean的时候实例化 
 第二：如果你使用ApplicationContext作为Spring Bean的工厂类，则又分为以下几种情况： 
    （1）：如果bean的scope是singleton的，并且[lazy-init为false]()（默认是false，所以可以不用设置），则ApplicationContext启动的时候就实例化该Bean，并且将实例化的Bean放在一个map结构的缓存中，下次再使用该Bean的时候，直接从这个缓存中取 
    （2）：如果bean的scope是singleton的，并且[lazy-init为true]()，则该Bean的实例化是在第一次使用该Bean的时候进行实例化 
    （3）：如果bean的scope是[prototype]()的，则该Bean的实例化是在第一次使用该Bean的时候进行实例化

![image-20210404185302942](picture/框架.assets/image-20210404185302942.png)

### 3.实例化出来的bean对象用什么类型来存储？

   答案是：使用 FactoryBean类型来存储。

> BeanFactory：bean工厂，用来生产 Bean 对象的一个工厂类；
> FactoryBean：由 Spring 生产出来的 Bean，就是 FactoryBean。是由 Spring 帮你生产的，不是由开发者自己生产（new）出来的。

### 4.JDK反射和Cglib代理，Spring默认使用哪个创建 Bean 实例？

​       Spring 默认使用 Cglib 代理来创建 Bean 实例。

```
//所有生成Bean都有BeanWrapper封装，bean的生成采用策略模式，CglibSubclassingInstantiationStrategy实现是默认的策略
//1.如果配置为工厂方法创建(配置了factory-mothed的)，由java反射Mehthod的invoke完成bean的创建。
//2.如果采用构造参数配置创建(配置了constructor-arg的)，如果bean对应的类包含多个构造参数，采用cglib动态字节码构造；如果只有唯一的构造参数，那么采用java反射Constructor的newInstance方法
//3.如果是普通bean配置,直接通过反射Class默认的Constructor，然后调用newInstance获得bean.
 instanceWrapper = createBeanInstance(beanName, mbd, args);
```

    因为：使用 Cglib 代理来创建 Bean，Spring 可以拥有该代理类的控制权；如果使用反射，获取到的控制权是有限的。
    
    比如接下来我们要介绍的 Spring AOP 功能(面向切面编程)、Spring Listener(监听机制)，如果使用反射，显然无法满足这些功能，所以 Spring 默认使用的是 Cglib 代理。
### 5.源码

#### 1.autowireByName()

```java
//根据名称对属性进行自动依赖注入
protected void autowireByName(
		String beanName, AbstractBeanDefinition mbd, BeanWrapper bw, MutablePropertyValues pvs) {

	//对Bean对象中非简单属性(不是简单继承的对象，如原始类型，字符串，URL等都是简单属性)进行处理
	String[] propertyNames = unsatisfiedNonSimpleProperties(mbd, bw);
	for (String propertyName : propertyNames) {
		//如果Spring IOC容器中包含指定名称的Bean
		if (containsBean(propertyName)) {
			//调用getBean方法向IOC容器索取指定名称的Bean实例，迭代触发属性的初始化和依赖注入
			Object bean = getBean(propertyName);
			//为指定名称的属性赋予属性值
			pvs.add(propertyName, bean);
			//指定名称属性注册依赖Bean名称，进行属性依赖注入
			registerDependentBean(propertyName, beanName);
    }
	}
}
```

####  2.autowireByType()

```java
//根据类型对属性进行自动依赖注入
protected void autowireByType(
		String beanName, AbstractBeanDefinition mbd, BeanWrapper bw, MutablePropertyValues pvs) {

	//获取用户定义的类型转换器
	TypeConverter converter = getCustomTypeConverter();
	if (converter == null) {
		converter = bw;
	}

	//存放解析的要注入的属性
	Set<String> autowiredBeanNames = new LinkedHashSet<>(4);
	//对Bean对象中非简单属性(不是简单继承的对象，如原始类型，字符,URL等都是简单属性)进行处理
	String[] propertyNames = unsatisfiedNonSimpleProperties(mbd, bw);
	for (String propertyName : propertyNames) {
		try {
			//获取指定属性名称的属性描述器
			PropertyDescriptor pd = bw.getPropertyDescriptor(propertyName);
			//不对Object类型的属性进行autowiring自动依赖注入
			if (Object.class != pd.getPropertyType()) {
				//获取属性的setter方法
				MethodParameter methodParam = BeanUtils.getWriteMethodParameter(pd);
				//检查指定类型是否可以被转换为目标对象的类型
				boolean eager = !PriorityOrdered.class.isInstance(bw.getWrappedInstance());
				//创建一个要被注入的依赖描述
				DependencyDescriptor desc = new AutowireByTypeDependencyDescriptor(methodParam, eager);
				//根据容器的Bean定义解析依赖关系，返回所有要被注入的Bean对象
				Object autowiredArgument = resolveDependency(desc, beanName, autowiredBeanNames, converter);
				if (autowiredArgument != null) {
					//为属性赋值所引用的对象
					pvs.add(propertyName, autowiredArgument);
				}
				for (String autowiredBeanName : autowiredBeanNames) {
					//指定名称属性注册依赖Bean名称，进行属性依赖注入
					registerDependentBean(autowiredBeanName, beanName);
				}
			}
		}
	}
}
```

####  3.applyPropertyValues()

```java
//对未配置autowiring的属性进行依赖注入处理的过程
protected void applyPropertyValues(String beanName, BeanDefinition mbd, BeanWrapper bw, PropertyValues pvs) {
	if (pvs.isEmpty()) {
		return;
	}

	//封装属性值
	MutablePropertyValues mpvs = null;
	List<PropertyValue> original;

	if (System.getSecurityManager() != null) {
		if (bw instanceof BeanWrapperImpl) {
			//设置安全上下文，JDK安全机制
			((BeanWrapperImpl) bw).setSecurityContext(getAccessControlContext());
		}
	}

	if (pvs instanceof MutablePropertyValues) {
		mpvs = (MutablePropertyValues) pvs;
		//属性值已经转换
		if (mpvs.isConverted()) {
			// Shortcut: use the pre-converted values as-is.
			try {
				//为实例化对象设置属性值
				bw.setPropertyValues(mpvs);
				return;
			}
			catch (BeansException ex) {
				throw new BeanCreationException(
						mbd.getResourceDescription(), beanName, "Error setting property values", ex);
			}
		}
		//获取属性值对象的原始类型值
		original = mpvs.getPropertyValueList();
	}
	else {
		original = Arrays.asList(pvs.getPropertyValues());
	}

	//获取用户自定义的类型转换
	TypeConverter converter = getCustomTypeConverter();
	if (converter == null) {
		converter = bw;
	}
	//创建一个Bean定义属性值解析器，将Bean定义中的属性值解析为Bean实例对象的实际值
	BeanDefinitionValueResolver valueResolver = new BeanDefinitionValueResolver(this, beanName, mbd, converter);

	// Create a deep copy, resolving any references for values.

	//为属性的解析值创建一个拷贝，将拷贝的数据注入到实例对象中
	List<PropertyValue> deepCopy = new ArrayList<>(original.size());
	boolean resolveNecessary = false;
	for (PropertyValue pv : original) {
		//属性值不需要转换
		if (pv.isConverted()) {
			deepCopy.add(pv);
		}
		//属性值需要转换
		else {
			String propertyName = pv.getName();
			//原始的属性值，即转换之前的属性值
			Object originalValue = pv.getValue();
      //*****************************************************************************
			//3.1.-->重点方法：转换属性值，例如将引用转换为IOC容器中实例化对象引用
			Object resolvedValue = valueResolver.resolveValueIfNecessary(pv, originalValue);
			//转换之后的属性值
			Object convertedValue = resolvedValue;
			//属性值是否可以转换
			boolean convertible = bw.isWritableProperty(propertyName) &&
					!PropertyAccessorUtils.isNestedOrIndexedProperty(propertyName);
			if (convertible) {
				//使用用户自定义的类型转换器转换属性值
				convertedValue = convertForProperty(resolvedValue, propertyName, bw, converter);
			}
			// Possibly store converted value in merged bean definition,
			// in order to avoid re-conversion for every created bean instance.
			//存储转换后的属性值，避免每次属性注入时的转换工作
			if (resolvedValue == originalValue) {
				if (convertible) {
					//设置属性转换之后的值
					pv.setConvertedValue(convertedValue);
				}
				deepCopy.add(pv);
			}
			//属性是可转换的，且属性原始值是字符串类型，且属性的原始类型值不是
			//动态生成的字符串，且属性的原始值不是集合或者数组类型
			else if (convertible && originalValue instanceof TypedStringValue &&
					!((TypedStringValue) originalValue).isDynamic() &&
					!(convertedValue instanceof Collection || ObjectUtils.isArray(convertedValue))) {
				pv.setConvertedValue(convertedValue);
				//重新封装属性的值
				deepCopy.add(pv);
			}
			else {
				resolveNecessary = true;
				deepCopy.add(new PropertyValue(pv, convertedValue));
			}
		}
	}
	if (mpvs != null && !resolveNecessary) {
		//标记属性值已经转换过
		mpvs.setConverted();
	}

	// Set our (possibly massaged) deep copy.
  //****************************************************************************
	//3.2.-->重点方法：进行属性依赖注入
	try {
		bw.setPropertyValues(new MutablePropertyValues(deepCopy));
	}
	catch (BeansException ex) {
		throw new BeanCreationException(
				mbd.getResourceDescription(), beanName, "Error setting property values", ex);
	}
}
```

##### 3.1 resolveValueIfNecessary()方法，完成对属性值的解析操作

```java
//解析属性值，对注入类型进行转换
@Nullable
public Object resolveValueIfNecessary(Object argName, @Nullable Object value) {
	// We must check each value to see whether it requires a runtime reference
	// to another bean to be resolved.
	//对引用类型的属性进行解析
	if (value instanceof RuntimeBeanReference) {
		RuntimeBeanReference ref = (RuntimeBeanReference) value;
		//调用引用类型属性的解析方法
		return resolveReference(argName, ref);
	}
	//对属性值是引用容器中另一个Bean名称的解析
	else if (value instanceof RuntimeBeanNameReference) {
		String refName = ((RuntimeBeanNameReference) value).getBeanName();
		refName = String.valueOf(doEvaluate(refName));
		//从容器中获取指定名称的Bean
		if (!this.beanFactory.containsBean(refName)) {
			throw new BeanDefinitionStoreException(
					"Invalid bean name '" + refName + "' in bean reference for " + argName);
		}
		return refName;
	}
	//对Bean类型属性的解析，主要是Bean中的内部类
	else if (value instanceof BeanDefinitionHolder) {
		// Resolve BeanDefinitionHolder: contains BeanDefinition with name and aliases.
		BeanDefinitionHolder bdHolder = (BeanDefinitionHolder) value;
		return resolveInnerBean(argName, bdHolder.getBeanName(), bdHolder.getBeanDefinition());
	}
	else if (value instanceof BeanDefinition) {
		// Resolve plain BeanDefinition, without contained name: use dummy name.
		BeanDefinition bd = (BeanDefinition) value;
		String innerBeanName = "(inner bean)" + BeanFactoryUtils.GENERATED_BEAN_NAME_SEPARATOR +
				ObjectUtils.getIdentityHexString(bd);
		return resolveInnerBean(argName, innerBeanName, bd);
	}
	//对集合数组类型的属性解析
	else if (value instanceof ManagedArray) {
		// May need to resolve contained runtime references.
		ManagedArray array = (ManagedArray) value;
		//获取数组的类型
		Class<?> elementType = array.resolvedElementType;
		if (elementType == null) {
			//获取数组元素的类型
			String elementTypeName = array.getElementTypeName();
			if (StringUtils.hasText(elementTypeName)) {
				try {
					//使用反射机制创建指定类型的对象
					elementType = ClassUtils.forName(elementTypeName, this.beanFactory.getBeanClassLoader());
					array.resolvedElementType = elementType;
				}
				catch (Throwable ex) {
					// Improve the message by showing the context.
					throw new BeanCreationException(
							this.beanDefinition.getResourceDescription(), this.beanName,
							"Error resolving array type for " + argName, ex);
				}
			}
			//没有获取到数组的类型，也没有获取到数组元素的类型
			//则直接设置数组的类型为Object
			else {
				elementType = Object.class;
			}
		}
		//创建指定类型的数组
		return resolveManagedArray(argName, (List<?>) value, elementType);
	}
	//解析list类型的属性值
	else if (value instanceof ManagedList) {
		// May need to resolve contained runtime references.
		return resolveManagedList(argName, (List<?>) value);
	}
	//解析set类型的属性值
	else if (value instanceof ManagedSet) {
		// May need to resolve contained runtime references.
		return resolveManagedSet(argName, (Set<?>) value);
	}
	//解析map类型的属性值
	else if (value instanceof ManagedMap) {
		// May need to resolve contained runtime references.
		return resolveManagedMap(argName, (Map<?, ?>) value);
	}
	//解析props类型的属性值，props其实就是key和value均为字符串的map
	else if (value instanceof ManagedProperties) {
		Properties original = (Properties) value;
		//创建一个拷贝，用于作为解析后的返回值
		Properties copy = new Properties();
		original.forEach((propKey, propValue) -> {
			if (propKey instanceof TypedStringValue) {
				propKey = evaluate((TypedStringValue) propKey);
			}
			if (propValue instanceof TypedStringValue) {
				propValue = evaluate((TypedStringValue) propValue);
			}
			if (propKey == null || propValue == null) {
				throw new BeanCreationException(
						this.beanDefinition.getResourceDescription(), this.beanName,
						"Error converting Properties key/value pair for " + argName + ": resolved to null");
			}
			copy.put(propKey, propValue);
		});
		return copy;
	}
	//解析字符串类型的属性值
	else if (value instanceof TypedStringValue) {
		// Convert value to target type here.
		TypedStringValue typedStringValue = (TypedStringValue) value;
		Object valueObject = evaluate(typedStringValue);
		try {
			//获取属性的目标类型
			Class<?> resolvedTargetType = resolveTargetType(typedStringValue);
			if (resolvedTargetType != null) {
				//对目标类型的属性进行解析，递归调用
				return this.typeConverter.convertIfNecessary(valueObject, resolvedTargetType);
			}
			//没有获取到属性的目标对象，则按Object类型返回
			else {
				return valueObject;
			}
		}
		catch (Throwable ex) {
			// Improve the message by showing the context.
			throw new BeanCreationException(
					this.beanDefinition.getResourceDescription(), this.beanName,
					"Error converting typed String value for " + argName, ex);
		}
	}
	else if (value instanceof NullBean) {
		return null;
	}
	else {
		return evaluate(value);
	}
}
```

```java
private Object resolveReference(Object argName, RuntimeBeanReference ref) {
    try {
    String refName = ref.getBeanName();
    refName = String.valueOf(doEvaluate(refName));
    if (ref.isToParent()) {
        if (this.beanFactory.getParentBeanFactory() == null) {
            throw new BeanCreationException("");
        }
        return this.beanFactory.getParentBeanFactory().getBean(refName);
    }else {
        Object bean = this.beanFactory.getBean(refName);
        this.beanFactory.registerDependentBean(refName, this.beanName);
        return bean;
    }
    }catch (BeansException ex) {throw new BeanCreationException(this.beanDefinition.getResourceDescription(), this.beanName,
                "Cannot resolve reference to bean '" + ref.getBeanName() + "' while setting " + argName,ex);
    }
}
```



##### 3.2 bw.setPropertyValues() 方法，完成对属性的注入操作

```java
protected void setPropertyValue(PropertyTokenHolder tokens, PropertyValue pv) throws BeansException {
    // 设置配置文件中的key:value对,例如.propertis中的属性
    if (tokens.keys != null) {
        processKeyedProperty(tokens, pv);
    }
    // 设置本地属性,简单属性注入，而 Array, Collection, Map 则走 processKeyedProperty 方法
    else {
        processLocalProperty(tokens, pv);
    }
}
```

```java
// 简单属性注入，而 Array, Collection, Map 则走 processKeyedProperty 方法
private void processLocalProperty(PropertyTokenHolder tokens, PropertyValue pv) {
    PropertyHandler ph = getLocalPropertyHandler(tokens.actualName);
    Object originalValue = pv.getValue();
    Object valueToApply = originalValue;
    // 1. 类型转换
    alueToApply = convertForProperty(
                    tokens.canonicalName, oldValue, originalValue, ph.toTypeDescriptor());
    // 2. 利用 JavaBean 的内省机制设置属性值
    ph.setValue(valueToApply);
}
```

> processLocalProperty 主要完成了两件事：一是类型转换；二是设置属性值。convertForProperty 利用 JDK 的 PropertyEditorSupport 进行类型转换，Spring 中内置了一批转换器，当然也可以自定义。而 setValue 则是使用反射进行赋值，关键代码如下：(BeanWrapperImpl#BeanPropertyHandler#setValue)

```java
public void setValue(final @Nullable Object value) throws Exception {
				//获取可写方法，例如javaBean中的setXXX()方法
    		final Method writeMethod = (this.pd instanceof GenericTypeAwarePropertyDescriptor ?
            ((GenericTypeAwarePropertyDescriptor) this.pd).getWriteMethodForActualAccess() :
            this.pd.getWriteMethod());
  
        ReflectionUtils.makeAccessible(writeMethod);
        // 调用Method方法的invoke方法,通过反射方法为bean设置值
        writeMethod.invoke(getWrappedInstance(), value);
}
```



```java
//实现属性依赖注入功能
@SuppressWarnings("unchecked")
private void processKeyedProperty(PropertyTokenHolder tokens, PropertyValue pv) {
	//调用属性的getter(readerMethod)方法，获取属性的值
	Object propValue = getPropertyHoldingValue(tokens);
	PropertyHandler ph = getLocalPropertyHandler(tokens.actualName);
	String lastKey = tokens.keys[tokens.keys.length - 1];

	//注入array类型的属性值
	if (propValue.getClass().isArray()) {
		Class<?> requiredType = propValue.getClass().getComponentType();
		int arrayIndex = Integer.parseInt(lastKey);
		Object oldValue = null;
		try {
			if (isExtractOldValueForEditor() && arrayIndex < Array.getLength(propValue)) {
				oldValue = Array.get(propValue, arrayIndex);
			}
			Object convertedValue = convertIfNecessary(tokens.canonicalName, oldValue, pv.getValue(),
					requiredType, ph.nested(tokens.keys.length));
			//获取集合类型属性的长度
			int length = Array.getLength(propValue);
			if (arrayIndex >= length && arrayIndex < this.autoGrowCollectionLimit) {
				Class<?> componentType = propValue.getClass().getComponentType();
				Object newArray = Array.newInstance(componentType, arrayIndex + 1);
				System.arraycopy(propValue, 0, newArray, 0, length);
				setPropertyValue(tokens.actualName, newArray);
				//调用属性的getter(readerMethod)方法，获取属性的值
				propValue = getPropertyValue(tokens.actualName);
			}
			//将属性的值赋值给数组中的元素
			Array.set(propValue, arrayIndex, convertedValue);
		}
	}

	//注入list类型的属性值
	else if (propValue instanceof List) {
		//获取list集合的类型
		Class<?> requiredType = ph.getCollectionType(tokens.keys.length);
		List<Object> list = (List<Object>) propValue;
		//获取list集合的size
		int index = Integer.parseInt(lastKey);
		Object oldValue = null;
		if (isExtractOldValueForEditor() && index < list.size()) {
			oldValue = list.get(index);
		}
		//获取list解析后的属性值
		Object convertedValue = convertIfNecessary(tokens.canonicalName, oldValue, pv.getValue(),
				requiredType, ph.nested(tokens.keys.length));
		int size = list.size();
		//如果list的长度大于属性值的长度，则多余的元素赋值为null
		if (index >= size && index < this.autoGrowCollectionLimit) {
			for (int i = size; i < index; i++) {
				try {
					list.add(null);
				}
			}
			list.add(convertedValue);
		}
		else {
			try {
				//将值添加到list中
				list.set(index, convertedValue);
			}
		}
	}

	//注入map类型的属性值
	else if (propValue instanceof Map) {
		//获取map集合key的类型
		Class<?> mapKeyType = ph.getMapKeyType(tokens.keys.length);
		//获取map集合value的类型
		Class<?> mapValueType = ph.getMapValueType(tokens.keys.length);
		Map<Object, Object> map = (Map<Object, Object>) propValue;
		// IMPORTANT: Do not pass full property name in here - property editors
		// must not kick in for map keys but rather only for map values.
		TypeDescriptor typeDescriptor = TypeDescriptor.valueOf(mapKeyType);
		//解析map类型属性key值
		Object convertedMapKey = convertIfNecessary(null, null, lastKey, mapKeyType, typeDescriptor);
		Object oldValue = null;
		if (isExtractOldValueForEditor()) {
			oldValue = map.get(convertedMapKey);
		}
		// Pass full property name and old value in here, since we want full
		// conversion ability for map values.
		//解析map类型属性value值
		Object convertedMapValue = convertIfNecessary(tokens.canonicalName, oldValue, pv.getValue(),
				mapValueType, ph.nested(tokens.keys.length));
		//将解析后的key和value值赋值给map集合属性
		map.put(convertedMapKey, convertedMapValue);
	}
}
```

## 3.AOP

![image-20210427142836848](picture/java框架.assets/image-20210427142836848.png)

![image-20210427142918057](picture/java框架.assets/image-20210427142918057.png)

AOP原理：

1.@EnableAspectJAutoProxy注入AspectJAutoProxyRegistrar

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(AspectJAutoProxyRegistrar.class)
public @interface EnableAspectJAutoProxy {

	boolean proxyTargetClass() default false;

	boolean exposeProxy() default false;

}
```

2.利用AspectJAutoProxyRegistrar给容器中导入组件AnnotationAwareAspectJAutoProxyCreator

```java
class AspectJAutoProxyRegistrar implements ImportBeanDefinitionRegistrar {

	@Override
	public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata) {
		AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry);
}
  
  public static BeanDefinition registerAspectJAnnotationAutoProxyCreatorIfNecessary
  (BeanDefinitionRegistry registry, Object source) {
        return registerOrEscalateApcAsRequired(AnnotationAwareAspectJAutoProxyCreator.class, registry, source);
}
  
  private static BeanDefinition registerOrEscalateApcAsRequired(Class<?> cls, BeanDefinitionRegistry registry, Object source) {
        if (registry.containsBeanDefinition("org.springframework.aop.config.internalAutoProxyCreator")) {
            BeanDefinition apcDefinition = registry.getBeanDefinition("org.springframework.aop.config.internalAutoProxyCreator");
            if (!cls.getName().equals(apcDefinition.getBeanClassName())) {
                int currentPriority = findPriorityForClass(apcDefinition.getBeanClassName());
                int requiredPriority = findPriorityForClass(cls);
                if (currentPriority < requiredPriority) {
                    apcDefinition.setBeanClassName(cls.getName());
                }
            }
            return null;
        }
    }
```

3.AnnotationAwareAspectJAutoProxyCreator的继承关系

```java
AnnotationAwareAspectJAutoProxyCreator

->AspectJAwareAdvisorAutoProxyCreator

->AbstractAdvisorAutoProxyCreator

->AbstractAutoProxyCreator implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware(重点关注后置处理器，在bean初始化完成前后做的事情)
```



# 熔断、降级、限流

## 服务熔断

​		在微服务架构中，微服务是完成一个单一的业务功能，这样做的好处是可以做到解耦，每个微服务可以独立演进。但是，一个应用可能会有多个微服务组成，微服务之间的数据交互通过远程过程调用完成。这就带来一个问题，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的“扇出”。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”。

![这里写图片描述](picture/框架.assets/20180331133509474)

​		熔断机制是应对雪崩效应的一种微服务链路保护机制。我们在各种场景下都会接触到熔断这两个字。高压电路中，如果某个地方的电压过高，熔断器就会熔断，对电路进行保护。股票交易中，如果股票指数过高，也会采用熔断机制，暂停股票的交易。

​		同样，在微服务架构中，熔断机制也是起着类似的作用。当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，进而熔断该节点微服务的调用，快速返回错误的响应信息。当检测到该节点微服务调用响应正常后，恢复调用链路。

在Spring Cloud框架里，熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败，就会启动熔断机制


## 降级

降级是指自己的待遇下降了，从RPC调用环节来讲，就是去访问一个本地的伪装者而不是真实的服务。

当双11活动时，**把无关交易的服务统统降级，如查看蚂蚁深林，查看历史订单，商品历史评论，只显示最后100条等等。**

**相同点：**

1. 目的很一致，都是从可用性可靠性着想，为防止系统的整体缓慢甚至崩溃，采用的技术手段；
   最终表现类似，对于两者来说，最终让用户体验到的是某些功能暂时不可达或不可用；

2. 粒度一般都是服务级别，当然，业界也有不少更细粒度的做法，比如做到数据持久层（允许查询，不允许增删改）；

3. 自治性要求很高，熔断模式一般都是服务基于策略的自动触发，降级虽说可人工干预，但在微服务架构下，完全靠人显然不可能，开关预置、配置中心都是必要手段；

**区别：**

1. 触发原因不太一样，服务熔断一般是某个服务（下游服务）故障引起，而服务降级一般是从整体负荷考虑；

2. 管理目标的层次不太一样，熔断其实是一个框架级的处理，每个微服务都需要（无层级之分），而降级一般需要对业务有层级之分（比如降级一般是从最外围服务开始）

3. 实现方式不太一样；服务降级具有代码侵入性(由控制器完成/或自动降级)，熔断一般称为自我熔断。
   

## 服务限流

在开发高并发系统时有三把利器用来保护系统：[**缓存、降级和限流**]()。

**缓存**的目的是提升系统访问速度和增大系统能处理的容量，可谓是抗高并发流量的银弹；

**降级**是当服务出问题或者影响到核心流程的性能则需要暂时屏蔽掉，待高峰或者问题解决后再打开；而有些场景并不能用缓存和降级来解决，比如稀缺资源（秒杀、抢购）、写服务（如评论、下单）、频繁的复杂查询（评论的最后几页），因此需有一种手段来限制这些场景的并发/请求量，即限流。

**限流**的目的是通过对并发访问/请求进行限速或者一个时间窗口内的的请求进行限速来保护系统，一旦达到限制速率则可以拒绝服务（定向到错误页或告知资源没有了）、排队或等待（比如秒杀、评论、下单）、降级（返回兜底数据或默认数据，如商品详情页库存默认有货）。

一般开发高并发系统常见的限流有：

> 1. 限制总并发数（比如数据库连接池、线程池）
>
> 2. 限制瞬时并发数（如nginx的limit_conn模块，用来限制瞬时并发连接数）
>
> 3. 限制时间窗口内的平均速率（如Guava的RateLimiter、nginx的limit_req模块，限制每秒的平均速率）；
>
> 4. 其他还有如限制远程接口调用速率、限制MQ的消费速率。
>
> 5. 另外还可以根据网络连接数、网络流量、CPU或内存负载等来限流。（资源角度）

### 限流算法

[**常见的限流算法有：令牌桶、漏桶。计数器也可以进行粗暴限流实现。**]()

#### 漏桶算法

漏桶(Leaky Bucket)算法思路很简单,水(请求)先进入到漏桶里,漏桶以一定的速度出水(接口有响应速率),当水流入速度过大会直接溢出(访问频率超过接口响应速率),然后就拒绝请求,可以看出漏桶算法能强行限制数据的传输速率.示意图如下:

![这里写图片描述](picture/框架.assets/20180331140456248)

#### 令牌桶算法

令牌桶算法(Token Bucket)和 Leaky Bucket [**效果一样但方向相反的算法**](),更加容易理解.随着时间流逝,系统会按恒定1/QPS时间间隔(如果QPS=100,则间隔是10ms)往桶里加入Token(想象和漏水相反,有个水龙头在不断的加水),如果桶已经满了就不再加了.新请求来临时,会各自拿走一个Token,如果没有Token可拿了就阻塞或者拒绝服务.

![这里写图片描述](picture/框架.assets/20180331140628686)

令牌桶的另外一个好处是[**可以方便的改变速度**](). 一旦需要提高速率,则按需提高放入桶中的令牌的速率. 一般会定时(比如100毫秒)往桶中增加一定数量的令牌, 有些变种算法则实时的计算应该增加的令牌的数量.

### 应用级限流

对于一个应用系统来说一定会有极限并发/请求数，即总有一个TPS/QPS阀值，如果超了阀值则系统就会不响应用户请求或响应的非常慢，因此我们最好进行过载保护，防止大量请求涌入击垮系统。

如果你使用过Tomcat，其Connector其中一种配置有如下几个参数：
acceptCount：如果Tomcat的线程都忙于响应，新来的连接会进入队列排队，如果超出排队大小，则拒绝连接；

maxConnections：瞬时最大连接数，超出的会排队等待；

maxThreads：Tomcat能启动用来处理请求的最大线程数，如果请求处理量一直远远大于最大线程数则可能会僵死。

另外如MySQL（如max_connections）、Redis（如tcp-backlog）都会有类似的[**限制连接数**]()的配置。

**池化技术**

如果有的资源是稀缺资源（如数据库连接、线程），而且可能有多个系统都会去使用它，那么需要限制应用；可以使用池化技术来限制总资源数：连接池、线程池。比如分配给每个应用的数据库连接是100，那么本应用最多可以使用100个资源，超出了可以等待或者抛异常。限流某个接口的总并发/请求数

**AtomicLong限流**

如果接口可能会有突发访问情况，但又担心访问量太大造成崩溃，如抢购业务；这个时候就需要限制这个接口的总并发/请求数总请求数了；因为粒度比较细，可以为每个接口都设置相应的阀值。可以使用Java中的AtomicLong进行限流：

```java
try {
if(atomic.incrementAndGet() > 限流数) {
//拒绝请求
    }
//处理请求
} finally {
    atomic.decrementAndGet();
}
```


参考：https://blog.csdn.net/g_hongjin/article/details/51649246

**分布式限流**

分布式限流最关键的是要将限流服务做成原子化，而解决方案可以使用redis+lua或者nginx+lua技术进行实现，通过这两种技术可以实现的高并发和高性能。

首先我们来使用redis+lua实现时间窗内某个接口的请求数限流，实现了该功能后可以改造为限流总并发/请求数和限制总资源数。Lua本身就是一种编程语言，也可以使用它实现复杂的令牌桶或漏桶算法。

有人会纠结如果应用并发量非常大那么redis或者nginx是不是能抗得住；不过这个问题要从多方面考虑：你的流量是不是真的有这么大，是不是可以通过一致性哈希将分布式限流进行分片，是不是可以当并发量太大降级为应用级限流；对策非常多，可以根据实际情况调节；像在京东使用Redis+Lua来限流抢购流量，一般流量是没有问题的。

对于分布式限流目前遇到的场景是业务上的限流，而不是流量入口的限流；流量入口限流应该在接入层完成，而接入层笔者一般使用Nginx。

**基于Redis功能的实现限流**

> 参考：https://www.cnblogs.com/exceptioneye/p/4783904.html
> 简陋的设计思路：假设一个用户（用IP判断）每分钟访问某一个服务接口的次数不能超过10次，那么我们可以在Redis中创建一个键，并此时我们就设置键的过期时间为60秒，每一个用户对此服务接口的访问就把键值加1，在60秒内当键值增加到10的时候，就禁止访问服务接口。在某种场景中添加访问时间间隔还是很有必要的。

**基于令牌桶算法的实现**

令牌桶算法最初来源于计算机网络。在网络传输数据时，为了防止网络拥塞，需限制流出网络的流量，使流量以比较均匀的速度向外发送。[**令牌桶算法就实现了这个功能，可控制发送到网络上数据的数目，并允许突发数据的发送。**]()

令牌桶算法是网络流量整形（Traffic Shaping）和速率限制（Rate Limiting）中最常使用的一种算法。典型情况下，令牌桶算法用来控制发送到网络上的数据的数目，并允许突发数据的发送。

大小固定的令牌桶可自行以恒定的速率源源不断地产生令牌。如果令牌不被消耗，或者被消耗的速度小于产生的速度，令牌就会不断地增多，直到把桶填满。后面再产生的令牌就会从桶中溢出。最后桶中可以保存的最大令牌数永远不会超过桶的大小。

传送到令牌桶的数据包需要消耗令牌。不同大小的数据包，消耗的令牌数量不一样。

令牌桶这种控制机制基于令牌桶中是否存在令牌来指示什么时候可以发送流量。令牌桶中的每一个令牌都代表一个字节。如果令牌桶中存在令牌，则允许发送流量；而如果令牌桶中不存在令牌，则不允许发送流量。因此，如果突发门限被合理地配置并且令牌桶中有足够的令牌，那么流量就可以以峰值速率发送。

<img src="picture/框架.assets/20160423213519927" alt="img" style="zoom:67%;" />


算法描述：

假如用户配置的平均发送速率为r，则每隔1/r秒一个令牌被加入到桶中（每秒会有r个令牌放入桶中）；

假设桶中最多可以存放b个令牌。如果令牌到达时令牌桶已经满了，那么这个令牌会被丢弃；

当一个n个字节的数据包到达时，就从令牌桶中删除n个令牌（不同大小的数据包，消耗的令牌数量不一样），并且数据包被发送到网络；

如果令牌桶中少于n个令牌，那么不会删除令牌，并且认为这个数据包在流量限制之外（n个字节，需要n个令牌。该数据包将被缓存或丢弃）；

算法允许最长b个字节的突发，但从长期运行结果看，数据包的速率被限制成常量r。对于在流量限制外的数据包可以以不同的方式处理：（1）它们可以被丢弃；（2）它们可以排放在队列中以便当令牌桶中累积了足够多的令牌时再传输；（3）它们可以继续发送，但需要做特殊标记，网络过载的时候将这些特殊标记的包丢弃。

Java实现
[**我们可以使用Guava 的 RateLimiter 来实现基于令牌桶的流控，RateLimiter 令牌桶算法是单桶实现。**]()RateLimiter 对简单的令牌桶算法做了一些工程上的优化，具体的实现是 SmoothBursty。需要注意的是，RateLimiter 的另一个实现SmoothWarmingUp，就不是令牌桶了，而是漏桶算法。也许是出于简单起见，RateLimiter 中的时间窗口能且仅能为 1s。

SmoothBursty 有一个可以放 N 个时间窗口产生的令牌的桶，系统空闲的时候令牌就一直攒着，最好情况下可以扛 N 倍于限流值的高峰而不影响后续请求。RateLimite允许某次请求拿走超出剩余令牌数的令牌，但是下一次请求将为此付出代价，一直等到令牌亏空补上，并且桶中有足够本次请求使用的令牌为止。当某次请求不能得到所需要的令牌时，这时涉及到一个权衡，是让前一次请求干等到令牌够用才走掉呢，还是让它先走掉后面的请求等一等呢？Guava 的设计者选择的是后者，先把眼前的活干了，后面的事后面再说。

# MyBatis

## 一级缓存和二级缓存概述

> 当在一个缓存作用域中发生了update、insert、delete 动作后，将会触发缓存失效，下一次查询将命中数据库，从而保证不会查到脏数据。

### 一级缓存

　　Mybatis对缓存提供支持，但是在没有配置的默认情况下，它只开启一级缓存，一级缓存只是相对于同一个SqlSession而言。所以在参数和SQL完全一样的情况下，我们使用同一个SqlSession对象调用一个Mapper方法，往往只执行一次SQL，因为使用SelSession第一次查询后，MyBatis会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，SqlSession都会取出当前缓存的数据，而不会再次发送SQL到数据库。

　　　　　　　　　　　　　　![img](picture/java框架.assets/1254583-20171026214546023-1354746770.png)

　　**1、一级缓存的生命周期有多长？**

```
　 a、MyBatis在开启一个数据库会话时，会 创建一个新的SqlSession对象，SqlSession对象中会有一个新的Executor对象。Executor对象中持有一个新的PerpetualCache对象；当会话结束时，SqlSession对象及其内部的Executor对象还有PerpetualCache对象也一并释放掉。

　　b、如果SqlSession调用了close()方法，会释放掉一级缓存PerpetualCache对象，一级缓存将不可用。

　　c、如果SqlSession调用了clearCache()，会清空PerpetualCache对象中的数据，但是该对象仍可使用。

　　d、SqlSession中执行了任何一个update操作(update()、delete()、insert()) ，都会清空PerpetualCache对象的数据，但是该对象可以继续使用
```

　  **2、怎么判断某两次查询是完全相同的查询？**

```
　　mybatis认为，对于两次查询，如果以下条件都完全一样，那么就认为它们是完全相同的两次查询。

　　2.1 传入的statementId

　　2.2 查询时要求的结果集中的结果范围

　　2.3. 这次查询所产生的最终要传递给JDBC java.sql.Preparedstatement的Sql语句字符串（boundSql.getSql() ）

　　2.4 传递给java.sql.Statement要设置的参数值
```

#### 示例

```java
/**
     * 开启事务，测试一级缓存效果
     * 缓存命中顺序：二级缓存---> 一级缓存---> 数据库
     **/
    @Test
    @Transactional(rollbackFor = Throwable.class)
    public void testFistCache(){
        // 第一次查询，缓存到一级缓存
        userMapper.selectById(1);
        // 第二次查询，直接读取一级缓存
        userMapper.selectById(1);

    }
```

执行结果：
<img src="picture/java框架.assets/1181064-20191116235612319-87021059.png" alt="img" style="zoom: 67%;" />

可以看到，虽然进行了两次查询，但最终只请求了一次数据库，第二次查询命中了一级缓存，直接返回了数据。
这里有两点需要说明一下：

1、为什么开启事务

> 由于使用了数据库连接池，默认每次查询完之后自动commit，这就导致两次查询使用的不是同一个sqlSessioin，根据一级缓存的原理，它将永远不会生效。
> 当我们开启了事务，两次查询都在同一个sqlSession中，从而让第二次查询命中了一级缓存。读者可以自行关闭事务验证此结论。

2、两种一级缓存模式

> 一级缓存的作用域有两种：session（默认）和statment，可通过设置local-cache-scope 的值来切换，默认为session。
> 二者的区别在于session会将缓存作用于同一个sqlSesson，而statment仅针对一次查询，所以，local-cache-scope: statment可以理解为关闭一级缓存。

### 二级缓存

　　MyBatis的二级缓存是Application级别的缓存，它可以提高对数据库查询的效率，以提高应用的性能。二级缓存可通过cache-ref让多个mapper.xml共享同一namespace，从而实现缓存共享。

![img](picture/java框架.assets/1254583-20171029185910164-1823278112.png)

　　SqlSessionFactory层面上的二级缓存默认是不开启的，二级缓存的开席需要进行配置，实现二级缓存的时候，MyBatis要求返回的POJO必须是可序列化的。 也就是要求实现Serializable接口，配置方法很简单，只需要在映射XML文件配置就可以开启缓存了<cache/>，如果我们配置了二级缓存就意味着：

- 映射语句文件中的所有select语句将会被缓存。
- 映射语句文件中的所欲insert、update和delete语句会刷新缓存。
- 缓存会使用默认的Least Recently Used（LRU，最近最少使用的）算法来收回。
- 根据时间表，比如No Flush Interval,（CNFI没有刷新间隔），缓存不会以任何时间顺序来刷新。
- 缓存会存储列表集合或对象(无论查询方法返回什么)的1024个引用
- 缓存会被视为是read/write(可读/可写)的缓存，意味着对象检索不是共享的，而且可以安全的被调用者修改，不干扰其他调用者或线程所做的潜在修改。

## 一级缓存和二级缓存

### 前言

MyBatis是常见的Java数据库访问层框架。在日常工作中，开发人员多数情况下是使用MyBatis的默认缓存配置，但是MyBatis缓存机制有一些不足之处，在使用中容易引起脏数据，形成一些潜在的隐患。个人在业务开发中也处理过一些由于MyBatis缓存引发的开发问题，带着个人的兴趣，希望从应用及源码的角度为读者梳理MyBatis缓存机制。

本次分析中涉及到的代码和数据库表均放在GitHub上，地址： [mybatis-cache-demo](https://github.com/kailuncen/mybatis-cache-demo) 。

### 目录

本文按照以下顺序展开。

- 一级缓存介绍及相关配置。
- 一级缓存工作流程及源码分析。
- 一级缓存总结。
- 二级缓存介绍及相关配置。
- 二级缓存源码分析。
- 二级缓存总结。
- 全文总结。

### 一级缓存

#### 一级缓存介绍

在应用运行过程中，我们有可能在一次数据库会话中，执行多次查询条件完全相同的SQL，MyBatis提供了一级缓存的方案优化这部分场景，如果是相同的SQL语句，会优先命中一级缓存，避免直接对数据库进行查询，提高性能。具体执行过程如下图所示。

<img src="picture/java框架.assets/6e38df6a.jpg" alt="img" style="zoom: 50%;" />

每个SqlSession中持有了Executor，每个Executor中有一个LocalCache。当用户发起查询时，MyBatis根据当前执行的语句生成`MappedStatement`，在Local Cache进行查询，如果缓存命中的话，直接返回结果给用户，如果缓存没有命中的话，查询数据库，结果写入`Local Cache`，最后返回结果给用户。具体实现类的类关系图如下图所示。

![img](picture/java框架.assets/d76ec5fe.jpg)

#### 一级缓存配置

我们来看看如何使用MyBatis一级缓存。开发者只需在MyBatis的配置文件中，添加如下语句，就可以使用一级缓存。共有两个选项，`SESSION`或者`STATEMENT`，默认是`SESSION`级别，即在一个MyBatis会话中执行的所有语句，都会共享这一个缓存。一种是`STATEMENT`级别，可以理解为缓存只对当前执行的这一个`Statement`有效。

```xml
<setting name="localCacheScope" value="SESSION"/>
```

#### 一级缓存实验

接下来通过实验，了解MyBatis一级缓存的效果，每个单元测试后都请恢复被修改的数据。

首先是创建示例表student，创建对应的POJO类和增改的方法，具体可以在entity包和mapper包中查看。

```sql
CREATE TABLE `student` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(200) COLLATE utf8_bin DEFAULT NULL,
  `age` tinyint(3) unsigned DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```

在以下实验中，id为1的学生名称是凯伦。

##### 实验1

开启一级缓存，范围为会话级别，调用三次`getStudentById`，代码如下所示：

```java
public void getStudentById() throws Exception {
        SqlSession sqlSession = factory.openSession(true); // 自动提交事务
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        System.out.println(studentMapper.getStudentById(1));
        System.out.println(studentMapper.getStudentById(1));
        System.out.println(studentMapper.getStudentById(1));
    }
```

执行结果：

![img](picture/java框架.assets/9e996384.jpg)

我们可以看到，只有第一次真正查询了数据库，后续的查询使用了一级缓存。

##### 实验2

增加了对数据库的修改操作，验证在一次数据库会话中，如果对数据库发生了修改操作，一级缓存是否会失效。

```java
@Test
public void addStudent() throws Exception {
        SqlSession sqlSession = factory.openSession(true); // 自动提交事务
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        System.out.println(studentMapper.getStudentById(1));
        System.out.println("增加了" + studentMapper.addStudent(buildStudent()) + "个学生");
        System.out.println(studentMapper.getStudentById(1));
        sqlSession.close();
}
```

执行结果：

![img](picture/java框架.assets/fb6a78e0.jpg)

我们可以看到，在修改操作后执行的相同查询，查询了数据库，**一级缓存失效**。

##### 实验3

开启两个`SqlSession`，在`sqlSession1`中查询数据，使一级缓存生效，在`sqlSession2`中更新数据库，验证一级缓存只在数据库会话内部共享。

```java
@Test
public void testLocalCacheScope() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 

        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);

        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        System.out.println("studentMapper2更新了" + studentMapper2.updateStudentName("小岑",1) + "个学生的数据");
        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentById(1));
}
```

![img](picture/java框架.assets/f480ac76.jpg)

`sqlSession2`更新了id为1的学生的姓名，从凯伦改为了小岑，但session1之后的查询中，id为1的学生的名字还是凯伦，出现了脏数据，也证明了之前的设想，一级缓存只在数据库会话内部共享。

#### 一级缓存工作流程&源码分析

那么，一级缓存的工作流程是怎样的呢？我们从源码层面来学习一下。

##### 工作流程

一级缓存执行的时序图，如下图所示。

![img](picture/java框架.assets/bb851700.png)

##### 源码分析

接下来将对MyBatis查询相关的核心类和一级缓存的源码进行走读。这对后面学习二级缓存也有帮助。

**SqlSession**： 对外提供了用户和数据库之间交互需要的所有方法，隐藏了底层的细节。默认实现类是`DefaultSqlSession`。

![img](picture/java框架.assets/ba96bc7f.jpg)

**Executor**： `SqlSession`向用户提供操作数据库的方法，但和数据库操作有关的职责都会委托给Executor。

![img](picture/java框架.assets/ef5e0eb3.jpg)

如下图所示，Executor有若干个实现类，为Executor赋予了不同的能力，大家可以根据类名，自行学习每个类的基本作用。

![img](picture/java框架.assets/83326eb3.jpg)

在一级缓存的源码分析中，主要学习`BaseExecutor`的内部实现。

**BaseExecutor**： `BaseExecutor`是一个实现了Executor接口的抽象类，定义若干抽象方法，在执行的时候，把具体的操作委托给子类进行执行。

```java
protected abstract int doUpdate(MappedStatement ms, Object parameter) throws SQLException;
protected abstract List<BatchResult> doFlushStatements(boolean isRollback) throws SQLException;
protected abstract <E> List<E> doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) throws SQLException;
protected abstract <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql) throws SQLException;
```

在一级缓存的介绍中提到对`Local Cache`的查询和写入是在`Executor`内部完成的。在阅读`BaseExecutor`的代码后发现`Local Cache`是`BaseExecutor`内部的一个成员变量，如下代码所示。

```java
public abstract class BaseExecutor implements Executor {
protected ConcurrentLinkedQueue<DeferredLoad> deferredLoads;
protected PerpetualCache localCache;
```

**Cache**： MyBatis中的Cache接口，提供了和缓存相关的最基本的操作，如下图所示：

<img src="picture/java框架.assets/793031d0.jpg" alt="img" style="zoom:50%;" />

有若干个实现类，使用装饰器模式互相组装，提供丰富的操控缓存的能力，部分实现类如下图所示：

![img](picture/java框架.assets/cdb21712.jpg)

`BaseExecutor`成员变量之一的`PerpetualCache`，是对Cache接口最基本的实现，其实现非常简单，内部持有HashMap，[**对一级缓存的操作实则是对HashMap的操作**]()。如下代码所示：

```java
public class PerpetualCache implements Cache {
  private String id;
  private Map<Object, Object> cache = new HashMap<Object, Object>();
```

在阅读相关核心类代码后，从源代码层面对一级缓存工作中涉及到的相关代码，出于篇幅的考虑，对源码做适当删减，读者朋友可以结合本文，后续进行更详细的学习。

为执行和数据库的交互，首先需要初始化`SqlSession`，通过`DefaultSqlSessionFactory`开启`SqlSession`：

```java
private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
    ............
    final Executor executor = configuration.newExecutor(tx, execType);     
    return new DefaultSqlSession(configuration, executor, autoCommit);
}
```

在初始化`SqlSesion`时，会使用`Configuration`类创建一个全新的`Executor`，作为`DefaultSqlSession`构造函数的参数，创建Executor代码如下所示：

```java
public Executor newExecutor(Transaction transaction, ExecutorType executorType) {
    executorType = executorType == null ? defaultExecutorType : executorType;
    executorType = executorType == null ? ExecutorType.SIMPLE : executorType;
    Executor executor;
    if (ExecutorType.BATCH == executorType) {
      executor = new BatchExecutor(this, transaction);
    } else if (ExecutorType.REUSE == executorType) {
      executor = new ReuseExecutor(this, transaction);
    } else {
      executor = new SimpleExecutor(this, transaction);
    }
    // 尤其可以注意这里，如果二级缓存开关开启的话，是使用CahingExecutor装饰BaseExecutor的子类
    if (cacheEnabled) {
      executor = new CachingExecutor(executor);                      
    }
    executor = (Executor) interceptorChain.pluginAll(executor);
    return executor;
}
```

`SqlSession`创建完毕后，根据Statment的不同类型，会进入`SqlSession`的不同方法中，如果是`Select`语句的话，最后会执行到`SqlSession`的`selectList`，代码如下所示：

```java
@Override
public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
      MappedStatement ms = configuration.getMappedStatement(statement);
      return executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER);
}
```

`SqlSession`把具体的查询职责委托给了Executor。如果只开启了一级缓存的话，首先会进入`BaseExecutor`的`query`方法。代码如下所示：

```java
@Override
public <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException {
    BoundSql boundSql = ms.getBoundSql(parameter);
    CacheKey key = createCacheKey(ms, parameter, rowBounds, boundSql);
    return query(ms, parameter, rowBounds, resultHandler, key, boundSql);
}
```

在上述代码中，会先根据传入的参数生成CacheKey，进入该方法查看CacheKey是如何生成的，代码如下所示：

```java
CacheKey cacheKey = new CacheKey();
cacheKey.update(ms.getId());
cacheKey.update(rowBounds.getOffset());
cacheKey.update(rowBounds.getLimit());
cacheKey.update(boundSql.getSql());
//后面是update了sql中带的参数
cacheKey.update(value);
```

在上述的代码中，将`MappedStatement`的Id、SQL的offset、SQL的limit、SQL本身以及SQL中的参数传入了CacheKey这个类，最终构成CacheKey。以下是这个类的内部结构：

```java
private static final int DEFAULT_MULTIPLYER = 37;
private static final int DEFAULT_HASHCODE = 17;

private int multiplier;
private int hashcode;
private long checksum;
private int count;
private List<Object> updateList;

public CacheKey() {
    this.hashcode = DEFAULT_HASHCODE;
    this.multiplier = DEFAULT_MULTIPLYER;
    this.count = 0;
    this.updateList = new ArrayList<Object>();
}
```

首先是成员变量和构造函数，有一个初始的`hachcode`和乘数，同时维护了一个内部的`updatelist`。在`CacheKey`的`update`方法中，会进行一个`hashcode`和`checksum`的计算，同时把传入的参数添加进`updatelist`中。如下代码所示：

```java
public void update(Object object) {
    int baseHashCode = object == null ? 1 : ArrayUtil.hashCode(object); 
    count++;
    checksum += baseHashCode;
    baseHashCode *= count;
    hashcode = multiplier * hashcode + baseHashCode;
    
    updateList.add(object);
}
```

同时重写了`CacheKey`的`equals`方法，代码如下所示：

```java
@Override
public boolean equals(Object object) {
    .............
    for (int i = 0; i < updateList.size(); i++) {
      Object thisObject = updateList.get(i);
      Object thatObject = cacheKey.updateList.get(i);
      if (!ArrayUtil.equals(thisObject, thatObject)) {
        return false;
      }
    }
    return true;
}
```

除去hashcode、checksum和count的比较外，只要updatelist中的元素一一对应相等，那么就可以认为是CacheKey相等。只要两条SQL的下列五个值相同，即可以认为是相同的SQL。

> Statement Id + Offset + Limmit + Sql + Params

BaseExecutor的query方法继续往下走，代码如下所示：

```java
list = resultHandler == null ? (List<E>) localCache.getObject(key) : null;
if (list != null) {
    // 这个主要是处理存储过程用的。
    handleLocallyCachedOutputParameters(ms, key, parameter, boundSql);
    } else {
    list = queryFromDatabase(ms, parameter, rowBounds, resultHandler, key, boundSql);
}
```

如果查不到的话，就从数据库查，在`queryFromDatabase`中，会对`localcache`进行写入。

在`query`方法执行的最后，会判断一级缓存级别是否是`STATEMENT`级别，如果是的话，就清空缓存，这也就是`STATEMENT`级别的一级缓存无法共享`localCache`的原因。代码如下所示：

```java
if (configuration.getLocalCacheScope() == LocalCacheScope.STATEMENT) {
        clearLocalCache();
}
```

在源码分析的最后，我们确认一下，如果是`insert/delete/update`方法，缓存就会刷新的原因。

`SqlSession`的`insert`方法和`delete`方法，都会统一走`update`的流程，代码如下所示：

```java
@Override
public int insert(String statement, Object parameter) {
    return update(statement, parameter);
  }
   @Override
  public int delete(String statement) {
    return update(statement, null);
}
```

`update`方法也是委托给了`Executor`执行。`BaseExecutor`的执行方法如下所示：

```java
@Override
public int update(MappedStatement ms, Object parameter) throws SQLException {
    ErrorContext.instance().resource(ms.getResource()).activity("executing an update").object(ms.getId());
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    clearLocalCache();
    return doUpdate(ms, parameter);
}
```

每次执行`update`前都会清空`localCache`。

至此，一级缓存的工作流程讲解以及源码分析完毕。

#### 总结

1. MyBatis一级缓存的生命周期和SqlSession一致。
2. MyBatis一级缓存内部设计简单，只是一个没有容量限定的HashMap，在缓存的功能性上有所欠缺。
3. MyBatis的一级缓存最大范围是SqlSession内部，有多个SqlSession或者分布式的环境下，数据库写操作会引起脏数据，建议设定缓存级别为Statement。

### 二级缓存

#### 二级缓存介绍

在上文中提到的一级缓存中，其最大的共享范围就是一个SqlSession内部，如果多个SqlSession之间需要共享缓存，则需要使用到二级缓存。开启二级缓存后，会使用CachingExecutor装饰Executor，进入一级缓存的查询流程前，先在CachingExecutor进行二级缓存的查询，具体的工作流程如下所示。

![img](picture/java框架.assets/28399eba.png)

二级缓存开启后，同一个namespace下的所有操作语句，都影响着同一个Cache，即二级缓存被多个SqlSession共享，是一个全局的变量。

当开启缓存后，数据的查询执行的流程就是 二级缓存 -> 一级缓存 -> 数据库。

#### 二级缓存配置

要正确的使用二级缓存，需完成如下配置的。

1. 在MyBatis的配置文件中开启二级缓存。

```xml
<setting name="cacheEnabled" value="true"/>
```

1. 在MyBatis的映射XML中配置cache或者 cache-ref 。

cache标签用于声明这个namespace使用二级缓存，并且可以自定义配置。

```xml
<cache/>   
```

- `type`：cache使用的类型，默认是`PerpetualCache`，这在一级缓存中提到过。
- `eviction`： 定义回收的策略，常见的有FIFO，LRU。
- `flushInterval`： 配置一定时间自动刷新缓存，单位是毫秒。
- `size`： 最多缓存对象的个数。
- `readOnly`： 是否只读，若配置可读写，则需要对应的实体类能够序列化。
- `blocking`： 若缓存中找不到对应的key，是否会一直blocking，直到有对应的数据进入缓存。

`cache-ref`代表引用别的命名空间的Cache配置，两个命名空间的操作使用的是同一个Cache。

```xml
<cache-ref namespace="mapper.StudentMapper"/>
```

#### 二级缓存实验

接下来我们通过实验，了解MyBatis二级缓存在使用上的一些特点。

在本实验中，id为1的学生名称初始化为点点。

##### 实验1

测试二级缓存效果，不提交事务，`sqlSession1`查询完数据后，`sqlSession2`相同的查询是否会从缓存中获取数据。

```java
@Test
public void testCacheWithoutCommitOrClose() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 
        
        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);

        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentById(1));
}
```

执行结果：

![img](picture/java框架.assets/71e2bfdc.jpg)

我们可以看到，当`sqlsession`没有调用`commit()`方法时，二级缓存并没有起到作用。

##### 实验2

测试二级缓存效果，当提交事务时，`sqlSession1`查询完数据后，`sqlSession2`相同的查询是否会从缓存中获取数据。

```java
@Test
public void testCacheWithCommitOrClose() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 
        
        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);

        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        sqlSession1.commit();
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentById(1));
}
```

![img](picture/java框架.assets/f366f34e.jpg)

从图上可知，`sqlsession2`的查询，使用了缓存，缓存的命中率是0.5。

##### 实验3

测试`update`操作是否会刷新该`namespace`下的二级缓存。

```java
@Test
public void testCacheWithUpdate() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 
        SqlSession sqlSession3 = factory.openSession(true); 
        
        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);
        StudentMapper studentMapper3 = sqlSession3.getMapper(StudentMapper.class);
        
        System.out.println("studentMapper读取数据: " + studentMapper.getStudentById(1));
        sqlSession1.commit();
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentById(1));
        
        studentMapper3.updateStudentName("方方",1);
        sqlSession3.commit();
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentById(1));
}
```

![img](picture/java框架.assets/3ad93c3a.jpg)

我们可以看到，在`sqlSession3`更新数据库，并提交事务后，`sqlsession2`的`StudentMapper namespace`下的查询走了数据库，没有走Cache。

##### 实验4

验证MyBatis的二级缓存不适应用于映射文件中存在多表查询的情况。

通常我们会为每个单表创建单独的映射文件，由于MyBatis的二级缓存是基于`namespace`的，多表查询语句所在的`namspace`无法感应到其他`namespace`中的语句对多表查询中涉及的表进行的修改，引发脏数据问题。

```java
@Test
public void testCacheWithDiffererntNamespace() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 
        SqlSession sqlSession3 = factory.openSession(true); 
    
        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);
        ClassMapper classMapper = sqlSession3.getMapper(ClassMapper.class);
        
        System.out.println("studentMapper读取数据: " + studentMapper.getStudentByIdWithClassInfo(1));
        sqlSession1.close();
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentByIdWithClassInfo(1));

        classMapper.updateClassName("特色一班",1);
        sqlSession3.commit();
        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentByIdWithClassInfo(1));
}
```

执行结果：

![img](picture/java框架.assets/5265ed97.jpg)

在这个实验中，我们引入了两张新的表，一张class，一张classroom。class中保存了班级的id和班级名，classroom中保存了班级id和学生id。我们在`StudentMapper`中增加了一个查询方法`getStudentByIdWithClassInfo`，用于查询学生所在的班级，涉及到多表查询。在`ClassMapper`中添加了`updateClassName`，根据班级id更新班级名的操作。

当`sqlsession1`的`studentmapper`查询数据后，二级缓存生效。保存在StudentMapper的namespace下的cache中。当`sqlSession3`的`classMapper`的`updateClassName`方法对class表进行更新时，`updateClassName`不属于`StudentMapper`的`namespace`，所以`StudentMapper`下的cache没有感应到变化，没有刷新缓存。当`StudentMapper`中同样的查询再次发起时，从缓存中读取了脏数据。

##### 实验5

为了解决实验4的问题呢，可以使用Cache ref，让`ClassMapper`引用`StudenMapper`命名空间，这样两个映射文件对应的SQL操作都使用的是同一块缓存了。

执行结果：

![img](picture/java框架.assets/a2e4c2d8.jpg)

不过这样做的后果是，缓存的粒度变粗了，多个`Mapper namespace`下的所有操作都会对缓存使用造成影响。

#### 二级缓存源码分析

MyBatis二级缓存的工作流程和前文提到的一级缓存类似，只是在一级缓存处理前，用`CachingExecutor`装饰了`BaseExecutor`的子类，在委托具体职责给`delegate`之前，实现了二级缓存的查询和写入功能，具体类关系图如下图所示。

![img](picture/java框架.assets/090216b1.jpg)

##### 源码分析

源码分析从`CachingExecutor`的`query`方法展开，源代码走读过程中涉及到的知识点较多，不能一一详细讲解，读者朋友可以自行查询相关资料来学习。

`CachingExecutor`的`query`方法，首先会从`MappedStatement`中获得在配置初始化时赋予的Cache。

```java
Cache cache = ms.getCache();
```

本质上是装饰器模式的使用，具体的装饰链是：

> SynchronizedCache -> LoggingCache -> SerializedCache -> LruCache -> PerpetualCache。

![img](picture/java框架.assets/1f5233b2.jpg)

以下是具体这些Cache实现类的介绍，他们的组合为Cache赋予了不同的能力。

- `SynchronizedCache`：同步Cache，实现比较简单，直接使用synchronized修饰方法。
- `LoggingCache`：日志功能，装饰类，用于记录缓存的命中率，如果开启了DEBUG模式，则会输出命中率日志。
- `SerializedCache`：序列化功能，将值序列化后存到缓存中。该功能用于缓存返回一份实例的Copy，用于保存线程安全。
- `LruCache`：采用了Lru算法的Cache实现，移除最近最少使用的Key/Value。
- `PerpetualCache`： 作为为最基础的缓存类，底层实现比较简单，直接使用了HashMap。

然后是判断是否需要刷新缓存，代码如下所示：

```java
flushCacheIfRequired(ms);
```

在默认的设置中`SELECT`语句不会刷新缓存，`insert/update/delte`会刷新缓存。进入该方法。代码如下所示：

```java
private void flushCacheIfRequired(MappedStatement ms) {
    Cache cache = ms.getCache();
    if (cache != null && ms.isFlushCacheRequired()) {      
      tcm.clear(cache);
    }
}
```

MyBatis的`CachingExecutor`持有了`TransactionalCacheManager`，即上述代码中的tcm。

`TransactionalCacheManager`中持有了一个Map，代码如下所示：

```java
private Map<Cache, TransactionalCache> transactionalCaches = new HashMap<Cache, TransactionalCache>();
```

这个Map保存了Cache和用`TransactionalCache`包装后的Cache的映射关系。

`TransactionalCache`实现了Cache接口，`CachingExecutor`会默认使用他包装初始生成的Cache，作用是如果事务提交，对缓存的操作才会生效，如果事务回滚或者不提交事务，则不对缓存产生影响。

在`TransactionalCache`的clear，有以下两句。清空了需要在提交时加入缓存的列表，同时设定提交时清空缓存，代码如下所示：

```java
@Override
public void clear() {
	clearOnCommit = true;
	entriesToAddOnCommit.clear();
}
```

`CachingExecutor`继续往下走，`ensureNoOutParams`主要是用来处理存储过程的，暂时不用考虑。

```java
if (ms.isUseCache() && resultHandler == null) {
	ensureNoOutParams(ms, parameterObject, boundSql);
```

之后会尝试从tcm中获取缓存的列表。

```java
List<E> list = (List<E>) tcm.getObject(cache, key);
```

在`getObject`方法中，会把获取值的职责一路传递，最终到`PerpetualCache`。如果没有查到，会把key加入Miss集合，这个主要是为了统计命中率。

```java
Object object = delegate.getObject(key);
if (object == null) {
	entriesMissedInCache.add(key);
}
```

`CachingExecutor`继续往下走，如果查询到数据，则调用`tcm.putObject`方法，往缓存中放入值。

```java
if (list == null) {
	list = delegate.<E> query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
	tcm.putObject(cache, key, list); // issue #578 and #116
}
```

tcm的`put`方法也不是直接操作缓存，只是在把这次的数据和key放入待提交的Map中。

```java
@Override
public void putObject(Object key, Object object) {
    entriesToAddOnCommit.put(key, object);
}
```

从以上的代码分析中，我们可以明白，如果不调用`commit`方法的话，由于`TranscationalCache`的作用，并不会对二级缓存造成直接的影响。因此我们看看`Sqlsession`的`commit`方法中做了什么。代码如下所示：

```java
@Override
public void commit(boolean force) {
    try {
      executor.commit(isCommitOrRollbackRequired(force));
```

因为我们使用了CachingExecutor，首先会进入CachingExecutor实现的commit方法。

```java
@Override
public void commit(boolean required) throws SQLException {
    delegate.commit(required);
    tcm.commit();
}
```

会把具体commit的职责委托给包装的`Executor`。主要是看下`tcm.commit()`，tcm最终又会调用到`TrancationalCache`。

```java
public void commit() {
    if (clearOnCommit) {
      delegate.clear();
    }
    flushPendingEntries();
    reset();
}
```

看到这里的`clearOnCommit`就想起刚才`TrancationalCache`的`clear`方法设置的标志位，真正的清理Cache是放到这里来进行的。具体清理的职责委托给了包装的Cache类。之后进入`flushPendingEntries`方法。代码如下所示：

```java
private void flushPendingEntries() {
    for (Map.Entry<Object, Object> entry : entriesToAddOnCommit.entrySet()) {
      delegate.putObject(entry.getKey(), entry.getValue());
    }
    ................
}
```

在`flushPending`Entries中，将待提交的Map进行循环处理，委托给包装的Cache类，进行`putObject`的操作。

后续的查询操作会重复执行这套流程。如果是`insert|update|delete`的话，会统一进入`CachingExecutor`的`update`方法，其中调用了这个函数，代码如下所示：

```java
private void flushCacheIfRequired(MappedStatement ms) 
```

在二级缓存执行流程后就会进入一级缓存的执行流程，因此不再赘述。

#### 总结

1. MyBatis的二级缓存相对于一级缓存来说，实现了`SqlSession`之间缓存数据的共享，同时粒度更加的细，能够到`namespace`级别，通过Cache接口实现类不同的组合，对Cache的可控性也更强。
2. MyBatis在多表查询时，极大可能会出现脏数据，有设计上的缺陷，安全使用二级缓存的条件比较苛刻。
3. 在分布式环境下，由于默认的MyBatis Cache实现都是基于本地的，分布式环境下必然会出现读取到脏数据，需要使用集中式缓存将MyBatis的Cache接口实现，有一定的开发成本，直接使用Redis、Memcached等分布式缓存可能成本更低，安全性也更高。

### 全文总结

本文对介绍了MyBatis一二级缓存的基本概念，并从应用及源码的角度对MyBatis的缓存机制进行了分析。最后对MyBatis缓存机制做了一定的总结，个人建议MyBatis缓存特性在生产环境中进行关闭，单纯作为一个ORM框架使用可能更为合适。



# 过滤器和拦截器的区别

![img](picture/java框架.assets/330611-20171023144517066-24770749.png)

**①拦截器是基于java的反射机制的，而过滤器是基于函数回调。**

![img](picture/java框架.assets/330611-20171023150730676-300068307.png)

