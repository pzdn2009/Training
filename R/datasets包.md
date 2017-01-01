# datasets包

# 1. 内置数据集
```
> data(package='datasets') #加载datasets中所有的数据集
> ?AirPassengers #查看AirPassengers的文档
> data(package= .packages(all.available=T)) # 获取R中的所有数据集列表，按照包分类
Data sets in package ‘boot’:

acme                   Monthly Excess Returns
aids                   Delay in AIDS Reporting in England and Wales
……

Data sets in package ‘cluster’:

agriculture            European Union Agricultural Workforces
animals                Attributes of Animals
chorSub                Subset of C-horizon of Kola Data
flower                 Flower Characteristics
plantTraits            Plant Species Traits Data
pluton                 Isotopic Composition Plutonium Batches
ruspini                Ruspini Data
votes.repub            Votes for Republican Candidate in
                       Presidential Elections
xclara                 Bivariate Data Set with 3 Clusters

Data sets in package ‘datasets’:

AirPassengers          Monthly Airline Passenger Numbers 1949-1960
BJsales                Sales Data with Leading Indicator
BJsales.lead (BJsales)
                       Sales Data with Leading Indicator
……
```

# 2. 外部安装
```
> install.packages("arules")
> library(Matrix)
> library(arules)
> data(Groceries)
> Groceries
transactions in sparse format with
 9835 transactions (rows) and
 169 items (columns)
```