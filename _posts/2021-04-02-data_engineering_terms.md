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


## 4. 데이터 스트림(Data Stream) VS 배치(Batch)

- 먼저, 데이터 적재 유형을 **bounded data**와 **unbounded data**로 나누어 설명할 수 있는데, 
  - bounded data는 일단 저장되면 이후 변화가 없는 데이터이다. 이러한 데이터들은 묶어서 특정 시간에 한 번에 일괄처리하는데 이러한 처리방법을 배치처리(**Batch processing**)한다. 이에 대한 예시로는 매 월 단위 매출 데이터, 매 월 신규 고객 유치 수 등이 있는데, 이 경우에는 월 마다 데이터를 한꺼번에 처리할 수있다.
  - 한편, unbounded data는 끊임없이 지속적으로 적재되는 데이터로 임의의 주기를 정해 배치처리하거나 혹은 스트림 처리(**Stream processing**)할 수 있다. 예컨대 시스템 로그 데이터, 주식 가격 변동 데이터 등은 시간 혹은 일별 단위로 배치 처리 하거나, 필터링 등의 세부 프로세스를 거쳐 필요한 정보만  실시간으로 스트림 처리할 수 있다.

<center><img src ="/images/2021-04-02/3.png"></center>

<center><small>출처: www.oreilly.com, The world beyond batch</small></center>

- 한편, **Micro Batch**란 배치의 주기를 상대적으로 짧게 설정하여 준 실시간 처리의 효과를 내는 방법으로 일종의 스트림 처리로 분류된다.

- 아래는 스트림 처리 인프라 아키텍처의 한 예시이다. 앞 부분에는 서버 혹은 앱에서 지속적으로 생성되는 로그 데이터를 데이터 스트리밍 오픈 소스인 **Apache Kafka**를 통해 실시간으로 불러와 **Spark streaming**으로 전송하는 과정이 표현되어 있다. 여기서 Kafka는 데이터 유실을 방지하기 위해 초반에 데이터를 잠시 담아두는 저장고(**queue**) 역할을 한다. Kafka에서 넘어온 데이터는 Spark streaming에서 실시간으로 분석되며, 그 분석 결과는 **HBASE**라고 하는 일종의 NoSQL데이터 베이스에 저장된다.
  

## 5. 워크플로우(Workflow)

- 사전적으로는 작업 절차를 통한 정보 또는 업무의 이동로 정의되며, 데이터 워크플로우는 데이터 처리의 작업 절차를 의미한다. 

- 예컨대 ETL도 워크플로우 스케쥴링 도구를 통해 자동으로 진행할 수 있다.

- 아래는 워크플로우의 예시로 노드와 노드 간의 화살표를 통해 작업, 데이터의 흐름을 쉽게 확인할 수 있다.

  <center><img src ="/images/2021-04-02/4.png"></center>

  <center><small>출처: www.knime.com</small></center>

- 워크플로우는 Directed Acyclic Graph(**DAG**)의 속성을 가지거나 그렇지 않을 수 있는데, DAG는 방향성 비순환 그래프로 해석하며 이를 준수해야한다는 것은 워크플로우가 방향을 가지되 서로 순환하는 형태가 없어야 함을 의미한다.

- 워크플로우 엔진으로는 **Apache Oozie**(html기반), **Apache Airflow**(Python기반) 등이 있다.


## 6. 컴퓨터 클러스터(Computer Cluster)

- 여러 대의 컴퓨터들이 네트워크 상으로 연결되어 하나의 시스템처럼 동작하는 컴퓨터들의 집합을 의미한다. 

- 물리적으로는 여러 컴퓨터이지만 외부 사용자는 마치 한대의 컴퓨터 인 것으로 인식하며 사용할 수 있다.

- 컴퓨터 클러스터의 구성요소는 다음과 같다.

  - Node(Master + Slave): 서버
  - Network: 네트워크
  - OS: 운영체제
  - Middleware: 이 클러스터를 동작하게 하는 미들웨어

- 서버의 확장을 통해 우수한 성능을 내기위한 목적을 가진다. 컴퓨터 클러스터를 구축하기위해서는 분산 컴퓨팅(distributed computing) 기술이 필요하다.

- 클러스터에서는 단일장애지점인 **SPOF**(Single Point Of Failure)가 없어야 하며, 고가용성인 **HA**(High Availability)가 유지되어야 한다. 다시말해, 시스템 구성 중에 동작하지 않으면 전체 시스템이 중단되는 요소가 없어야 하고, 서버와 네트워크, 프로그램 등의 시스템이 지속적으로 운영가능해야한다.

  

  <center><img src ="/images/2021-04-02/5.png"></center>

  <center><small>출처: 패스트 캠퍼스</small></center>

  예를 들어, 위 그림에서 Master 1이 피치 못하게 중단 될 경우, 대비책인 Master 2가 동작하게 한다. 

  
**Reference**

- 패스트 캠퍼스 데이터 분석 입문 올인원 패키지 강의