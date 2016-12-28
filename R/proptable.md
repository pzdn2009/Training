# prop.table

查看佔比概率Proportion


eg：
```
> prop.table(table(train$Survived))

        0         1 
0.6161616 0.3838384 
```

设置row-wise，column-wise

This is done by setting the second argument of prop.table(), called margin, to 1 or 2, respectively.

```
# Your train and test set are still loaded
str(train)
str(test)

# Survival rates in absolute numbers
table(train$Survived)

# Survival rates in proportions
prop.table(table(train$Survived))
  
# Two-way comparison: Sex and Survived
table(train$Sex,train$Survived)

# Two-way comparison: row-wise proportions
prop.table(table(train$Sex,train$Survived),1)

```

