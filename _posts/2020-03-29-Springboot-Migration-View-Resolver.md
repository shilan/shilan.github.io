---
title: Migration to Spring Boot, View Resolver
author: Shilan Kalhor
date: 2020-06-29 12:34:00 +0800
categories: [Coding, Springboot, migration, legacy spring, ViewResolver, JSP]
tags: [Springboot, migration, java, legacy spring, ViewResolver, JSP]
toc: false
---

You wanna migrate your legacy java jsp servlet base application to Spring boot 2 and Java the latest? Though you should get ready beforehand but don't get intimidated!
When we wanted to migrate our giant 7 years old application to Spring boot 6 months ago, his blog post was the best covering document.
But still we had our own challenges too.
Now that we are in production finally I am gonna write few posts how we resolved our problems.
## Today is about JSP view resolver
You might have noticed in Spring Boot migration document mentioned that you can easily set the application.properties to 
```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
```
while this is true and works for simple scenarios, what if you needed more than one ViewResolver? What if you had to customize the forwards and includes like we had to do?
Assume your legacy code JSP folder is not centeralized in one place. In our application forexample we had two different placeholders for our JSP files. One directly under WEB-INF,
the other one under webapp.
In this case you should programetically access those either by setting a view resolver in your dispatcher-servlet.xml file, (if you are still using xml-based configuration)
like below:
```
    <bean id="firstViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/myapp/jsp/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>
```

Or define them as two separate beans with two different names:

```
@Bean(name="firstViewResolver")
public ViewResolver getFirstViewResolver(){
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/jsp/");
    resolver.setSuffix(".jsp");
    resolver.setViewClass(JstlView.class);
    return resolver;
}

@Bean(name="secondViewResolver")
public ViewResolver getSecondViewResolver(){
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/myapp/jsp/");
    resolver.setSuffix(".jsp");
    resolver.setViewClass(JstlView.class);
    return resolver;
}
```
Beside this issue, we encountered another problem. It seems that Spring boot has changed the Include/Forward behaviour and by default changes 
Include requests to Forward. Which was causing a problem for our application.
It is also mentioned in InternalResourceViewResolver javadoc:
"Specify whether to always include the view rather than forward to it.
Default is "false". Switch this flag on to enforce the use of a
Servlet include, even if a forward would be possible.
"
To fix that we need to set ``alwaysInclude`` to true, otherwise spring boot changes dispatcherType of dispatches to jsps to FORWARD automatically.
There is no application property for setting it in configuration file but now that we have our customized ViewResolver we can easily set it in our xml:
```
    <!-- Page-level JSP views are managed through spring internal viewResolver.  -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/myapp/jsp/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
        <property name="alwaysInclude">
        	<value>true</value>
        </property>
        <property name="contentType">
        	<value>text/html;charset=UTF-8</value>
        </property>
    </bean>
``` 
or just ``resolver.setAlwaysInclude(true)`` in our bean definition.

Not here I have also set the contentType to UTF-8 as I noticed that I missing UTF8 formatting in some of the Forwards and Includes.


