---

layout: single
title: "데이터 엔지니어링(Data Engineering)관련 용어정리"
description: "Terms related to DE"
headline: "Terms related to DE"
typora-copy-images-to: ../images/2021-04-02

---



## 1. 데이터 크롤링(clawling)

- 데이터 크롤링은 데이터를 수집하는 행위이고, 수집하는 프로그램을 크롤러(clawer)라고한다. 데이터를 웹에서 수집할 때, 이를 웹 크롤링이라고 하며 이에 활용되는 프로그램를 웹 크롤러라고 한다.
- WWW는 하위링크와 다양한 정보들로 이루어져있는데, 웹 크롤러가 url주소를 기반으로 해당 데이터를 자동으로 수집한다. 수집 데이터들은 **html**형식을 가지는데, 이 형식으로는 분석하기 어려움으로, 필요한 정보를 추출하는 **parsing**이라는 전환과정을 거쳐야한다. 수집된 데이터에서 필요한 정보를 추출하는 전과정 전체를 통상적으로 **scrapping**이라고 표현하기도한다.

## 2. ETL(Extract, Transformation and Loading)

- ETL은 각각 Extract, Transformation, Loading의 약자이다.

- 내외부의 데이터를 추출하고 이를 필요에 맞게 변환한 후 저장하는 일련의 데이터 전처리 절차를 의미한다.
- ELT는 기존 ETL에서 transformation과 loading의 순서를 서로 바꾼 것으로 데이터의 크기가 너무 커 변환과정에서 너무 오랜 시간이 필요할 때, 로딩을 먼저하고 변환을 그 후에 진행하는 것이다.
- ETL은 데이터웨어하우스(저장구조)를 마련하는 데에 매우 중요하다.
- ELT 오픈소스도구로는 Apache NIFI, KNIME, Talend, Pentaho, Stream Sets등이 있다.



## 3. Data Warehouse(DW) VS Data Lake

- DW는 일종의 **데이터 저장소**인데, raw데이터가 아닌 **사용자 의사결정**에 도움을 줄 수 있도록, 여러 시스템의 데이터베이스에 축적된 데이터를 **공통의 형식**으로 변환해서 관리한다.

<center><img src ="/images/2021-04-02/1.png"></center>

<center><small>출처: 패스트 캠퍼스</small></center>

- 위의 아키텍처에서 ETL를 통해 데이터가 Enterprise Data Warehouse(EDW)에 적재되고, 이 데이터들은 mart라고하는 주제별 테이블로 처리되어, 사용자에 따라 적절한 도구(ex. BI)를 통해 분석된다.
- DW는 다음과 같은 단점을 가지는데, 먼저 ELT를 통해 대량의 데이터를 주기적으로 가져오고 복잡한 절차로 통합하기때문에 상당한 부하가 가중되고, 유지비용도 크다. 한편, 테이블이 잘 관리되지 않을 경우 중복데이터가 생기고 데이터 사일로(data silo)현상을 야기할 수있다. 가장 큰 한계로는 정형된 데이터만 취급하고 비정형데이터는 처리하지 못한다.



<center><big>VS</big></center>



- Data Lake는 DW의 한계를 보완하고자 나타난 개념이다.

<center><img src="/images/2021-04-02/2.png"></center>

<center><small>출처: 패스트 캠퍼스</small></center>

- DW가 정형데이터만 취급하는 것에 반해, Data Lake는 정형, 비정형, 반정형에 상관없이 **원래 형식** 그대로 저장하여 분석에 활용한다. 때문에 DW에 비해 더 큰 데이터 저장공간이 필요하다.
- Data Lake에는 다양한 빅데이터 기술 (ex.hadoop, Spark 등)이 포함되며, DW와 Data Lake를 함께 하이브리드의 형태로 사용하기 한다.



**This will be keep updating**



**Reference**

- 패스트 캠퍼스 데이터 분석 입문 올인원 패키지 강의
