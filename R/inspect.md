# inspect

Display Associations and Transactions in Readable Form。

示例：
```
> data("Adult")
> rules <- apriori(Adult)
> inspect(rules[1000])
    lhs                         rhs                              support confidence     lift
[1] {education=Some-college,                                                                
     sex=Male,                                                                              
     capital-loss=None}      => {native-country=United-States} 0.1208181  0.9256471 1.031449


> inspect(rules[1000], ruleSep = "---->", itemSep = " + ", setStart = "", setEnd ="", linebreak = FALSE)
    lhs                                                         rhs                          support   confidence lift    
[1] education=Some-college + sex=Male + capital-loss=None ----> native-country=United-States 0.1208181 0.9256471  1.031449
```