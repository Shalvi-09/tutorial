[https://www.baeldung.com/spring-boot-add-filter](https://www.baeldung.com/spring-boot-add-filter)


[https://www.concretepage.com/spring-boot/spring-boot-filter](https://www.concretepage.com/spring-boot/spring-boot-filter)

## When is filter executed and when is Interceptor executed

![](https://synaren-app.com/images/spring-filter-interceptor-flow.png)

A Filter can be registered using either FilterRegistrationBean class or `@Component` annotation or `@ServletComponentScan` annotation. FilterRegistrationBean registers a Filter as Spring bean and it provides methods to add URL mappings, set Filter order etc. When we register a Filter using Spring `@Component`, we can set Filter order using Spring `@Order` annotation but there is no way to change default URL mappings in this case. When we register Filter using `@ServletComponentScan`, our filters must be annotated with `@WebFilter` annotation and we can add URL mappings using its urlPatterns attribute but we cannot set Filter order in this case. `@ServletComponentScan` works __only when using embedded server__. Here on this page we will provide complete Spring Boot Filter example with filters, servlets and Spring controller.


## Register Filter with FilterRegistrationBean
`FilterRegistrationBean` registers filters in __Servlet 3.0 + container__. FilterRegistrationBean registers a filter as Spring bean. Find some of its methods.
setFilter(): Sets filter object.
addUrlPatterns(): Adds filter URL mappings.
setOrder(): Sets filter order.

Find the JavaConfig to register ABCFilter and XYZFilter classes.

```
@Configuration
public class WebConfig {
   //Register ABCFilter 	
   @Bean
   public FilterRegistrationBean<ABCFilter> abcFilter() {
	   FilterRegistrationBean<ABCFilter> filterRegBean = new FilterRegistrationBean<>();
	   filterRegBean.setFilter(new ABCFilter());
	   filterRegBean.addUrlPatterns("/app/*");
	   filterRegBean.setOrder(Ordered.LOWEST_PRECEDENCE -1);
	   return filterRegBean;
   }
   //Register XYZFilter   
   @Bean
   public FilterRegistrationBean<XYZFilter> xyzFilter() {
	   FilterRegistrationBean<XYZFilter> filterRegBean = new FilterRegistrationBean<>();
	   filterRegBean.setFilter(new XYZFilter());
	   filterRegBean.addUrlPatterns("/app/*");	
	   filterRegBean.setOrder(Ordered.LOWEST_PRECEDENCE -2);
	   return filterRegBean;
   }   
   ------
} 

```

### Filter URL Patterns
To add filter URL patterns we can use `addUrlPatterns()` or `setUrlPatterns()` methods of `FilterRegistrationBean`. Suppose we want to define URL patterns as /app1/* and /app2/* using addUrlPatterns(), it is achieved as following.

```
filterRegBean.addUrlPatterns("/app1/*", "/app2/*"); 
```
If we are using setUrlPatterns(), it is achieved as following.

```
filterRegBean.setUrlPatterns(Arrays.asList("/app1/*", "/app2/*")); 

```

### Filter Order
When we register filters using `FilterRegistrationBean`, we can set filter order using its `setOrder()` method. Find the code snippet.

```
filterRegBean.setOrder(Ordered.LOWEST_PRECEDENCE); 
```

`Ordered.HIGHEST_PRECEDENCE`: This is the highest precedence.
`Ordered.LOWEST_PRECEDENCE`: This is the lowest precedence.

Lower the order number, higher the precedence. Find the sample precedence order.
__Example-1:__
```
Ordered.LOWEST_PRECEDENCE -2 > Ordered.LOWEST_PRECEDENCE -1
```
__Example-2:__
```
Ordered.HIGHEST_PRECEDENCE +1 > Ordered.HIGHEST_PRECEDENCE +2
```
It is usually safe to leave the filter beans unordered. Spring boot provides them default order and that is usually Ordered.LOWEST_PRECEDENCE. If we want to run our custom filters before or after any in-built filter such as Spring security filter, we need to order them using FilterRegistrationBean. It means if we want to run our custom filter after Spring security filter, we need to create our own FilterRegistrationBean for Spring security filter and specify the order.


## Register Filter with @Component and @Order
We can register a filter using `@Component` and set order using `@Order`. Create a filter implementing Java Filter and annotate it with Spring `@Component` as following.

```

@Order(Ordered.LOWEST_PRECEDENCE -1)
@Component
public class ABCFilter implements Filter {
  ------
} 


```

```

@Order(Ordered.LOWEST_PRECEDENCE -2)
@Component
public class XYZFilter implements Filter {
  ------
}
```
When we register filter using @Component, we can use Spring @Order annotation to set filter order as

```
@Order(Ordered.LOWEST_PRECEDENCE) 
```

__Filter URL Patterns cannot be set with @Component__

The default filter URL pattern is "/*". We cannot change it in case we register filter with @Component annotation. If we want filter URL mappings, we should register filters using FilterRegistrationBean.

## Register Filter with @ServletComponentScan and @WebFilter
To register a filter in Spring Boot, we can use `@ServletComponentScan` and the filter should be annotated with `@WebFilter` annotation. We need to use `@ServletComponentScan` with `@Configuration` or `@SpringBootApplication` annotations. `@ServletComponentScan` in Spring Boot will scan servlets annotated with `@WebServlet`, filters annotated with `@WebFilter` and listeners annotated with `@WebListener` only when using an embedded web server. Suppose we have two filters annotated with `@WebFilter` as following.

```
@WebFilter(urlPatterns="/app/*")
public class ABCFilter implements Filter {
  ------
} 

```

```
@WebFilter(urlPatterns="/app/*")
public class XYZFilter implements Filter {
  ------
} 
```

Now use Spring boot starter Main class with@ServletComponentScan to scan above filters.

```
package com.concretepage;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@ServletComponentScan
@SpringBootApplication
public class SpringBootAppStarter {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootAppStarter.class, args);
    }
} 

```

### Filter URL Patterns
The annotation `@WebFilter` has the attribute urlPatterns to map URLs as following.

```
@WebFilter(urlPatterns= {"/app1/*", "/app2/*"})
public class ABCFilter implements Filter {
   ------
} 
```

### Filter Order cannot be set with @WebFilter
When we register filters using `@WebFilter` then we cannot order them in Spring Boot. `@WebFilter` does not provide any attribute to set order. We also cannot use Spring `@Order` annotation, because Spring does not identify `@WebFilter` annotated class as Spring bean. `@WebFilter` is a Java annotation and not a Spring annotation. If we want to set order we should register our filters using `FilterRegistrationBean`.





# How to Define a Spring Boot Filter?


In this quick tutorial, we'll explore how to define custom filters and specify their invocation order with the help of Spring Boot.

## 2. Defining Filters and the Invocation Order
Let's start by creating two filters:

1. TransactionFilter – to start and commit transactions
2. RequestResponseLoggingFilter – to log requests and responses
In order to create a filter, we need to simply implement the Filter interface:

```
@Component
@Order(1)
public class TransactionFilter implements Filter {
 
    @Override
    public void doFilter
      ServletRequest request, 
      ServletResponse response, 
      FilterChain chain) throws IOException, ServletException {
  
        HttpServletRequest req = (HttpServletRequest) request;
        LOG.info(
          "Starting a transaction for req : {}", 
          req.getRequestURI());
  
        chain.doFilter(request, response);
        LOG.info(
          "Committing a transaction for req : {}", 
          req.getRequestURI());
    }
 
    // other methods 
}
```

```
@Component
@Order(2)
public class RequestResponseLoggingFilter implements Filter {
 
    @Override
    public void doFilter(
      ServletRequest request, 
      ServletResponse response, 
      FilterChain chain) throws IOException, ServletException {
  
        HttpServletRequest req = (HttpServletRequest) request;
        HttpServletResponse res = (HttpServletResponse) response;
        LOG.info(
          "Logging Request  {} : {}", req.getMethod(), 
          req.getRequestURI());
        chain.doFilter(request, response);
        LOG.info(
          "Logging Response :{}", 
          res.getContentType());
    }
 
    // other methods
}
```

In order for Spring to be able to recognize a filter, we needed to define it as a bean with the `@Component` annotation.

__And, to have the filters fire in the right order – we needed to use the @Order annotation.__

### 2.1. Filter With URL Pattern

In the example above, our filters are registered by default for all the URL's in our application. However, we may sometimes want a filter to only apply to certain URL patterns.

In this case, we have to remove the @Component annotation from the filter class definition and __register the filter using a FilterRegistrationBean:__



