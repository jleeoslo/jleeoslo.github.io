---
layout: single
title: "[Chi-squared test] Testing the independence of two categorical variables with R script"
description: "[Chi-squared test] Testing the independence of two categorical variables with R script"
headline: "[Chi-squared test] Testing the independence of two categorical variables with R script"
typora-copy-images-to: ../images/2021-04-08
---



## Testing the independence of categorical data

### (1) Opening the file

      raw = read.csv('data/raw.csv')
      head(raw)
    
    ##   PROD   AGE
    ## 1    B AGE20
    ## 2    C AGE30
    ## 3    B AGE30
    ## 4    A AGE20
    ## 5    B AGE20
    ## 6    C AGE30
    
      nrow(raw)
    
    ## [1] 100

There are two categorical variables: products(A,B and C) and age(20 and 30)



### (2) Making a cross tabulation and drawing a heatmap

      table_raw = table(raw$PROD, raw$AGE)
      table_raw
    
    ##    
    ##     AGE20 AGE30
    ##   A    30     0
    ##   B    20    20
    ##   C     0    30

<br>

      heatmap(table_raw,
              Rowv=NA, Colv=NA, scale='none', col=colorRampPalette(c('ivory','red'))(100), revC = TRUE)



<center><img src ="/images/2021-04-08/heatmap-1.png"></center>

The heatmap shows that people in 20 tend to buy A product  while the people in 30 are likely to buy product. Then, let's do chi-squared test to figure out the two categorical variables are not independent.



### (3) Using chisq.test(  ), testing the independence of two variables

      chisq.test(table_raw)
    
    ## 
    ##  Pearson's Chi-squared test
    ## 
    ## data:  table_raw
    ## X-squared = 60, df = 2, p-value = 9.358e-14

As the P-value is very small(9.358e-14), the null hypothesis stating that the two variable(product and age) are independent is rejected. Thus, we can say product and age are **not** independent but related.



**Reference**

- 패스트 캠퍼스 데이터 분석 입문 올인원 패키지 강의
