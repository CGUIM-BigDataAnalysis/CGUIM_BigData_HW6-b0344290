前10大死因與年齡層之關係
================

分析議題背景
------------

B0344240陳衍儒 B0344250歐宜宣 因生活習慣改變，許多印象中只有老年人才會得到的疾病，竟漸漸出現在年輕人身上。

分析動機
--------

希望藉由報告研究各年齡層與疾病死因的關聯性。 \#\# 使用資料 我們使用的是100年到104年之全國死因資料 資料內容包括年度、地區、死因、年齡、性別等。(X100\_104) 另外一個資料表則是地區代碼對應之地區(x1)

載入使用資料們

``` r
#這是R Code Chunk
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 3.3.3

``` r
X100_104 <- read_excel("C:/Users/Ariel/Desktop/CGUIM_BigData_HW6-b0344290-master/100-104.xlsx")
 X1 <- read_excel("C:/Users/Ariel/Desktop/CGUIM_BigData_HW6-b0344290-master/1.xlsx")
```

資料處理與清洗
--------------

1.我們先將資料跟地區代碼表結合 2.接著我們將104年的死因資料取出放入data104中 3.將data104以性別分開 4.接著分別計算男/女性死因排名 5.合併兩個表成causeN 6.將寬表轉成長表

處理資料

``` r
#這是R Code Chunk
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
data <- inner_join(X100_104,X1,by="100county")
data104 <- data[grepl("104",data$year),]
data104_m <-  data104[grepl("男",data104$sex),]
data104_f <- data104[grepl("女",data104$sex),]

causeN_m <- group_by(data104_m,cause) %>%
 summarise(causeN_m = sum(N))%>%
  arrange(desc(causeN_m))

causeN_f <- group_by(data104_f,cause) %>%
  summarise(causeN_f = sum(N))%>%
  arrange(desc(causeN_f))

causeN <- merge(causeN_m,causeN_f,by="cause",all=TRUE)

library(reshape2)
```

    ## Warning: package 'reshape2' was built under R version 3.3.3

``` r
data2 <- melt(causeN,id.vars = c("cause"))
```

探索式資料分析
--------------

``` r
#這是R Code Chunk
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 3.3.3

``` r
ggplot(data2,
       aes(x= cause,y= value,
           color=variable))+
  geom_point()
```

    ## Warning: Removed 1 rows containing missing values (geom_point).

![](README_files/figure-markdown_github/unnamed-chunk-3-1.png)

期末專題分析規劃
----------------

期末專題要做100~104年死因的排名比較，並增加年齡層之分析
