# Kerberos简介

* 麻省理工大学发明
* 客户端/服务器结构与DES加密技术，并且能够进行相互认证，即客户端和服务器端均可对对方进行身份认证
* 一种应用对称密钥体制进行密钥管理的系统，支持SSO
* 不适应互联网环境
* UNIX 和 Windows 作为默认的安全认证服务集成进操作系统
* 苹果的Mac OS X也使用了Kerberos的客户和服务器版本
* Red Hat Enterprise Linux4 和后续的操作系统使用了Kerberos的客户和服务器版本

Kerberos 是一种网络**认证**协议，其设计目标是通过密钥系统为客户机 / 服务器应用程序提供强大的认证服务。

协议的安全主要依赖于参加者对时间的松散同步和短周期的叫做Kerberos票据的认证声明。

## 相关概念

AS（Authentication Server）= 认证服务器 KDC（Key Distribution Center）= 密钥分发中心 TGT（Ticket Granting Ticket）= 票据授权票据，票据的票据 TGS（Ticket Granting Server）= 票据授权服务器 SS（Service Server）= 特定服务提供端

## 认证流程

* 客户端用户发送自己的用户名到KDC服务器以向AS服务进行认证。
* KDC服务器会生成相应的TGT票据，打上时间戳，在本地数据库中查找该用户的密码，并用该密码对TGT进行加密，将结果发还给客户端用户。
* 该操作仅在用户登录或者kinit申请的时候进行。
* 客户端收到该信息，并使用自己的密码进行解密之后，就能得到TGT票据了。这个TGT会在一段时间之后失效，也有一些程序\(session manager\)能在用户登陆期间进行自动更新。
* 当客户端用户需要使用一些特定服务\(Kerberos术语中用"principal"表示\)的时候，该客户端就发送TGT到KDC服务器中的TGS服务。
* 当该用户的TGT验证通过并且其有权访问所申请的服务时，TGS服务会生成一个该服务所对应的ticket和session key，并发还给客户端。
* 客户端将服务请求与该ticket一并发送给相应的服务端即可。

## 简化流程

* Client向KDC申请TGT（Ticket Granting Ticket）。
* Client通过获得TGT向KDC申请用于访问Server的Ticket。
* Client最终向为了Server对自己的认证向其提交Ticket。

