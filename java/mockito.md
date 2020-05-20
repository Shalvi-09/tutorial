


Mocking is a way to test the functionality of a class in isolation. Mocking does not require a database connection or properties file read or file server read to test a functionality. Mock objects do the mocking of the real service. A mock object returns a dummy data corresponding to some dummy input passed to it.

## Mockito JUnit integration
we'll learn how to integrate JUnit and Mockito together.


#### MockitoJUnitRunner @RunWith
If you are using @Mock or @InjectsMocks i.e Mockito annotations then you should specific type of Runner. 
The first option we have is to annotate the JUnit test with a MockitoJUnitRunner as in the following example:


```
@RunWith(MockitoJUnitRunner.class)
public class MyTest{

//...
}
```

## Mockito - Adding Behavior

Mockito adds a functionality to a mock object using the methods `when() and thenReturn(..)`.  It means that when the mock object (addService) is called for add method with (num1, num2) parameters, then it returns the value stored in the expected variable. Take a look at the following code snippet.

```
when(calcService.add(10.0,20.0)).thenReturn(30.00);
```
Here we've instructed Mockito to give a behavior of adding 10 and 20 to the add method of calcService and as a result, to return the value of 30.00.

## Mockito - Verifying Behavior
Mockito can ensure whether a mock method is being called with reequired arguments or not. It is done using the verify() method.

Mockito framework keeps track of all the method calls and their parameters to the mock object. Mockito verify() method on the mock object verifies that a method is called with certain parameters. We can also specify the number of invocation logic, such as the exact number of times, at least specified number of times, less than the specified number of times, etc. We can use VerificationModeFactory for number of invocation times logic.

Mockito verify() method checks that a method is called with the right parameters. It does not check the result of a method call like assert method. 

```
//test the add functionality
Assert.assertEquals(calcService.add(10.0, 20.0),30.0,0);

//verify call to calcService is made or not with same arguments.
verify(calcService).add(10.0, 20.0);
```

## Mockito - Expecting Calls
Mockito provides a special check on the number of calls that can be made on a particular method. Suppose MathApplication should call the CalculatorService.serviceUsed() method only once, then it should not be able to call CalculatorService.serviceUsed() more than once.

```
//add the behavior of calc service to add two numbers
when(calcService.add(10.0,20.0)).thenReturn(30.00);

//limit the method call to 1, no less and no more calls are allowed
verify(calcService, times(1)).add(10.0, 20.0);
```

### Mockito provides the following additional methods to vary the expected call counts.

  *   atLeast (int min) − expects min calls.
  
  *  atLeastOnce () − expects at least one call.

  *  atMost (int max) − expects max calls.

## Mockito - Exception Handling

Mockito provides the capability to a mock to throw exceptions, so exception handling can be tested. Take a look at the following code snippet.

```
doThrow(new Runtime Exception("divide operation not implemented"))
   .when(calcService).add(10.0,20.0);
   ```

## stubbing with generic interface

Mockito provides a Answer interface which allows stubbing with generic interface.

```
//add the behavior to add numbers
when(calcService.add(20.0,10.0)).thenAnswer(new Answer<Double>() {
   @Override
   public Double answer(InvocationOnMock invocation) throws Throwable {
      //get the arguments passed to mock
      Object[] args = invocation.getArguments();
      //get the mock 
      Object mock = invocation.getMock();	
      //return the result
      return 30.0;
   }
});
```

## Mockito - Resetting Mock

Mockito provides the capability to a reset a mock so that it can be reused later. Take a look at the following code snippet.
```
//reset mock
reset(calcService);
```

Here we've reset mock object. MathApplication makes use of calcService and after reset the mock, using mocked method will fail the test.

## Mockito - Behavior Driven Development (BDD)
Behavior Driven Development is a style of writing tests uses given, when and then format as test methods. Mockito provides special methods to do so. Take a look at the following code snippet.

```
//Given
given(calcService.add(20.0,10.0)).willReturn(30.0);

//when
double result = calcService.add(20.0,10.0);

//then
Assert.assertEquals(result,30.0,0);	
  ```
  Here we're using given method of BDDMockito class instead of when method of .
  
## Mockito Annotations - @Mock, @Runwith, @InjectMocks and @Captor

### Enable Mockito Annotations
Before we go further, let's explore different ways to enable the use of annotations with Mockito tests.

#### MockitoJUnitRunner @RunWith
If you are using @Mock or @InjectsMocks i.e Mockito annotations then you should specific type of Runner. 
The first option we have is to annotate the JUnit test with a MockitoJUnitRunner as in the following example:


```
@RunWith(MockitoJUnitRunner.class)
public class MyTest{

//...
}
```

#### MockitoAnnotations.initMocks()

Alternatively, we can enable Mockito annotations programmatically as well, by invoking `MockitoAnnotations.initMocks()`:
```
@Before
public void init() {
    MockitoAnnotations.initMocks(this);
}
```

#### MockitoJUnit.rule()
Lastly, we can use a MockitoJUnit.rule() as shown below:

```
public class MockitoInitWithMockitoJUnitRuleUnitTest {
 
    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();
 
    ...
}
```

### @Mock

The most used widely used annotation in Mockito is @Mock. We can use @Mock to create and inject mocked instances without having to
call `Mockito.mock` manually.

```
@Test
public void whenNotUseMockAnnotation_thenCorrect() {
    List mockList = Mockito.mock(ArrayList.class);
     
    mockList.add("one");
    Mockito.verify(mockList).add("one");
    assertEquals(0, mockList.size());
 
    Mockito.when(mockList.size()).thenReturn(100);
    assertEquals(100, mockList.size());
}
```
And now we'll do the same but we'll inject the mock using the @Mock annotation:
```
@Mock
List<String> mockedList;
 
@Test
public void whenUseMockAnnotation_thenMockIsInjected() {
    mockedList.add("one");
    Mockito.verify(mockedList).add("one");
    assertEquals(0, mockedList.size());
 
    Mockito.when(mockedList.size()).thenReturn(100);
    assertEquals(100, mockedList.size());
}
```


`@Mock` Annotation behaves exactly same as static function `Mockito.mock(MyClassName.class)` and there is no other difference.
It just makes code redable and nothing else.

You should have seen like this `employeeServiceMock = Mockito.mock(EmployeeService.class);`. The mockito annotation `@Mock` does the same
but it uses annotation instead.

so you can use it like
```
@RunWith(MockitoJUnitRunner.class)
public class MyTest{

@Mock
private EmployeeService employeeServiceMock;

    @Before
    public void mySetup(){

        //Below line not require any more
        //  employeeServiceMock = Mockito.mock(EmployeeService.class);

    }
  //...
}

```

### @Captor
Mockito ArgumentCaptor is used to capture arguments for mocked methods. ArgumentCaptor is used with Mockito `verify()` methods
We can create ArgumentCaptor instance for any class, then its capture() method is used with verify() methods.

Finally, we can get the captured arguments from getValue() and getAllValues() methods.

getValue() method can be used when we have captured a single argument. If the verified method was called multiple times then getValue() method will return the latest captured value.

If multiple arguments are captured, call getAllValues() to get the list of arguments.

In the following example – we create an ArgumentCaptor with the old way without using @Captor annotation:
```
@Test
public void whenNotUseCaptorAnnotation_thenCorrect() {
    List mockList = Mockito.mock(List.class);
    ArgumentCaptor<String> arg = ArgumentCaptor.forClass(String.class);
 
    mockList.add("one");
    Mockito.verify(mockList).add(arg.capture());
 
    assertEquals("one", arg.getValue());
}
```

Let's now make use of @Captor for the same purpose – to create an ArgumentCaptor instance:
```
@Mock
List mockedList;
 
@Captor
ArgumentCaptor argCaptor;
 
@Test
public void whenUseCaptorAnnotation_thenTheSam() {
    mockedList.add("one");
    Mockito.verify(mockedList).add(argCaptor.capture());
 
    assertEquals("one", argCaptor.getValue());
}
```
### @InjectMocks

Actually `@InjectMock` helps in resolving the dependencies of a mocked class. Say for Example: you have `EmployeeService` class with methods and instance variables like 
```
public class EmployeeService{

private EmployeeRepository repository;

public EmployeeService(EmployeeRepository repository){
this.repository=repository;
}

//other getEmployee(), saveEmployee(..) etc.
}
```
and this employeeService class actually depedends on `EmployeeRepository` class, since this `EmployeeRepository` class calls `DB and external services` to get data so in our Unit Test cases we dont want to call actual Repository instead we want to mock it and inject it in `Employee Service class.

To achieve this, you can do something like this:

```

public MyTest{
  
  @Mock
  private EmployeeRepository employeeRepository;
  
  @InjectMock
  EmployeeService employeeService;
  
  //... other stuff

}

```

Actually what Mockito will do is, it will encounter @InjectMock annotation, so now it will start looking into `EmployeeService class` to check wheather this `EmployeeService class` has any field dependency whose `type` is matching with any of the the mocked Class defined in this Test class, if yes it will inject that else it will gnore that.

so in this case, when Mockito encounters `@InjectMock` anotation on `EmployeeService class` it inspects it and founds that `EmployeeService class` does have depedency and thats the `EmployeeRepository class` and since Mockito already knows that `EmployeeRepository` is mocked so it injects that depedency there.

## Mockito Verify Cookbook

This cookbook illustrates how to use Mockito verify in a variety of usecases.
### verify simple invocation on mock
```
List<String> mockedList = mock(MyList.class);
mockedList.size();
verify(mockedList).size();
```

### verify number of interactions with mock

```
List<String> mockedList = mock(MyList.class);
mockedList.size();
verify(mockedList, times(1)).size();
```

### verify no interaction with the whole mock occurred

```
List<String> mockedList = mock(MyList.class);
verifyZeroInteractions(mockedList);
```
### verify no interaction with a specific method occurred
```
List<String> mockedList = mock(MyList.class);
verify(mockedList, times(0)).size();
```
### verify order of interactions
```
List<String> mockedList = mock(MyList.class);
mockedList.size();
mockedList.add("a parameter");
mockedList.clear();
 
InOrder inOrder = Mockito.inOrder(mockedList);
inOrder.verify(mockedList).size();
inOrder.verify(mockedList).add("a parameter");
inOrder.verify(mockedList).clear();
```

### verify an interaction has not occurred
```
List<String> mockedList = mock(MyList.class);
mockedList.size();
verify(mockedList, never()).clear();
```

### verify an interaction has occurred at least certain number of times
```
List<String> mockedList = mock(MyList.class);
mockedList.clear();
mockedList.clear();
mockedList.clear();
 
verify(mockedList, atLeast(1)).clear();
verify(mockedList, atMost(10)).clear();
```

### verify interaction with exact argument

```
List<String> mockedList = mock(MyList.class);
mockedList.add("test");
verify(mockedList).add("test");
```

### verify interaction with flexible/any argument
```
List<String> mockedList = mock(MyList.class);
mockedList.add("test");
verify(mockedList).add(anyString());
```

### verify interaction using argument capture
```
List<String> mockedList = mock(MyList.class);
mockedList.addAll(Lists.<String> newArrayList("someElement"));
ArgumentCaptor<List> argumentCaptor = ArgumentCaptor.forClass(List.class);
verify(mockedList).addAll(argumentCaptor.capture());
List<String> capturedArgument = argumentCaptor.<List<String>> getValue();
assertThat(capturedArgument, hasItem("someElement"));
```
## Mockito.mock() vs @Mock vs @MockBean

In this quick tutorial, we'll look at three different ways of creating mock objects and how they differ from each other – with Mockito and with the Spring mocking support.

### Mockito.mock()
The Mockito.mock() method allows us to create a mock object of a class or an interface.

Then, we can use the mock to stub return values for its methods and verify if they were called.
This method doesn't need anything else to be done before it can be used. We can use it to create mock class fields as well as local mocks in a method.

### Mockito's @Mock Annotation

This annotation is a shorthand for the Mockito.mock() method. As well, we should only use it in a test class. Unlike the mock() method, we need to enable Mockito annotations to use this annotation.

We can do this either by using the MockitoJUnitRunner to run the test or calling the MockitoAnnotations.initMocks() method explicitly.
Apart from making the code more readable, @Mock makes it easier to find the problem mock in case of a failure, as the name of the field appears in the failure message:

```
Wanted but not invoked:
mockRepository.count();
-> at org.baeldung.MockAnnotationTest.testMockAnnotation(MockAnnotationTest.java:22)
Actually, there were zero interactions with this mock.
 
  at org.baeldung.MockAnnotationTest.testMockAnnotation(MockAnnotationTest.java:22)
  ```
  
  Also, when used in conjunction with @InjectMocks, it can reduce the amount of setup code significantly.
  
 ### Spring Boot's @MockBean Annotation

We can use the @MockBean to add mock objects to the Spring application context. The mock will replace any existing bean of the same type in the application context.

If no bean of the same type is defined, a new one will be added. This annotation is useful in integration tests where a particular bean – for example, an external service – needs to be mocked.

To use this annotation, we have to use SpringRunner to run the test:

```
@RunWith(SpringRunner.class)
public class MockBeanAnnotationIntegrationTest {
     
    @MockBean
    UserRepository mockRepository;
     
    @Autowired
    ApplicationContext context;
     
    @Test
    public void givenCountMethodMocked_WhenCountInvoked_ThenMockValueReturned() {
        Mockito.when(mockRepository.count()).thenReturn(123L);
 
        UserRepository userRepoFromContext = context.getBean(UserRepository.class);
        long userCount = userRepoFromContext.count();
 
        Assert.assertEquals(123L, userCount);
        Mockito.verify(mockRepository).count();
    }
}
```
When we use the annotation on a field, as well as being registered in the application context, the mock will also be injected into the field.

This is evident in the code above. Here, we have used the injected UserRepository mock to stub the count method. We have then used the bean from the application context to verify that it is indeed the mocked bean.



## Injecting Mockito Mocks into Spring Beans
In this article we'll show how to use dependency injection to insert Mockito mocks into Spring Beans for unit testing.

In real-world applications, where components often depend on accessing external systems, it's important to provide proper test isolation so that we can focus on testing the functionality of a given unit without having to involve the whole class hierarchy for each test.

Injecting a mock is a clean way to introduce such isolation.

First, let's create a simple service that we'll be testing:

```
@Service
public class NameService {
    public String getUserName(String id) {
        return "Real user name";
    }
}

//And inject it into the UserService class:

@Service
public class UserService {
 
    private NameService nameService;
 
    @Autowired
    public UserService(NameService nameService) {
        this.nameService = nameService;
    }
 
    public String getUserName(String id) {
        return nameService.getUserName(id);
    }
}
```

For this tutorial, the given classes return a single name regardless of the id provided. This is done so that we don't get distracted by testing any complex logic.

We'll also need a standard Spring Boot main class to scan the beans and initialize the application:

```
@SpringBootApplication
public class MocksApplication {
    public static void main(String[] args) {
        SpringApplication.run(MocksApplication.class, args);
    }
}
```
Now let's move on to the test logic. First of all, we have to configure application context for the tests:

```
@Profile("test")
@Configuration
public class NameServiceTestConfiguration {
    @Bean
    @Primary
    public NameService nameService() {
        return Mockito.mock(NameService.class);
    }
}

```

The @Profile annotation tells Spring to apply this configuration only when the “test” profile is active. The @Primary annotation is there to make sure this instance is used instead of a real one for autowiring. The method itself creates and returns a Mockito mock of our NameService class.

Now we can write the unit test:

```
@ActiveProfiles("test")
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = MocksApplication.class)
public class UserServiceUnitTest {
 
    @Autowired
    private UserService userService;
 
    @Autowired
    private NameService nameService;
 
    @Test
    public void whenUserIdIsProvided_thenRetrievedNameIsCorrect() {
        Mockito.when(nameService.getUserName("SomeId")).thenReturn("Mock user name");
        String testName = userService.getUserName("SomeId");
        Assert.assertEquals("Mock user name", testName);
    }
}
```

We use the @ActiveProfiles annotation to enable the “test” profile and activate the mock configuration we wrote earlier. Because of this, Spring autowires a real instance of the UserService class, but a mock of the NameService class. The test itself is a fairly typical JUnit+Mockito test. We configure the desired behavior of the mock, then call the method which we want to test and assert that it returns the value that we expect.

It's also possible (though not recommended) to avoid using environment profiles in such tests. To do so, remove the @Profile and @ActiveProfiles annotations and add an @ContextConfiguration(classes = NameServiceTestConfiguration.class) annotation to the UserServiceTest class.

