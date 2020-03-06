# 时间序列

## 時間序列

## 1. Getting Started

### 1.1 Nile

```r
> print(Nile)
Time Series:
Start = 1871 
End = 1970 
Frequency = 1 
  [1] 1120 1160  963 1210 1160 1160  813 1230 1370 1140  995  935 1110  994 1020
 [16]  960 1180  799  958 1140 1100 1210 1150 1250 1260 1220 1030 1100  774  840
 [31]  874  694  940  833  701  916  692 1020 1050  969  831  726  456  824  702
 [46] 1120 1100  832  764  821  768  845  864  862  698  845  744  796 1040  759
 [61]  781  865  845  944  984  897  822 1010  771  676  649  846  812  742  801
 [76] 1040  860  874  848  890  744  749  838 1050  918  986  797  923  975  815
 [91] 1020  906  901 1170  912  746  919  718  714  740

> length(Nile)
[1] 100

> head(Nile,n=10)
 [1] 1120 1160  963 1210 1160 1160  813 1230 1370 1140

> tail(Nile,n=12)
 [1]  975  815 1020  906  901 1170  912  746  919  718  714  740

# Plot the Nile data
plot(Nile)

# Plot the Nile data with xlab and ylab arguments
plot(Nile, xlab = "Year", ylab = "River Volume (1e9 m^{3})")

# Plot the Nile data with xlab, ylab, main, and type arguments
plot(Nile, main="Annual River Nile Volume at Aswan, 1871-1970",type="b")
```

![](../../../.gitbook/assets/rnile001%20%281%29.png)

### 1.2 均勻Time Index

```r
# Plot the continuous_series using continuous time indexing
> par(mfrow=c(2,1))
> plot(continuous_time_index,continuous_series, type = "b")

# Make a discrete time index using 1:20 
> discrete_time_index <- 1:20

# Now plot the continuous_series using discrete time indexing
> plot(discrete_time_index,continuous_series, type = "b")

> continuous_time_index
 [1]  1.210322  1.746137  2.889634  3.591384  5.462065  5.510933  7.074295
 [8]  8.264398  9.373382  9.541063 11.161122 12.378371 13.390559 14.066280
[15] 15.093547 15.864515 16.857413 18.091457 19.365451 20.180524
> discrete_time_index
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
> continuous_series
 [1]  0.56889468  0.76630408  0.99207512  0.97481741  0.39912320  0.37660246
 [7] -0.38532033 -0.83635852 -0.99966983 -0.99831019 -0.64622280 -0.09386151
[13]  0.40052909  0.68160578  0.95318159  0.99693803  0.83934194  0.37003754
[19] -0.25509676 -0.61743983
```

![](../../../.gitbook/assets/rtimeindex002%20%281%29.png)

### 1.3 函數

* start
* end
* frequcency
* deltat

```r
> str(AirPassengers)
 Time-Series [1:144] from 1949 to 1961: 112 118 132 129 121 135 148 148 136 119 ...

> plot(AirPassengers)

> start(AirPassengers)
[1] 1949    1

> end(AirPassengers)
[1] 1960   12

> time(AirPassengers)
          Jan      Feb      Mar      Apr      May      Jun      Jul      Aug
1949 1949.000 1949.083 1949.167 1949.250 1949.333 1949.417 1949.500 1949.583
1950 1950.000 1950.083 1950.167 1950.250 1950.333 1950.417 1950.500 1950.583
1951 1951.000 1951.083 1951.167 1951.250 1951.333 1951.417 1951.500 1951.583
1952 1952.000 1952.083 1952.167 1952.250 1952.333 1952.417 1952.500 1952.583
1953 1953.000 1953.083 1953.167 1953.250 1953.333 1953.417 1953.500 1953.583
1954 1954.000 1954.083 1954.167 1954.250 1954.333 1954.417 1954.500 1954.583
1955 1955.000 1955.083 1955.167 1955.250 1955.333 1955.417 1955.500 1955.583
1956 1956.000 1956.083 1956.167 1956.250 1956.333 1956.417 1956.500 1956.583
1957 1957.000 1957.083 1957.167 1957.250 1957.333 1957.417 1957.500 1957.583
1958 1958.000 1958.083 1958.167 1958.250 1958.333 1958.417 1958.500 1958.583
1959 1959.000 1959.083 1959.167 1959.250 1959.333 1959.417 1959.500 1959.583
1960 1960.000 1960.083 1960.167 1960.250 1960.333 1960.417 1960.500 1960.583
          Sep      Oct      Nov      Dec
1949 1949.667 1949.750 1949.833 1949.917
1950 1950.667 1950.750 1950.833 1950.917
1951 1951.667 1951.750 1951.833 1951.917
1952 1952.667 1952.750 1952.833 1952.917
1953 1953.667 1953.750 1953.833 1953.917
1954 1954.667 1954.750 1954.833 1954.917
1955 1955.667 1955.750 1955.833 1955.917
1956 1956.667 1956.750 1956.833 1956.917
1957 1957.667 1957.750 1957.833 1957.917
1958 1958.667 1958.750 1958.833 1958.917
1959 1959.667 1959.750 1959.833 1959.917
1960 1960.667 1960.750 1960.833 1960.917

> deltat(AirPassengers)
[1] 0.08333333

> frequency(AirPassengers)
[1] 12

> cycle(AirPassengers)
     Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
1949   1   2   3   4   5   6   7   8   9  10  11  12
1950   1   2   3   4   5   6   7   8   9  10  11  12
1951   1   2   3   4   5   6   7   8   9  10  11  12
1952   1   2   3   4   5   6   7   8   9  10  11  12
1953   1   2   3   4   5   6   7   8   9  10  11  12
1954   1   2   3   4   5   6   7   8   9  10  11  12
1955   1   2   3   4   5   6   7   8   9  10  11  12
1956   1   2   3   4   5   6   7   8   9  10  11  12
1957   1   2   3   4   5   6   7   8   9  10  11  12
1958   1   2   3   4   5   6   7   8   9  10  11  12
1959   1   2   3   4   5   6   7   8   9  10  11  12
1960   1   2   3   4   5   6   7   8   9  10  11  12
```

![](../../../.gitbook/assets/rairpassengers003%20%281%29.png)

### 1.4 綜合示例

```r
# Check whether Nile is a ts object
is.ts(Nile)

# Check whether AirPassengers is a ts object
is.ts(AirPassengers)


# Check whether eu_stocks is a ts object
is.ts(EuStockMarkets)

# View the start, end, and frequency of eu_stocks
start(EuStockMarkets)
end(EuStockMarkets)
frequency(EuStockMarkets)

# Generate a simple plot of eu_stocks
plot(EuStockMarkets)

# Use ts.plot with eu_stocks
ts.plot(EuStockMarkets, col = 1:4, xlab = "Year", ylab = "Index Value", main = "Major European Stock Indices, 1991-1998")

# Add a legend to your ts.plot
legend("topleft", colnames(EuStockMarkets), lty = 1, col = 1:4, bty = "n")
```

![](../../../.gitbook/assets/reustock004.png)

![](../../../.gitbook/assets/reustock005.png)

