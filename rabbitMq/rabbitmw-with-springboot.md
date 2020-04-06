
[https://www.devglan.com/spring-boot/springboot-rabbitmq-example](https://www.devglan.com/spring-boot/springboot-rabbitmq-example)

# Spring Boot RabbitMQ Example

RabbitMQ is a widely used AMQP broker. In this article, we will learn how to integrate RabbitMQ with Spring Boot and develop a message producer and consumer example app with RabbitMQ and spring boot.  We will be building a simple notification system and we will be testing the app with CommandLineRunner. 

The producer will publish the message `to the direct exchange` with `routing key` and the consumer `consumes` this message `asynchronously`.

`RabbitMQ` is an `AMQP(Advanced Message Queuing Protocol) broker` and is different from JMS(Java Messaging Service). You can visit my previous articles for [spring boot JMS integration here](https://www.devglan.com/spring-boot/spring-boot-jms-activemq-example).


## Installing RabbitMQ on Windows

Let us first start with RabbitMQ installation on our local system. Below are the steps.

* First download and install Erlang depending upon Windows-32 or Windows-64 bit of your OS from the url https://www.erlang.org/downloads. The erlang version that I have is OTP 22.0

* Next, download the windows installer from https://www.rabbitmq.com/install-windows.html and follow the window installment instruction.

* Once the installation process is done, you can find the installation directory here C:\Program Files\RabbitMQ Server.

* To enable the management console on Windows, you can traverse to the sbin directory and execute below command:

* C:\Program Files\RabbitMQ Server\rabbitmq_server-3.7.17\sbin>rabbitmq-plugins.bat enable rabbitmq_management

![](https://i.imgur.com/gwiI2km.png)

**After this the RabbitMQ management console can be accessed at http://localhost:15672/ and the default username/password would be guest/guest.**

```
rabbitmqctl.bat stop
rabbitmqctl.bat status
rabbitmq-service.bat start
spring-boot-rabbitmq-example
```
## RabbitMQ Essentials

RabbitMQ is a message broker that implements Advanced Message Queing Protocol(AMQP). It increases loose coupling and scalability.

`In any messaging system`, there are `3 components` involved - `Producer`, `Consumer`, and `Queue or Topic`. The producer produces the messages in the Queue and the consumer consumes that message asynchronously from the queue or topic. But `it is` a little `different in case of AMQP`.


For a single exchange and queue, the process is very simple. The producer `publishes` a message `to the exchange` and the `exchange sends the message to the queue` and the `consumer consumes the message from the queue`.

__With a complex system,__ we will have multiple queues and multiple consumers. In that case, `the producer` `sends message` `to the exchange with a routing key` and the `exchange connects with the Queue only with binding key` and then the messages are distributed to all the queues.   

There are different types of exchange.


* Direct Exchange - It routes messages to a queue by matching routing key equal to binding key.

* Fanout Exchange - It ignores the routing key and sends message to all the available queues.

* Topic Exchange –  It routes messages to multiple queues by a partial matching of a routing key. It uses patterns to match the routing and binding key.

* Headers Exchange – It uses message header instead of routing key.

* Default(Nameless) Exchange - It routes the message to queue name that exactly matches with the routing key.

## Spring Boot RabbitMQ Project Setup

Head over to https://start.spring.io to download the sample spring boot project with spring-boot-starter-amqp artifact.
























