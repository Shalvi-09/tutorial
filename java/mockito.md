


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


