#Groceries数据集

该数据集是从一个杂货店采集，包含现实世界1个月（30天）销售的交易数据。该数据集包含9835笔交易，而交易的商品项分为169类别。

数据类型：

```
> class(Groceries)
[1] "transactions"
attr(,"package")
[1] "arules" #所在包
```

摘要：
```
> Groceries
transactions in sparse（稀疏） format with
 9835 transactions (rows) and
 169 items (columns)

> dim(Groceries)
[1] 9835  169
```

查看：
```
> inspect(Groceries[1:3])
    items                
[1] {citrus fruit,       
     semi-finished bread,
     margarine,          
     ready soups}        
[2] {tropical fruit,     
     yogurt,             
     coffee}             
[3] {whole milk}    
```