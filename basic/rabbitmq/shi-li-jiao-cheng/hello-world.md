# Hello World

## Hello world

RabbitMQ is a message broker. it accepts and forwards messages.

* 生產者P
* 隊列Q
* 消費者C

![](../../../.gitbook/assets/rmq2.png)

## 1. Send

```csharp
using System;
using RabbitMQ.Client;
using System.Text;

class Send
{
    public static void Main()
    {
        var factory = new ConnectionFactory() { HostName = "localhost" }; //1
        using(var connection = factory.CreateConnection()) //2
        using(var channel = connection.CreateModel()) //3
        {
            channel.QueueDeclare(queue: "hello",
                                 durable: false,
                                 exclusive: false,
                                 autoDelete: false,
                                 arguments: null); //4

            string message = "Hello World!";
            var body = Encoding.UTF8.GetBytes(message);

            channel.BasicPublish(exchange: "",
                                 routingKey: "hello",
                                 basicProperties: null,
                                 body: body); //5
            Console.WriteLine(" [x] Sent {0}", message);
        }

        Console.WriteLine(" Press [enter] to exit.");
        Console.ReadLine();
    }
}
```

1. 創建連接工廠，使用本地主機，默認的用戶名和密碼guest等；
2. 創建一個Connection，連接到Rq；
3. 創建一個Channel；
4. 申明隊列，名稱、持久化、排他隊列、自動刪除消息、其他參數；
5. 寫消息到隊列

## 2. Receive

```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System;
using System.Text;

class Receive
{
    public static void Main()
    {
        var factory = new ConnectionFactory() { HostName = "localhost" };  //1
        using(var connection = factory.CreateConnection()) //2
        using(var channel = connection.CreateModel()) //3
        {
            channel.QueueDeclare(queue: "hello",
                                 durable: false,
                                 exclusive: false,
                                 autoDelete: false,
                                 arguments: null); //4

            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) =>
            {
                var body = ea.Body;
                var message = Encoding.UTF8.GetString(body);
                Console.WriteLine(" [x] Received {0}", message);
            };  //5
            channel.BasicConsume(queue: "hello",
                                 noAck: true,
                                 consumer: consumer); //6

            Console.WriteLine(" Press [enter] to exit.");
            Console.ReadLine();
        }
    }
}
```

1. 創建連接工廠，使用本地主機，默認的用戶名和密碼guest等；
2. 創建一個Connection，連接到Rq；
3. 創建一個Channel；
4. 申明隊列，名稱、持久化、排他隊列、自動刪除消息、其他參數；
5. 創建消費者，且註冊消費事件；
6. 消費

Ref:[http://blog.csdn.net/lmj623565791/article/details/37607165](http://blog.csdn.net/lmj623565791/article/details/37607165)

