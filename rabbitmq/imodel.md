# IModel

## Basic Methods

```csharp
void BasicAck(ulong deliveryTag, bool multiple);
//確認一個或多個消息。
deliveryTag：BasicDeliverEventArgs. DeliveryTag

void BasicCancel(string consumerTag);

string BasicConsume(string queue, bool noAck, IBasicConsumer consumer);
string BasicConsume(string queue, bool noAck, string consumerTag, IBasicConsumer consumer);
string BasicConsume(string queue, bool noAck, string consumerTag, IDictionary<string, object> arguments, IBasicConsumer consumer);
string BasicConsume(string queue, bool noAck, string consumerTag, bool noLocal, bool exclusive, IDictionary<string, object> arguments, IBasicConsumer consumer);
consumer通常為：EventingBasicConsumer.

BasicGetResult BasicGet(string queue, bool noAck);
獲取一條獨立的消息。如果沒有可用的消息，返回null。

void BasicNack(ulong deliveryTag, bool multiple, bool requeue);
拒絕一條或多條消息。

void BasicPublish(PublicationAddress addr, IBasicProperties basicProperties, byte[] body);
void BasicPublish(string exchange, string routingKey, IBasicProperties basicProperties, byte[] body);
void BasicPublish(string exchange, string routingKey, bool mandatory, IBasicProperties basicProperties, byte[] body);
發送消息

void BasicQos(uint prefetchSize, ushort prefetchCount, bool global);
配置Qos參數

void BasicRecover(bool requeue);
void BasicRecoverAsync(bool requeue);

void BasicReject(ulong deliveryTag, bool requeue);
拒絕一條消息。
```