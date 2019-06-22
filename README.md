# Headline

> An awesome project.

# 自动装配bean

这一次，我们主要学习@Autowired注解装备bean。

话不多说，先上一段例子。

  

## 1、bean代码实例

```

//xml配置bean

<bean id="CustomerBean" class="com.yiibai.common.Customer">

<property name="action" value="buy" />

<property name="type" value="1" />

</bean>

<bean id="PersonBean" class="com.yiibai.common.Person">

<property name="name" value="yiibai" />

<property name="address" value="address 123" />

<property name="age" value="28" />

</bean>

```

```

package com.yiibai.common;

public class Customer

{

    private Person person;

    private int type;

    private String action;

}

```

```

package com.yiibai.common;

public class Person

{

    private String name;

private String address;

private int age;

}

```

  

## 2、代码分析

我们这边声明了2个bean，分别对应了一个customer对象和person对象。

  

逻辑上，customer对象拥有person对象的引用。

所以，在装配customer对象时，除了会注入CustomerBean的两个基本数据类型属性，还需要把PersonBean注入到customer的属性person中。

  

问题来了，怎么把PersonBean注入到person属性中呢？

首先我们回忆下平常如何给一个属性赋值：1、构造器中注明参数类型 2、set方法中注明参数类型。

  

没错，spring也是从这两方面入手，只不过它不再需要显式地调用这两个方法。他可以根据参数类型查找合适的bean，也可以根据参数名查找合适的bean。总之，在通过某种方法找到合适的bean后，就在该对象被创建时注入到属性中。

  
  

## 3、@Autowired的引入

@Autowired注解是一种相对简单高效的匹配bean的方法。使用它还需要在配置文件中增加一些内容，具体有两种方法，我这里介绍其中之一。

  

加上这句代码就可以了：

  

```

<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor">

</bean>

```

  

## 4、@Autowired的位置

除了第2点中介绍的构造器和set方法，@Autowired注解还支持属性名。作为初学者很容易分不清这三种方法，但其实说白了是一回事。

  

如果我们在属性名上方添加了@Autowired注解，就没必要在构造器和set方法上方添加@Autowired注解了；其他两者也是同理。

这三种方法都可以成功地注入一个对象所需要的bean，甚至可以通用，具体用哪个看心情就好。

  

但这里有一个坑，如果你在练习时使用了属性上的注解，并且没有添加对应的set方法或者构造函数，那么编译器会报错。因为属性注解只是一种简单的表现形式，实际上我们都知道不可能直接凭空给一个属性安排上值的，它还是得借助另外两个老大哥。

  

## 5、特殊情况装配

### @Qualifier

有时候，我们会申明多个同类型的bean。@Autowired就晕了，它不知道到底要装配哪个bean。使用@Qualifier（"xxx"）可以告诉@Autowired就装配我括号里点名的那个bean。

@@Qualifier（"xxx"）的位置一般放在@Autowired的下方。

  

### required属性

有时候，@Autowired找不到对应的bean。出现这种情况也是很合理的，@Autowired会毫不留情地抛出一个异常，告诉我们找不到。这样也挺好，但如果你不希望它抛异常，你可以设置required属性为false。这就告诉@Autowired，我不需要这个对象了，你能装上最好，装不上就拉倒吧，空着就行。

  
  

## 6、配置bean的变形

### 内部bean

内部bean适用在某个bean对象只在另一个bean对象内部被使用。

举例如下，bean中bean。

```

<bean id="CustomerBean" class="com.yiibai.common.Customer">

<property name="action" value="buy"></property>

<property name="type" value="1"></property>

<property name="person">

<bean id="PersonBean" class="com.yiibai.common.Person">

<property name="name" value="wuyu"></property>

<property name="address" value="neu"></property>

<property name="age" value="28"></property>

</bean>

</property>

</bean>

```

### ref

有人说，我不想要在java代码里写东西，那推荐你用ref属性。Person虽然不同于基本数据类型，但本质差不多，也可以在CustomerBean里面添加property标签。

举例如下。

```

<bean id="CustomerBean" class="com.yiibai.common.Customer">

<property name="action" value="buy"></property>

<property name="type" value="1"></property>

<property name="person" ref="PersonBean">

</property>

</bean>

  

<bean id="PersonBean" class="com.yiibai.common.Person">

<property name="name" value="wuyu1"></property>

<property name="address" value="neu1"></property>

<property name="age" value="28"></property>

</bean>

```

  

### 加载多个配置

为了项目结构更好，有的人把不同模块的bean放到不同的xml文件里。但是在引入的时候比较麻烦，需要读取好几个xml文件。

解决方法是最后整一个全部模块的xml文件，里面不放别的东西，就放import。

把所有的xml文件整合到一起，最后只需要读这个大文件就行了。

---

学习链接：
https://www.yiibai.com/spring/spring-autowiring-qualifier-example.html