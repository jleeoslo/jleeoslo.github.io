---
layout: single
title: "로지스틱 회귀분석 (logistic regression analysis)"
description: "Basic understanding  of logistic regression analysis"
headline: "Basic understanding  of logistic regression analysis"
typora-copy-images-to: ../images/2021-03-31
---




## 로지스틱 회귀분석이란?

- 독립변수가 연속형 변수이지만, 종속변수가 비연속형 변수(특히, **이분형 변수** - 어떠한 사건이 발생하거나 발생하지 않을 (0,1)확률만 가짐)일 경우 로지스틱 회귀분석을 사용한다. 

<center><img src="/images/2021-03-31/4.png"></center>

<center><small>출처: 패스트캠퍼스</small></center>

- 종속변수가 이분형 변수임에 따라, 회귀식은 직선으로 표현되지 않고 로그함수와 같은 모습을 취한다.
- 로지스틱 회귀분석은, 회귀분석과 비슷한 측면이 있지만 그 원리가 다름에 따라, 회귀계수를 검증하는 방식이 다르고 회귀분석에 대한 해석(ex. 가능성으로 표현함)이 다르다.



## 로지스틱 회귀분석의 원리

- 특정 사건이 발생할 확률과 발생하지 않을 확률 간의 비율인 Odd ratio =  p / (1-P)로 표현되는데, 여기에 자연로그를 취한 값을 기존 회귀식의 종속변수(y)로 대체한다.
  - 즉, 기존 회귀식:  y = b<sub>0</sub> + b<sub>1</sub> &middot; x + e
          로지스틱 회귀식:  ln<sub>p / (1-P)</sub>> = b<sub>0</sub> + b<sub>1</sub> &middot; x + e
- 종속변수에 로짓이 적용됨에 따라 기울기에 대한 해석도 기존의 회귀분석과는 다르다. b<sub>1</sub> &gt;
  0일 경우, 가 증가할 수록 특정 사건이 발생하지 않을 확률보다 발생할 확률이 높다는 의미이다. 반대로  b<sub>1</sub> &lt;0 일 경우, 가 증가할 수록 특정 사건이 발생할 확률보다 발생하지 않을 확률이 높다는 의미이다.



## 로지스틱 회귀분석 대표 가설

- H0: 독립변수가 종속변수에 미치는 영향의 크기는 ‘0’이 아니다.
- H1(양측검증): 독립변수가 종속변수에 미치는 영향의 크기는 ‘0’이다.
- H1(단측검증): 독립변수가 종속변수에 미치는 영향의 크기는 ‘0’보다 크다.



## 모형적합도

- 모형적합도란 모형이 적절하게 만들어졌는지를 보여주는 지표이다.
- 모형에 대한 검정으로 대표적으로는 카이제곱(x<sup>2</sup>)을, 모형 설명력을 위해서는 -2log likelihood, Cox and Snee R<sup>2</sup> , NagelkerkeR<sup>2</sup> 을 활용한다.

**More to read**
- Logistic Regression Analysis 로지스틱 회귀분석 (https://m.blog.naver.com/libido1014/120122772781)
- [SPSS 26] 로지스틱 회귀분석(Logistic Regression) – 동시입력(https://m.blog.naver.com/y4769/221851780608) 

**Reference** 

- 패스트 캠퍼스 데이터 분석 입문 올인원 패키지 강의
