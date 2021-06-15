---
layout: default
title: 강화학습을 이용한 주식투자 프로젝트 
parent: Projects
nav_order: 2
---

## 프로젝트 소개



## 강화학습





* 강화학습 모델 https://github.com/terri1102/rl_model_v1
* 데이터 불러오고, 전처리하는 패키지 https://github.com/terri1102/pyloadnprep 

신경망 모델과 강화학습 알고리즘 같은 경우는 파이썬과 케라스를 이용한 딥러닝/강화학습 주식투자의 코드를 많이 참고했고,  전처리 부분 코드는 사용한 특성이 달라서 많이 고쳤습니다.



## 프로젝트 타임라인

## 05.31

:chad: 

:heavy_check_mark: 강화학습 책 읽기 (파이썬과 케라스를 이용한 딥러닝/강화학습 주식투자)

:heavy_check_mark: 도서관에 스파크/카프카 책 예약하기



문제 정의 및 개선 방안 / 해결책

| 문제 정의                                       | 개선방안 | 솔루션                                |
| ----------------------------------------------- | -------- | ------------------------------------- |
| 대량 데이터 적재 및 하루에 한번 업데이트 자동화 |          | kubeflow의 스케줄링(당근마켓 글 참고) |

[데브시스터즈 데이터 레이크 구축 이야기 ](https://www.slideshare.net/awskorea/data-lake-architecture-case-study-bi-gaming-on-aws-2018)

[당근마켓 딥러닝 추천 모델 in production](https://medium.com/daangn/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EC%B6%94%EC%B2%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-in-production-fa623877e56a)





---

## 06.01(화)

:thinking:

:chad:

- [x] 프로젝트 계획서 작성 :heavy_check_mark: 
- [x] 키움 api 신청  :heavy_check_mark: 
- [ ] 키움 api 실습: 상장 주식수 얻기
- [x] 크래프트 영상 3개(금융과 딥러닝 :heavy_check_mark: , 동적자산배분 :heavy_check_mark: , 강화학습을 통한 집행전략  :heavy_check_mark: )
- [ ] 블로그 글 2개
- [ ] 사용할 스택 생각해보기 (postgres가 최선인지? 스파크 vs 빅쿼리)
- [ ] 스파크 공부
- [ ] 다른 기업들 조사:fount 깔아서 써보기, 키움의 로보어드바이저 보기



저녁 시간에 집중이 안 되는 것을 해결할 방법을 찾자!

오늘은 밖에 나가봐야지

---

**데이터 베이스:**

MariaDB와 Postgresql 둘 중에서 고민을 많이 했다. 오픈 소스 DBMS 중에 둘이 많이 사용되기 때문에. MariaDB: MySQL과 비슷하며 MySQL 커뮤니티 버전보다는 많은 기능을 제공하기에 한번쯤 써보고 싶었음. 엔진을 메모리로 선택가능. OLTP에 좀 더 적합

PostgreSQL: 복잡한 쿼리 실행 속도 빠름. DW, 데이터분석 분야에 많이 사용됨, SSD 환경에 적합

Postgres 쓰는 편이 나을 것 같다. 

[PostgreSQL vs MariaDB](https://rastalion.me/postgresql%EA%B3%BC-mariadb%EC%9D%98-%EC%82%AC%EC%9D%B4%EC%97%90%EC%84%9C%EC%9D%98-%EC%84%A0%ED%83%9D/) 



**데이터 전처리**

주가 데이터 + 가공 + PER

[스파크와 postgres 연결](https://severalnines.com/database-blog/big-data-postgresql-and-apache-spark)

---

더 벨 : 문제점-무료 버전은 3일 정도 느림-실시간 정보 반영 불가;

robots.txt

```
User-agent: *
Allow: /free/
Allow: /free/contents/
Allow: /newspartner/

Sitemap: http://www.thebell.co.kr/newspartner/google.asp
```



[Apache Airflow로 작업 흐름 개발해보기](https://medium.com/@aldente0630/%EC%95%84%ED%8C%8C%EC%B9%98-%EC%97%90%EC%96%B4%ED%94%8C%EB%A1%9C%EC%9A%B0%EB%A1%9C-%EC%9E%91%EC%97%85%ED%9D%90%EB%A6%84-%EA%B0%9C%EB%B0%9C%ED%95%B4%EB%B3%B4%EA%B8%B0-8f3653d749b4)

---

사용해보고 싶은 툴

* kubeflow

* spark

  window10에 spark 설치 https://icefree.tistory.com/entry/Spark-Window-10%EC%97%90-Spark%EC%84%A4%EC%B9%98

* apache airflow

  

* bigquery?? OLAP에 적합

* cupy: gpu 연산을 지원

* fastapi :matplotlib은 flask 지원하는 듯

---



---

## 06.02(수)

:chad:

- [ ] 키움 api 실습: 상장 주식수 얻기
- [ ] 블로그 글 2개
- [ ] 스파크 공부(T 아카데미)
- [ ] 다른 기업들 조사:fount 깔아서 써보기, 키움의 로보어드바이저 보기



1. 똥같은데이터 어떻게 전처리하나
2. 어떤모델에 어떻게 학습해야 성능나오나
3. 이 모델을 어디 얹어서 돌리고 서비스를 만드나
4. 이 과정을 어떻게 자동화할까



fc이전 레어에서 출력하는 피쳐맵크기가 달라지겠죠?뭐 Global Average Pooling이나 Conv레이어 트릭처럼써서 해결하려는 시도도..

 엔드디바이스 경량화 tf xla tvm tensorrt 같은거 찾아보십쇼

___

## WSL2 환경 구성



#### venv로 가상환경 구성하기

1. 설치

``` bash
$ sudo apt-get install python3-venv
```



2. 가상환경 만들기

```bash
$ python3 -m venv ./(가상환경이름)
```



3. activate

```bash
$ source (가상환경이름)/bin/activate
```



4. deactivate
```bash
deactivate
```


가상환경을 구성했으니 신나게 패키지를 깔아보자! pandas부터 다시 깔아야 하네...

ipykernel도 깔고

---

## 06.03 (목)

:chad:

- [ ] 키움 api 실습: 상장 주식수 얻기
- [ ] 블로그 글 2개
- [ ] 스파크 공부(T 아카데미)
- [ ] 다른 기업들 조사:fount 깔아서 써보기, 키움의 로보어드바이저 보기
- [x] 책 읽고 코드 이해

---

## 06.04(금)

:chad:

- [ ] 키움 api 실습: 상장 주식수 얻기
- [ ] 블로그 글 2개
- [ ] 스파크 공부(T 아카데미)
- [ ] 다른 기업들 조사:fount 깔아서 써보기, 키움의 로보어드바이저 보기

- [x] 책 읽고 코드 이해
- [ ] stacked autoencoder for time series
- [x] 도서관 다녀오기
- [ ] kubeflow 동영상
- [x] dl4j 동영상

---

## 06.06(일)

- [ ] 키움 api 실습: 상장 주식수 얻기
- [ ] 블로그 글 2개
- [ ] 스파크 공부(T 아카데미)
- [ ] 다른 기업들 조사:fount 깔아서 써보기, 키움의 로보어드바이저 보기
- [ ] stacked autoencoder for time series
- [ ] kubeflow 동영상
- [ ] FinRL https://towardsdatascience.com/finrl-for-quantitative-finance-tutorial-for-multiple-stock-trading-7b00763b7530

https://paperswithcode.com/paper/finrl-a-deep-reinforcement-learning-library

## 해결해야하는 문제

내가 원하는 것: 매일매일 api 가 작동해서(파이썬 스크립트) db에 데이터를 적재하는 것

매매를 너무 자주 하지 않게 설정. 수수료를 높이면 되나? 주 봉으로 투자 행동을 결정

강화학습의 신경망으로 어떤 게 적합한지 알아보기

stacked autoencoder의 결과물이 어떤 건지(전처리의 결과물) 정확하게 알기!

속도 개선: 한 종목의 16년치 데이터를 학습하는데 10분 이상 걸림. 근데 이거를 1000번 이상 반복해야해.

하이퍼파라미터 튜닝방법 (sigmoid->relu, sgd->rmsprop or adam)

주의점: csv 만들 때 header 없게 해야함

추가로 필요한 작업

locale 설정

mplfinance install

multi stock trading: 거래하는 종목이 증가할 수록 상태와 액션의 차원이 지수적으로 증가하게 된다.

FinRL 라이브러리: https://towardsdatascience.com/finrl-for-quantitative-finance-tutorial-for-multiple-stock-trading-7b00763b7530



---



프로젝트 기획서

* 주제

* 프로젝트 목적

* 가설 및 예상결과
* 분석방법
* 언어 및 환경
* 예상 결과물
* 데이터셋 제출

---

### 주간 리포트

**이번 주 진행사항**: 

우분투 환경 구축

사용할 강화학습 코드 이해하기+ 제대로 작동되는지 확인

시계열 데이터에 대한 이해: 책 읽기

스파크 공부



다음주 계획: 웹 앱 개발, 스파크로 자동화

세부사항: 앱 개발 및 학습 빠르게



---

Final 리포트

프로젝트 개요

이번 프로젝트는 시계열 데이터를 이용해서 강화학습을 통해 주식 투자를 할 수 있게 하는 것입니다.

프로젝트 기획 배경

시계열 데이터 처리에 대해 관심이 계속 있었고, 딥러닝을 주식투자에 활용해 보고 싶어서 프로젝트를 기획하게 되었습니다. 실제로도 딥러닝/머신러닝을 이용한 주식 투자 서비스가 많이 나오고 있어서, 이와 비슷하게 구현해보려고 노력하였습니다. 또한, 취직방향에 대해 고민했을 때 일부 증권사나 핀테크 기업에서 강화학습 관련 구인공고를 보았기에 프로젝트 주제를 주가 데이터 분석으로 선정해서 진행하게 되었습니다.



프로젝트 진행 방법

-시계열 데이터 전처리

-모델링

-배포

시계열 데이터 전처리 과정에서 여태까지 배운 분석 방법들을 다양하게 사용해 보려고 했습니다. 

제가 사용한 데이터는 크게 거시 시장 지표(국채지수, 환율, 이자율 등) /종목별 기술지표(주가를 이용한 /종목별 가치지표가 있습니다. 위의 데이터에 추가해서 종목별 종가를 오토 인코더를 통한 feature extraction 후에  pca로 축소하여 일부 선택하여 사용하였습니다. 

2000년 1월 1일부터 주가를 사용한다고 하면 5000개 정도 입니다. 따라서 차원 축소의 필요성 때문에 





-모델링: 강화학습에 대해서 처음 공부해봤기 때문에 ''파이썬과 케라스를 이용한 딥러닝/강화학습 주식투자'' 책에 나온 코드를 많이 참고하였습니다.  강화학습 기법으로는 A3C를 사용했으며, 신경망은 LSTM을 사용했습니다.



프로젝트 결과

프로젝트의 결과는 시계열 데이터 전처리 패키지와 강화학습 모델입니다. 앞으로 배포까지 진행할 예정입니다.

프로젝트 기대 효과

주식투자에 있어서 시계열 데이터를 전처리 파이프라인 구축과 다양한 기법의 강화학습을 통해서 투자에 도움을 받을 수 있습니다.



---

앞으로 더 보충할 것

전처리 패키지의 statistical 검증: heteroskedasticity, multicollinearity, serial correlation



시간 관리 잘 하자

시간 모자란 이유 1) 놀아서 이틀 낭비한듯

2) 스파크 파서..



---

네임서버 정보(dns)

| 1차 네임서버 | ns1.hosting.co.kr | 121.254.170.11  |
| ------------ | ----------------- | --------------- |
| 2차 네임서버 | ns2.hosting.co.kr | 114.108.175.146 |

주소: goldenbeetle.space





---

## references

https://machinelearningmastery.com/autoencoder-for-regression/

https://github.com/borisbanushev/stockpredictionai#fouriertransform

