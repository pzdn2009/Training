# 四色原型

Ref：[http://www.cnblogs.com/happyframework/archive/2013/04/26/3043515.html](http://www.cnblogs.com/happyframework/archive/2013/04/26/3043515.html)

什么人Role在什么地点PPT干了什么事MI。

某个人（Party）的角色（PartyRole）在某个地点（Place）的角色（PlaceRole）用某个东西（Thing）的角色（ThingRole）做了某件事情（Moment-Interval）。

* Role：yellow

  a role that a party plays

* Party ,Place,or Thing:green
* Moment-Interval：red

  时刻-时间段：某个时刻或时间段发生了某个活动。

* Description：blue

示例：

* **PartPlaceThing**：简称PPT，用淡绿色表示，常见的PPT有：部门、岗位、人员、地点、物品等。
* **Description**：简称Des，用淡蓝色表示，主要用来对PPT进行描述，常见的Des有：部门类型、岗位层级、人员类型、地点区域、物品分类等。
* **Role**：用淡黄色表示，主要表示PPT在某个场景下扮演的角色，常见的角色有：财务类部门、管理类岗位、请假者、销售点、产品等。
* **MomentInterval**：简称MI，用淡红色表示，主要表示在一刻或一段时间内发生的一件事情，常见的MI有：部门移动、岗位移动、员工离职、产品销售等。
* **MomentInteval**：简称MIDetail，用淡红色表示，主要表示MI的明细，常见的MIDetail有销售明细、入库明细、出库明细等。

