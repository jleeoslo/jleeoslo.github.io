---
layout: single
title: "[Naïve Bayes] Understanding mathematical concepts of Naïve Bayes classifiers with an example"
description: "[Naïve Bayes] Understanding mathematical concepts of Naïve Bayes classifiers with an example"
headline: "[Naïve Bayes] Understanding mathematical concepts of Naïve Bayes classifiers with an example"
typora-copy-images-to: ../images/2021-04-14
---

## Understanding Naïve Bayes classifiers

- Naïve Bayes are a set of supervised learning classification algorithms. Naïve Bayes predict labels of the class 'y' according to the conditions of features x<sub>1</sub>,x<sub>2</sub>, ... ,x<sub>n</sub>.

- Naïve Bayes are based on applying Bayes’ theorem with strong conditional independence assumptions between every pair of features given the labels of the class.

- <center><img src ="/images/2021-04-14/1.png"></center>

  <center><small>source: fastcampus.co.kr</small></center>

  - When fitting a Naïve Bayes classifier on the example data above, a label(yes or no) of playing tennis can be predicted depending on the conditions of features (outlook and humidity).
  - Following Bayes' theorem, the two features 'outlook' and 'humidity' are considered to be independent(although in real life they are related). 

<br>
- Bayes' theorem is stated as the following equation when A and B are events and P(B) &#8800;0. 

  <center><img src ="/images/2021-04-14/2.png"></center>

  

  Furthermore, When A<sub>1</sub>, ... , A<sub>*k*</sub> are a set of mutually exclusive events with prior probabilities P(A<sub>i</sub>) (*i* = 1, ..., *k*) , For any other event B for which P(B) > 0, the posterior probability of A<sub>i</sub> given that B has occurred is:

  <center><img src ="/images/2021-04-14/3.png"></center>

  

  By applying Bayes' theorem, the formula of Naïve Bayes is:

  <center><img src ="/images/2021-04-14/4.png"></center>


<br>

- Then, in the same example, the label of playing tennis when outlook is sunny and 		humidity is normal can be decided like this:

<center><img src ="/images/2021-04-14/5.png"></center>

<center><img src ="/images/2021-04-14/6.png"></center>

<br>


​		According to the argmax function, between *Yes* and *No*, the one with the higher probability will be labeled.

<center><img src ="/images/2021-04-14/7.png"></center>

<br>


​		Therefore,

<center><img src ="/images/2021-04-14/8.png"></center>



<center><small>source: fastcampus.co.kr</small></center>


<br>

- Regardless of the number of dependent features, Naïve Bayes classifier calculates the final probability is by the product of each probability, so the calculation process is simple and convenient. 
- Yet, as Naïve Bayes classifier assumes conditional independence between dependent features, the association between features cannot be explained in actual data. As already mentioned, in the example data 'outlook' and 'humidity' are considered to be independent although in real life they are related.



**Reference**

- 패스트 캠퍼스 머신러닝과 데이터분석 A-Z 올인원 패키지 Online 강의

- <https://en.wikipedia.org/wiki/Bayes%27_theorem>

- <https://scikit-learn.org/stable/modules/naive_bayes.html>

- <https://math.stackexchange.com/questions/948888/when-to-use-total-probability-rule-and-bayes-theorem>

  

