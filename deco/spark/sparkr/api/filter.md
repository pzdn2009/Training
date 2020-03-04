# filter

按照給定的條件過濾DF的行。返回新的DF。

參數：

* x:DF
* condition:過濾條件，列表達式或者SQL語句。

Eg：

```r
sparkR.session()
path <- "path/to/file.json"
df <- read.json(path)
filter(df, "col1 > 0")
filter(df, df$col2 != "abcdefg")
```

