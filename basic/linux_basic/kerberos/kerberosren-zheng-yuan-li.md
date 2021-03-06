# Kerberos认证原理

## Kerberos认证原理

Ref:[https://www.jianshu.com/p/7de58a36ba4d](https://www.jianshu.com/p/7de58a36ba4d)

在keberos Authentication中，Kerberos有三个主体：**Client、Server和KDC**。

![](../../../.gitbook/assets/kerberos.webp)

## Key

为了便于理解，这里引入两个关键的概念：

* Master Key：这是一个Long-term Key。在Security的领域中，有的Key可能长期内保持不变，比如你在密码，可能几年都不曾改变，这样的Key、以及由此派生的Key被称为Long-term Key。对于Long-term Key的使用有这样的原则：被Long-term Key加密的数据不应该在网络上传输。原因很简单，一旦这些被Long-term Key加密的数据包被恶意的网络监听者截获，在原则上，只要有充足的时间，他是可以通过计算获得你用于加密的Long-term Key的——任何加密算法都不可能做到绝对保密。
* Session Key：这是一个Short-term Key。由于被Long-term Key加密的数据包不能用于网络传送，所以我们使用另一种Short-term Key来加密需要进行网络传输的数据。由于这种Key只在一段时间内有效，即使被加密的数据包被黑客截获，等他把Key计算出来的时候，这个Key早就已经过期了。

Kerberos里有两种Session Key：

* LogonKey: 这里引入了一个LogonKey用来进行Client向KDC进行TGT申请是进行各种数据传输和身份验证，是一个Short-term Key，相比于直接使用Client Master Key 加密更安全
* SessionKey: Client和 Server交互的Short-term Key

## Kerberos认证的过程

1. Client向KDC（准确地说是Authentication Service，简称AS）发起请求需要一个TGT，TGT是Server无关的，即用一个TGT可以申请多个Server的ticket。发送的请求中包含用Client自己的Master Key加密的一些信息（包括ClientName，timestamp，TGS ServerName）
2. AS收到Client发来的请求，从数据库中拿到Client的Master Key进行解密，验证ClientName和Timestamp，验证通过后给Client发送一个response，该response中包括两部分：**用Client Master Key加密的LogonKey**以及**用KDC master key加密的TGT**，该TGT里面也包含了LogonKey（用于后面解密TGT，因为KDC不保留任何SessionKey），还包含了ClientName和过期时间。
3. Client收到AS发回的response后，用自己的Master Key解密得到LogonKey，还保有一个加密的TGT

以上就是`kinit`做的事情，以后访问各个Service就用这个加密的TGT就可以了。

真正运行访问Service的程序时，进行一下步骤：

1. Client向TGS发送一个请求，其中包括一个加密的TGT、用LogonKey加密的Authenticator（自己的信息）、要访问的Server Name
2. TGS收到Client发来的请求，先用KDC的MasterKey解密TGT，拿到了LogonKey，再用LogonKey解密Authenticator进行Client信息验证，验证通过则向Client发送一个response，其中包含用LogonKey加密的SessionKey，和使用Server的Master Key进行加密的Ticket，这个Ticket里面包含了Session Key 以及 Client的一些信息\(这里称为ClientInfo1\)
3. Client收到TGS发来的response后，用LogonKey解密得到Session Key,

   Client用Session Key 加密自己的Authenticator\(包含ClientInfo2\)，然后连同之前TGS发给他的加密的Ticket一起发给Server

4. Server收到这两组数据包后，先用自己的Master Key解密Ticket，得到了Ticket里的SessionKey和ClientInfo1， 然后再用Session Key解密Authenticator，得到了Authenticator里的ClientInfo2。用ClientInfo1和ClientInfo2比较，如果相同即认证成功。

### 为什么要使用Timestamp？

Client向Server发送的数据包被某个恶意网络监听者截获，该监听者随后将数据包座位自己的Credential冒充该Client对Server进行访问，在这种情况下，依然可以很顺利地获得Server的成功认证。为了解决这个问题，Client在Authenticator中会加入一个当前时间的Timestamp。在Server对Authenticator中的Client Info和Session Ticket中的Client Info进行比较之前，会先提取Authenticator中的Timestamp，并同当前的时间进行比较，如果他们之间的偏差超出一个可以接受的时间范围（一般是5mins），Server会直接拒绝该Client的请求。

### 双向认证（Mutual Authentication）

Kerberos一个重要的优势在于它能够提供双向认证：不但Server可以对Client 进行认证，Client也能对Server进行认证。具体过程是这样的，如果Client需要对他访问的Server进行认证，会在它向Server发送的Credential中设置一个是否需要认证的Flag。Server在对Client认证成功之后，会把Authenticator中的Timestamp提出出来，通过Session Key进行加密，当Client接收到并使用Session Key进行解密之后，如果确认Timestamp和原来的完全一致，那么他可以认定Server正式他试图访问的Server。 那么为什么Server不直接把通过Session Key进行加密的Authenticator原样发送给Client，而要把Timestamp提取出来加密发送给Client呢？原因在于防止恶意的监听者通过获取的Client发送的Authenticator冒充Server获得Client的认证。

