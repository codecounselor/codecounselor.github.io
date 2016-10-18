---
layout: post
section-type: post
title: Implementing a fully Java based Spring MVC Context inside a XML based root context
category: Technology
tags: [ 'software', 'coding']
---

# Situation

You're working with a legacy system that is monolithic (i.e. one WAR, no microservice architecture) which is currently configured using a web.xml file and one or more Spring XML context definitions.  You have a root spring context and potentially one or more child contexts declared using a `DispatcherServlet`.  Perhaps for a REST service endpoint, like this:

```xml
    <servlet>
        <servlet-name>rest-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
        	<param-name>contextConfigLocation</param-name>
        	<param-value>/WEB-INF/spring-rest.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>rest-dispatcher</servlet-name>
        <url-pattern>/api/*</url-pattern>
    </servlet-mapping>
```

# Problem

I see a number of problems with this approach:

1. Even if you are able to separate the code into distinct modules, the web.xml file inside the war module creates a strong coupling between the root context (war module) and the child (service module)
1. Subsequently, you sacrifice portability since
    - (a) the service module (a jar) can not just be dropped into a root context (a war) without manual configuration in the web.xml.
    - (b) the module can not run standalone without declaring its own web.xml
1. Mixing a context defined in XML with annotated Java Beans for the same module is confusing and messy.
1. Testing using the Spring Test Framework is very powerful.  However, you may not be able nor want to load the entire spring application context.  In this scenario, you need a way to have Spring inject/autowire your mock dependencies for you.


# Solution

Most material available covers either a XML based approach, or a code based approach using Spring Boot.  Spring Boot is great because it does a lot of the heavy lifting for you out of the box.  But when dealing with a legacy app you may need to migrate more gradually.  

The following steps will help you to incrementally implement, extract, or refactor isolated contexts from your existing app.  Which will in turn help you to plainly see logical components or microservices within the monolithic system.

1. Declare the configuration for the servlet container of the child context via code in `RestApiWebInitializer.java`
2. Declare the Spring MVC Configuration via code


## Declare the Servlet Container (DispatcherServlet) in Java

```java
public class RestApiWebInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected String getServletName() {
        return "rest-api";
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] {RestApiWebConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/api/v1/*" };
    }

    @Override
    protected Filter[] getServletFilters() {
        return new Filter[] { new CORSFilter() };
    }
}
```

* `null` must be returned from `getRootConfigClasses` or an exception will occur stating you attempted to define multiple root contexts.
* It is important to override `getServletName` if you plan to implement spring security anywhere outside of the root context.  This is explained in more detail in Appendix A.
* You may list more than one `servlet config class`, but I preferred to use `@Import` annotations in my primary web configuation class.

In order to not reinvent the wheel, [here is a good detailed explanation of `AbstractAnnotationConfigDispatcherServletInitializer`](http://joshlong.com/jl/blogPost/simplified_web_configuration_with_spring.html).

## Declare the Spring MVC Configuration via code

```java
@Configuration
@EnableWebMvc
@Import({SecurityConfig.class, SwaggerWebConfig.class})
@ComponentScan(basePackages = {"org.codecounselor.springmvcdemo.api"} )
public class FlexAPIV1WebConfig {

    @Bean
    public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
        PropertySourcesPlaceholderConfigurer configurer = new PropertySourcesPlaceholderConfigurer();
        //configurer.setIgnoreUnresolvablePlaceholders(true);
        //configurer.setIgnoreResourceNotFound(true);
        return configurer;
    }
}
```

* This class represents the entry point for the Sprint MVC configuration for the context, hence the `@EnableWebMvc` annotation.
* `@Import` is used here instead of listing these inside of `RestApiWebInitializer`, to provide better **encapsulation and separation of concerns**.  It would not be good if the servlet initializer had knowledge of how the MVC components needed to be configured.
* If you intend to use `@PropertySource` annotations then you must declare a **static** `PropertySourcesPlaceholderConfigurer` bean, as  [Spring authors have made the decision](https://jira.spring.io/browse/SPR-8539) to not automatically inject this for you.
* Some will suggest separating your RestController configuration from the rest of the beans and using inclusion and/or exclusion filters on the component scan.
* I left the configurer options commented here in the event you would like to ignore any unfound resources.

## Testing The Implementation
TODO

### Testing SecurityConfig
TODO

### Testing Controllers
TODO

# Appendix A - Setting up Spring Security

Problems you may encounter:

1. `Exception starting filter springSecurityFilterChain: org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'springSecurityFilterChain' is defined`

## Implementing JWT


https://www.toptal.com/java/rest-security-with-jwt-spring-security-and-java

# Appendix B - Managing Beans Programmatically

Before landing on the solution of using Mockito for integration tests an attempt was made to programmatically
inject mocks generated from JMockit.  Ultimately, this did not work out of the box because the mocks are not available
before wiring is performed.  But here is some of the code which allows you to interact with bean definitions:

```
ConfigurableListableBeanFactory beanFactory = ((ConfigurableApplicationContext) applicationContext).getBeanFactory();
String beanClass = UserProfileService.class.getCanonicalName();
((DefaultListableBeanFactory)applicationContext.getAutowireCapableBeanFactory()).destroySingleton(beanClass);
beanFactory.registerSingleton(beanClass, userProfileService);

//BeanDefinition bd = beanFactory.getBeanDefinition("getMockUserProfileService");
// Don't want to delete the definition because that gets rid of the qualifier, just swap in the new instance only
//((BeanDefinitionRegistry)applicationContext.getAutowireCapableBeanFactory()).removeBeanDefinition("getMockUserProfileService");
```
