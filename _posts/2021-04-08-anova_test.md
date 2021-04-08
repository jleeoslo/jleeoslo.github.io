---
layout: single
title: "[ANOVA test] Testing the difference in means between groups"
description: "[ANOVA test] Testing the difference in means between groups"
headline: "[ANOVA test] Testing the difference in means between groups"
typora-copy-images-to: ../images/2021-04-08
---



## Testing the difference in means between groups

### (1) Opening the file

      team_score = read.csv('data/TEAM_SCORE.csv')
      team_score
    
    ##    TEAM SCORE
    ## 1     A     0
    ## 2     A    15
    ## 3     A     5
    ## 4     A    10
    ## 5     A    10
    ## 6     B     8
    ## 7     B    12
    ## 8     B    10
    ## 9     B    10
    ## 10    B    20
    ## 11    C    10
    ## 12    C    11
    ## 13    C     9
    ## 14    C     9
    ## 15    C    11
    
      nrow(team_score)
    
    ## [1] 15

There are three groups having a numerical variable: TEAM(A,B and C) and SCORE(0~20)



### (2) Using aggregate( ), calculating means of each group and drawing their boxplots

      aggregate(SCORE ~ TEAM, data=team_score, mean)
    
    ##   TEAM SCORE
    ## 1    A     8
    ## 2    B    12
    ## 3    C    10

<br>

      boxplot(SCORE ~ TEAM, data=team_score)

<center><img src ="/images/2021-04-08/boxplot-1.png"></center>



### (3) Using aov( ), testing the difference in means between the groups

      result = aov(SCORE ~ TEAM, data=team_score)
      summary(result)
    
    ##             Df Sum Sq Mean Sq F value Pr(>F)
    ## TEAM         2     40    20.0   1.081   0.37
    ## Residuals   12    222    18.5

\#\#\# As F-value is 0.37 here, the null hypothesis stating that there is no difference in means between the groups is rejected. Therefore, the difference in means among A,B and C teams (at least between two groups) is statistically significant.



**Reference**

- 패스트 캠퍼스 데이터 분석 입문 올인원 패키지 강의
