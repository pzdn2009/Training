# aggregate

聚合函數

公式，數據集，應用的函數。類似GroupBY。

```text
> aggregate(Survived ~  Sex, data=train, FUN=function(x) {sum(x)/length(x)})
     Sex  Survived
1 female 0.7420382
2   male 0.1889081
```

