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

## Exchange

```csharp
void ExchangeBind(string destination, string source, string routingKey, IDictionary<string, object> arguments);
void ExchangeBind(string destination, string source, string routingKey);
void ExchangeBindNoWait(string destination, string source, string routingKey, IDictionary<string, object> arguments);
//Bind an exchange to an exchange.

void ExchangeDeclare(string exchange, string type, bool durable, bool autoDelete, IDictionary<string, object> arguments);
void ExchangeDeclare(string exchange, string type, bool durable);
void ExchangeDeclare(string exchange, string type);
void ExchangeDeclareNoWait(string exchange, string type, bool durable, bool autoDelete, IDictionary<string, object> arguments);
void ExchangeDeclarePassive(string exchange);
//Declare an exchange

void ExchangeDelete(string exchange, bool ifUnused);
void ExchangeDelete(string exchange);
void ExchangeDeleteNoWait(string exchange, bool ifUnused);
//Delete an exchange.

void ExchangeUnbind(string destination, string source, string routingKey, IDictionary<string, object> arguments);
void ExchangeUnbind(string destination, string source, string routingKey);
void ExchangeUnbindNoWait(string destination, string source, string routingKey, IDictionary<string, object> arguments);
//Unbind an exchange from an exchange.
```
## Transaction

```csharp
void TxCommit();
void TxRollback();
void TxSelect();
```

