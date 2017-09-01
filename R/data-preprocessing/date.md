# Date

## 概述

日期通常以**字符串**的形式输入到R中，然后转化为数值形式存储的日期变量，函数as.Date()用于执行这种转化，其语法为as.Date(x, "input_format"),其中x是字符型数据，input_format则是读入日期的适当格式。

* %d 数字表示的日期（0~31） 例如01~31
* %a 缩写的星期名 例如Mon
* %A 非缩写的星期名 例如Monday
* %m 月份（00~12） 例如00~12
* %b 缩写的月份 例如Jan
* %B 非缩写的月份 例如January
* %y 两位数的年份 例如07
* %Y 四位数的年份  例如2007

默认的日期格式为yyyy-mm-dd，语句为：
```r
> mydates <- as.Date(c("2007-06-22","2004-02-13"))
> mydates

[1] "2007-06-22" "2004-02-13"
```
 
## 格式转化

```r
> strDates <- c("01/05/1965", "08/16/1975")
> dates <- as.Date(strDates, "%m/%d/%Y") #输入的日期转化为对应日期，其中%m对应为月份，%d对应为日期，%Y对应为年
> dates
[1] "1965-01-05" "1975-08-16"
> str(dates)
 Date[1:2], format: "1965-01-05" "1975-08-16"
```

## 案例

在leadership数据集中，日期是以mm/dd/yy的格式来表示的，现在将其转化为默认的yyyy-mm-dd格式，代码为：

```r
> myformat <- "%m/%d/%y"
> leadership$testDate <- as.Date(leadership$testDate, myformat)
> leadership
  managerID   testDate country gender age item1 item2 item3 item4 item5 stringsAsFactor
1         1 2008-10-24      US      M  32     5     4     5     5     5           FALSE
2         2 2008-10-28      US      F  45     3     5     2     5     5           FALSE
3         3 2008-10-01      UK      F  25     3     5     5     5     2           FALSE
4         4 2008-10-12      UK      M  39     3     3     4    NA    NA           FALSE
5         5 2009-05-01      UK      F  NA     2     2     1     2     1           FALSE
  agecat
1  Young
2  Young
3  Young
4  Young
5   <NA>
```

## 当前日期

```r
> Sys.Date() # 给出当前日期
[1] "2014-01-18"
> date() #返回当前的日期和时间
[1] "Sat Jan 18 14:22:07 2014"
> today<-Sys.Date()
> format(today,format = "%B %d %Y") # 格式化当前日期
[1] "一月 18 2014"
> format(today,fomat ="%A")
[1] "2014-01-18"
```

## 两个日期的相减：
```r
> startdate <- as.Date("2004-02-12")
> enddate <- as.Date("2011-01-22")
> days <-enddate - startdate;days # 两个时间点之差
Time difference of 2536 days
```

## 用difftime()来计算时间间隔，并以星期，天，时，分，秒来表示。
```r
> today <- Sys.Date()
> dob <- as.Date("1956-10-12")
> difftime(today, dob, units="weeks")
Time difference of 2988.143 weeks
```

## convert
 as.character(dates)用于将日期转化为字符型变量，其它与日期有关的函数可以查看help(as.Date),help(strftime)，与日期有关的包为lubridate，复杂日期计算有关
的包为fCalendar。

Ref:http://blog.csdn.net/learneraiqi/article/details/46043311