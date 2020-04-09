[https://www.baeldung.com/spring-boot-add-filter](https://www.baeldung.com/spring-boot-add-filter)

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



