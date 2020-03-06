# EventingBasicConsumer

```csharp
public event EventHandler<BasicDeliverEventArgs> Received;
public event EventHandler<ConsumerEventArgs> Registered;
public event EventHandler<ShutdownEventArgs> Shutdown;
public event EventHandler<ConsumerEventArgs> Unregistered;
```

## BasicDeliverEventArgs

* ConsumerTag

  Eg:amq.ctag-Gc…

* DeliveryTag

  Eg:1

* Exchange

  Eg:””

* Redelivered

  Eg:false

* RoutingKey

  Eg:task\_queue

* Body
* BasicProperties 

## ConsumerEventArgs

* ConsumerTag

## ShutdownEventArgs

