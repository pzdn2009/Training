# groupBy(x, ...)

用指定的列來分組，然後就可以用聚合了。

參數：
- x: DF
- ...: variable(s) (character names(s) or Column(s)) to group on.

Eg：
```r
# Compute the average for all numeric columns grouped by department.
avg(groupBy(df, "department"))

# Compute the max age and average salary, grouped by department and gender.
agg(groupBy(df, "department", "gender"), salary="avg", "age" -> "max")
```