# Overview

Ref：http://blog.csdn.net/samxx8/article/details/47417133 

![AMQP](/assets/rmq1.png)

# 1. 概念

**Broker**：简单来说就是消息队列服务器实体。

**Exchange**：消息交换机，它指定消息按什么规则，路由到哪个队列。

**Queue**：消息队列载体，每个消息都会被投入到一个或多个队列。

**Binding**：绑定，它的作用就是把exchange和queue按照路由规则绑定起来。

**Routing Key**：路由关键字，exchange根据这个关键字进行消息投递。

**vhost**：虚拟主机，一个broker里可以开设多个vhost，用作不同用户的权限分离。

**Producer**：消息生产者，就是投递消息的程序。

**Consumer**：消息消费者，就是接受消息的程序。

**Channel**：消息通道，在客户端的每个连接里，可建立多个channel，每个channel代表一个会话任务。

**消息队列的使用过程大概如下：**

1. 客户端连接到消息队列服务器，打开一个channel;
2. 客户端声明一个exchange，并设置相关属性;
3. 客户端声明一个queue，并设置相关属性;
4. 客户端使用routing key，在exchange和queue之间建立好绑定关系;
5. 客户端投递消息到exchange。

# 2. Exchange

在AMQP模型中，Exchange是**接受生产者消息并将消息路由到消息队列的关键组件**。ExchangeType和Binding决定了消息的路由规则。

所以生产者想要发送消息，首先必须要声明一个Exchange和该Exchange对应的Binding。
可以通过 ExchangeDeclare和BindingDeclare完成。

ExchangeDeclare：
>声明一个Exchange需要三个参数：ExchangeName，ExchangeType和Durable。
- ExchangeName是该Exchange的名字，该属性在创建Binding和生产者通过publish推送消息时需要指定。
- ExchangeType，指Exchange的类型，在RabbitMQ中，有三种类型的Exchange：direct ，fanout和topic，不同的Exchange会表现出不同路由行为。
- Durable是该Exchange的持久化属性。

BindingDeclare：
>声明一个Binding需要提供一个QueueName，ExchangeName和BindingKey。

# 3. ExchangeType

生产者在发送消息时，都需要指定一个RoutingKey和Exchange，Exchange在接到该RoutingKey以后，会判断该ExchangeType:
- 如果是**Direct**类型，则会将消息中的RoutingKey与该Exchange关联的所有Binding中的BindingKey进行比较，如果相等，则发送到该Binding对应的Queue中。
- 如果是  **Fanout  **类型，则会将消息发送给所有与该  Exchange  定义过  Binding 的所有  Queues  中去，其实是一种广播行为。
- 如果是**Topic**类型，则会按照正则表达式，对RoutingKey与BindingKey进行匹配，如果匹配成功，则发送到对应的Queue中。

