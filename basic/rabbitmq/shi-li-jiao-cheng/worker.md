# Worker

## Worker

## 1. Sender

```csharp
using RabbitMQ.Client;
using System;
using System.Text;

namespace RmqSolutions
{
    public class NewTask
    {
        public static void Main(string[] args)
        {
            var factory = new ConnectionFactory() { Host = "localhost" };

            using (var connection = factory.CreateConnection())
            using (var channel = connection.CreateModel())
            {
                channel.QueueDeclare(queue: "task_queue",
                                     durable: true,
                                     exclusive: false,
                                     autoDelete: false,
                                     arguments: null); //[2]

                var message = GetMessage(args);
                var body = Encoding.UTF8.GetBytes(message);

                var properties = channel.CreateBasicProperties();
                properties.Persistent = true;  //[3]

                channel.BasicPublish(exchange: "",
                                     routingKey: "task_queue",
                                     basicProperties: properties,
                                     body: body);
                Console.WriteLine(" [x] Sent {0}", message);
            }

            Console.WriteLine(" Press [enter] to exit.");
            Console.ReadLine();
        }

        private static string GetMessage(string[] args)
        {
            return ((args.Length > 0) ? string.Join(" ", args) : "Hello World!");
        }
    }
}
```

* 消息持久化（message durability）

  首先，聲明隊列是持久化的：\[2\]

  其次，保證消息是持久化的：\[3\]

## 2. Receiver\(worker\)

```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System;
using System.Text;
using System.Threading;

namespace Consumers
{
    public class Worker
    {
        public static void Main()
        {
            var factory = new ConnectionFactory() { Host = "localhost" };

            using (var connection = factory.CreateConnection())
            using (var channel = connection.CreateModel())
            {
                channel.QueueDeclare(queue: "task_queue",
                                     durable: true,
                                     exclusive: false,
                                     autoDelete: false,
                                     arguments: null);  

                channel.BasicQos(prefetchSize: 0, prefetchCount: 1, global: false); //[4]

                Console.WriteLine(" [*] Waiting for messages.");

                var consumer = new EventingBasicConsumer(channel);
                consumer.Received += (model, ea) =>
                {
                    var body = ea.Body;
                    var message = Encoding.UTF8.GetString(body);
                    Console.WriteLine(" [x] Received {0}", message);

                    int dots = message.Split('.').Length - 1;
                    Thread.Sleep(dots * 1000);

                    Console.WriteLine(" [x] Done");

                    channel.BasicAck(deliveryTag: ea.DeliveryTag, multiple: false);  //[1]
                };
                channel.BasicConsume(queue: "task_queue",
                                     noAck: false,
                                     consumer: consumer);

                Console.WriteLine(" Press [enter] to exit.");
                Console.ReadLine();
            }
        }
    }

}
```

* 消息应答（message acknowledgments）

  保證消息永不丟失。如果處理過了，則告訴Rabbit，可以刪除了，如果Rabbit沒有收到應答，那麼就發送給下一個消費者（Worker）。

  An ack\(nowledgement\) is sent back from the consumer to tell RabbitMQ that a particular message has been received, processed and that RabbitMQ is free to delete it.

  noAck："no manual acks"，設置為false，即開啟手動確認。

  關鍵代碼 : \[1\]

* 公平轉發

  传递参数为prefetchCount = 1。这样告诉RabbitMQ不要在同一时间给一个消费者超过一条消息。

  關鍵代碼：\[4\]

