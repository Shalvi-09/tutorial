
[https://www.logicbig.com/tutorials/spring-framework/spring-core/alias-for-annotation.html](https://www.logicbig.com/tutorials/spring-framework/spring-core/alias-for-annotation.html)



# Using anotation and conditionals with annotation in spring

## Spring - Understanding @AliasFor annotation

`@AliasFor` annotation is used to declare aliases for annotation elements. Spring framework uses this annotation internally with a lot of other annotations, for example, @Bean, @ComponentScan, @Scope etc.

Following is the snippet of `@AliasFor` definition:

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Documented
public @interface AliasFor {
	@AliasFor("attribute")
	String value() default "";
	@AliasFor("value")
	String attribute() default "";
	Class<? extends Annotation> annotation() default Annotation.class;
}

```


In this tutorial, we are going to understand the usage of @AliasFor by creating our own annotations.

## Aliasing within an annotation
In the following example, we are going to declare aliases between the elements of an annotation.

```

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface AccessRole {

  @AliasFor("accessType")
  String value() default "visitor";

  @AliasFor("value")
  String accessType() default "visitor";

  String module() default "gui";
}

```
```
@AccessRole("super-user")
public class MyObject1 {
}
```

Creating and using annotations do not do anything unless we process them. Spring core provides `AnnotatedElementUtils` for that purpose. We can even use it outside of Spring container.

```
@Configuration
public class ExampleAliasFor {

  public static void main(String[] args) {
      AnnotationAttributes aa = AnnotatedElementUtils
              .getMergedAnnotationAttributes(MyObject1.class, AccessRole.class);
      System.out.println("Attributes of AccessRole used on MyObject1: " + aa);
  }
}

//output
Attributes of AccessRole used on MyObject1: {value=super-user, module=gui, accessType=super-user}

```

Note that both 'value' and 'accessType' are populated with same value even though we specified only 'value' on MyObject1. That is because the both elements are aliases of each other.

### Different defaults for alias references are not allowed

Alias references cannot have different default values, otherwise `AnnotationConfigurationException` will be thrown as shown in the following example.


```

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface AccessRole2 {

  @AliasFor("accessType")
  String value() default "visitor";

  @AliasFor("value")
  String accessType() default "admin";

  String module() default "gui";
}

@AccessRole2("super-user")
public class MyObject3 {
}

@Configuration
public class ExampleAliasForDifferentDefaults {

  public static void main(String[] args) {
      AnnotationAttributes aa = AnnotatedElementUtils
              .getMergedAnnotationAttributes(MyObject3.class, AccessRole2.class);
      System.out.println("Attributes of AccessRole3 used on MyObject3: " + aa);
  }
}
```

```
Caused by: org.springframework.core.annotation.AnnotationConfigurationException: Misconfigured aliases: attribute 'value' in annotation [com.logicbig.example.AccessRole2] and attribute 'accessType' in annotation [com.logicbig.example.AccessRole2] must declare the same default value.
	at org.springframework.core.annotation.AnnotationUtils$AliasDescriptor.validateDefaultValueConfiguration(AnnotationUtils.java:2133)
	at org.springframework.core.annotation.AnnotationUtils$AliasDescriptor.validate(AnnotationUtils.java:2111)
	at org.springframework.core.annotation.AnnotationUtils$AliasDescriptor.from(AnnotationUtils.java:2034)
	at org.springframework.core.annotation.AnnotationUtils.getAttributeAliasNames(AnnotationUtils.java:1703)
	at org.springframework.core.annotation.AnnotationUtils.getAttributeAliasMap(AnnotationUtils.java:1616)
	at org.springframework.core.annotation.AnnotationUtils.postProcessAnnotationAttributes(AnnotationUtils.java:1245)
	at org.springframework.core.annotation.AnnotatedElementUtils.getMergedAnnotationAttributes(AnnotatedElementUtils.java:339)
	at com.logicbig.example.ExampleAliasForDifferentDefaults.main(ExampleAliasForDifferentDefaults.java:13)
	... 6 more

```

## Aliasing attributes from a meta-annotation

Annotating a new annotation definition with an existing annotation is known as meta-annotation. Just like we can specify an alias for another element within the same annotation, we can also do the same for a meta-annotation but we have to additionally specify 'annotation' as shown in the following example.

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@AccessRole("admin")
public @interface AdminAccess {
  @AliasFor(annotation = AccessRole.class, attribute = "module")
  String value() default "service";
}

@AdminAccess
public class MyObject2 {
}

@Configuration
public class ExampleAliasForMetaAnnotation {

  public static void main(String[] args) {
      AnnotationAttributes aa = AnnotatedElementUtils
              .getMergedAnnotationAttributes(MyObject2.class, AdminAccess.class);
      System.out.println("attributes of AdminAccess used on MyObject2 " + aa);

      aa = AnnotatedElementUtils
              .getMergedAnnotationAttributes(MyObject2.class, AccessRole.class);
      System.out.println("attributes of AccessRole used on MyObject2 " + aa);
  }
}
```

output
```
attributes of AdminAccess used on MyObject2 {value=service}
attributes of AccessRole used on MyObject2 {value=admin, module=service, accessType=admin}
```
As seen in above output, we can get attributes of both @AdminAccess and @AccessRole, even though we specified only @AdminAccess on MyObject2. The important thing is, the attributes of the meta-annotation are overridden by the target annotation, which is a very useful feature of Spring's meta-annotation programming model.










