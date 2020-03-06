# 参数控制

## 参数控制

## 1.通过支持度、置信度共同控制

```text
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.5))
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.6))
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.61))
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.62))
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.63))
r2<-apriori(Groceries,parameter=list(support=0.005,confidence=0.64))
```

## 2. 通过支持度控制

```text
> r1<-apriori(Groceries,parameter=list(support=0.001,confidence=0.5))
> rules.sorted_sup = sort(r1,by="support")
> rules.sorted_con = sort(r1,by="confidence")
> rules.sorted_lift= sort(r1,by="lift")

通过置信度控制
> inspect(rules.sorted_sup[1:5])
    lhs                                      rhs          support    confidence lift    
[1] {other vegetables,yogurt}             => {whole milk} 0.02226741 0.5128806  2.007235
[2] {tropical fruit,yogurt}               => {whole milk} 0.01514997 0.5173611  2.024770
[3] {other vegetables,whipped/sour cream} => {whole milk} 0.01464159 0.5070423  1.984385
[4] {root vegetables,yogurt}              => {whole milk} 0.01453991 0.5629921  2.203354
[5] {pip fruit,other vegetables}          => {whole milk} 0.01352313 0.5175097  2.025351

通过信度控制
> inspect(rules.sorted_con[1:5])
    lhs                                           rhs          support     confidence lift    
[1] {rice,sugar}                               => {whole milk} 0.001220132 1          3.913649
[2] {canned fish,hygiene articles}             => {whole milk} 0.001118454 1          3.913649
[3] {root vegetables,butter,rice}              => {whole milk} 0.001016777 1          3.913649
[4] {root vegetables,whipped/sour cream,flour} => {whole milk} 0.001728521 1          3.913649
[5] {butter,soft cheese,domestic eggs}         => {whole milk} 0.001016777 1          3.913649

通过提升度控制
> inspect(rules.sorted_lift[1:5])
    lhs                                   rhs              support     confidence lift    
[1] {Instant food products,soda}       => {hamburger meat} 0.001220132 0.6315789  18.99565
[2] {soda,popcorn}                     => {salty snack}    0.001220132 0.6315789  16.69779
[3] {flour,baking powder}              => {sugar}          0.001016777 0.5555556  16.40807
[4] {ham,processed cheese}             => {white bread}    0.001931876 0.6333333  15.04549
[5] {whole milk,Instant food products} => {hamburger meat} 0.001525165 0.5000000  15.03823
```

## 3. 其他参数

rhs：

```text
> r4<-apriori(Groceries,parameter=list(maxlen=2,supp=0.001,conf=0.1),appearance=list(rhs="mustard",default="lhs"))
> inspect(r4)
    lhs             rhs       support     confidence lift    
[1] {mayonnaise} => {mustard} 0.001423488 0.1555556  12.96516
```

