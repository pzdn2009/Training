# 可视化

所在包：arulesViz包

```text
> r5<-apriori(Groceries,parameter=list(support=0.002,confidence=0.5))
> plot(r5) #默认support为横轴，confidence为纵轴，lift为shading。

> plot(rules5,measure=c("support","lift"),shading="confidence") #support为横轴，lift为纵轴，confidence为条线线。

> plot(rules5,interactive=T) #交互式，可以使用filter过滤，单击两次选择一个区域查看详细规则内容。

> plot(rules5,method="grouped") #分组图
> plot(rules5,method="matrix3D",measure=”lift”) #3D图
```

