---
layout: default
title: "MLOps 커뮤니티 행사 후기"
date: 2021-06-05
parent: ETC
grand_parent: Reviews
nav_order: 1
comments: true
---



MLOps

Airmeet이라는 플랫폼에서 진행되었다.

[공지] 영상은 추후 페이스북 그룹을 통해 공개할 예정입니다.

![welcome_session]

1시에서 1시 30분까지는 소개였다.

2019년에 시작되었다. 모든 세션은 각 30분

code of conduct 모두를 배려하는 느낌



---

# MLOps 춘추 전국 시대

연사: 변성윤

소카 데이터 팀

발표자료는 공유할 예정



## MLOps

머신러닝 프로세스 -Research

문제정의 - EDA - Feature 생성 - 모델 학습 - 예측



보통 고정된 데이터를 사용해 학습

이런 머신러닝 모델을 웹, 앱 서비스에 적용하기(production)



Flask로 해보자! 도커로! 

![]

모델을 API 형태 또는 Batch 형태로 배포한 후 생기는 일

1) 모델 결과값이 이상한 것 같아요!

인풋 데이터는 0~23까지 데이터가 들어가야 하는데 77이 들어갔네? float이어야 하는데 int가 들어갔네?

2) 모델 성능? 

모델 모니터링 대시보드 미리 만들어서 확인. 리서치 환경에서와 프로덕션에서의 모델 성능이 다르기 때문에 모델 pkl 파일 바꿔가며 함..

모델링에 집중하고 싶은데 신경써야 할  다른 일이 많음

"Hidden Technical Dept in Machine Learning Systems"

![]

MLOPs 머신러닝 엔지니어링 + 데이터 엔지니어링 + 인프라

머신러닝 

ex) api 형태로 서버 만들기, 실험 파라미터와 결과 저장하기, 모델 결과 자동화하기

현실의 RISK를 줄이는 것

MLOps의 목표는 빠른 시간 내에 가장 적은 위험을 부담하며 아이디어 단계부터 프로덕션 ML 프로젝트를 진행할 수 있도록 기술적 마찰을 줄이는 것.

![research와 production]

요즘 MLOps는 엄청 다양하다

## MLOps component

요리를 좋아해서 집에서 타코를 만들 때, 타코 레스토랑을 만들 때의 차이

음식-모델, 식재료-데이터나 feature, 요리-학습

매일 20시 마다 서빙 받길 원하는 경우 batch serving을 함

online serving: 한번에 하나씩 포장해서 배송

serving: 프로덕션 환경에 사용할 수 있도록 모델을 배포

대표적인 serving 방식

1) Batch 단위: 주로 했던 거, 정해진 주기

airflow라 cronjob

2) API: 요청 받을 때

flask, fastapi, docker, kubernates, serving 프레임워크 쓰다가...

kubeflow, bentoml, seldon core, crtex, krserving, tensorflow serving

![서빙]



bentoml

깃헙 스타 숫자로 인기 확인



머신러닝 모델

artifact: 학습하는 과정에 생기는 이미지, 모델 파일

모니터링 대시보드가 필요함

wandb, mlflow 등



우버 논문: 모델을 선택하는 기준 같은 게 있음 mae < 5

MLFlow가 압도적 인기



feature store

모델이 많아지면 사용하는 feature도 같은 경우도 존재. 재사용 가능하게 해줌.

최초엔 배치 단위로 쿼리를 실행해서 db table에 그 테이블을 feature store로 사용하는 방법도 있음.

오픈소스는 Feast, Hopsworks가 대표 SageMaker, Vertex Ai(구글)

실시간 데이터를 받을 수 있음



feature 분포

feature decay

Data Drift, Model Drift, Concept Drift 시간이 흐를 수록 성능 떨어지면 재학습-이부분을 모니터링



Continuous Training

1. 새로운 데이터가 도착한 경우
2.  일정 기간(매일, 매시간)
3. 갑자기 매출이 줄은 경우
4. 

라이브러리가 있진 않고 CI/CD와 진행

우버: metric bias가 0.1 아래면 callback action을 해라

학습은 1시간마다 하되, 0.1 아래면 deploy해라



자동화 시스템(AutoML)



## MLOps 공부하는 방법

1) 회사에서 머신러닝 인프라 구축?

기본적으로 CI/CD, 클라우드, 쿠버네티스를 잘 알고 있으면 좋음. MLOps 기본 강의

엔듀류 응 MLOps, TFX 기반 강의

Full Stack Deep Learning: 실용적, 가장 추천

애저듣보잡 MLOps 101:  1시간 이내, 애저 기반

송호연님의 머신러닝 엔지니어 실무(인프런)



awesome-production-machine-learning : 깃헙, 테마별 정리되어 있음

awesome-mlops

클라우드의 MLOps 사용해보기



머신러닝 시스템 디자인 패턴: 방법론에 대한 이야기



글로벌 회사들의 MLOps Platform

구글의 TFX



논문

Pratitioners Guide to MLOps



추천 사이트

https//ml-ops.org/ 정리가 아주 잘 되어 있음



채용

머신러닝 엔지니어/데이터 엔지니어가 진행하는 경우가 대부분

MLOps : ML+DevOps 프로덕션 환경으로 가기 위한 여러 시스템

---

# Ray: 대규모 ML인프라를 위한 분산 시스템 프레임워크

## 머신러닝 인프라와 Ray

발표자: 조상빈

레이에어서 코어 분산 시스템 개발 담당

Anyscale에서 Ray 오픈소스 개발



머신러닝에서 분산처리에 대한 중요성이 늘고 있다.



industry 주요 워크플로우

HPC(High performance Computing), AI, Micro-services, Big-data

![복잡성증가]



머신러닝 파이프라인: 다양한 분산 시스템이 접합됨

피처 프로세싱/전처리(spark)->분산훈련(horovod, XGBoost, etc)->스코어링/서빙(spark, Neuropod)

데이터 전송을 위한 중간단계 스토리지, 워크 플로우 관리해주는 airflow



현대 머신러닝 인프라 : 계산 집약적인 분산 처리 필수





레이: 파이썬의 분산처리를 쉽게 해결할 수 있도록 만들어짐

-간단함: 3개의 API

```python
@ray.remote

```

ML 플랫폼에서 레이의 역할: 계산 집약적인 모델 훈련, 튜닝, 서빙 등에 사용됨



기본 ML pipeline

데이터 전처리/ 피처 프로세싱--모델 훈련/하이퍼 파라미터 최적화--모델 평가/디플로이먼트->pickling, 컨테이너



분산 시슽템 접합 필수

전처리 -> 스파크

데이터 로딩: pytorch dataloader, petastorm

모델 훈련/하이퍼파라미터 최적화 horovod

모델평가

워크플로우: airflow



레이 & 레이 라이브러리

전처리: spark, Dask on Ray

데이타 로딩 : Ray ML Dataset

Spark on Ray



pandas 분산처리

modin.pandas



![rayprocess]



## 레이와 레이 libraries 소개

### 분산 컴퓨팅을 위한 간단한 API

파이썬 

function: stateless

class: statefull . 상태에서 따라 input과 output 형태 달라질 수 있음

Ray에선

function->task

class->actor

### 라이브러리 에코시스템

자체 라이브러리: 

rllib 가장 인기 있는 scalable 강화학습 라이브러리. 

tune: 하이퍼 파라미터 튜닝

서드파티 라이브러리: 허깅페이스, Dask, spacy 등

### 범용 시스템/ 서버리스 추상화

stateless 대규모 데이터 분산처리(DAG)

stateful: 모델 서빙/ 강화 학습/ 스트리밍 프로세스

엑터를 만들 때 리소스를 요구하면 레이의 autoscaler가 자동적으로 클러스터를 스케일링함

### 고성능



### 언제 왜 써야하나?

언제: ML 인프라에서 계산 집약적 작업이 필요한 클러스터를 구현할 때

소규모/대규모 분산/ 병렬 처리작업

scalable한 라이브러리 개발



왜

범용성, 고성능

분산시스템 관리



## 레이의 실제 industry 사용 사례

우버: 레이를 이용

airflow 대신 파이썬 스크립트 train.py로 사용



---

# Data-centric MLOps

발표자: superb ai 이정권

데이터 구축!



AL / ML = Model + Data

왜 굳이 데이터 중심이라는 말을 하게 된 걸까? 

"From Model-centric to Data-Centric AI"

모델 성능을 올리기 위해 모델 코드를 바꿔야 할까 아니면 데이터를 바꿔야 할까? 데이터(80%)

문제 해결을 위해 가장 우선적으로 개선해야 할 것은 데이터다! 

데이터의 퀄리티: consistency, error rate, diversity 등

좋은 데이터란? 빅 데이터, 노이즈가 있는 데이터

딥러닝의 시대가 오면서 빅데이터 중요성 높아졌지만, 모든 상황에서 충분한 데이터 양 확보가 어렵다. 작지만 클린한 데이터가 중요해지고 있다.(consistent)



프로젝트 프로세스

첫달: 모델의 베이스라인 제작

그 후 계속 데이터 구축

Andrej Karpathy 2018: 실제 프로덕션에선 데이터 셋 구축이 어렵



제대로 분류하려면 이미지 몇개나 필요할까? 10만개?(보통 경험적) 연사의 답: 5000개 부터 시작

여전히, 잘 모른다. 단순히 지루한 작업을 자동화하는 과정이 아닌 ML 문제를 해결하기 위한 과정

여전히 도메인 전문가의 의견에 많이 좌우됨.



데이터 구축(1 iteration)

기간: 두 달 아래

데이터 구축 인원 30명 보다 작게

이미지 데이터 2만 데이터보다 작음



superb ai

시작점: labeling too, data 정제

reusable data spec: 들어오는 data는 계속 변경됨. 데엔이 json 파일보고 변경해서 넣으면 되지 않나? 굳이 이를 자동화해야하나? 응 해야해



어떤 pipeline을 만들어야 하나?

다른 문제는 다른 데이터가 필요하고, 다른 방법이 필요하다. 

빠른 딜리버리가 중요한 데이터도 있고, precision이 중요한 데이터 라벨링도 있다.

ms coco-정확도



human in the loop^2 

human intellegence를 뽑아내는 과정에 피드백을 주기 어려움. 쓸만한 모델이 완성되지 않은 경우도 많고, 서비스에는 다양하게 고려할 점이 있음



keep labels consistent

collision이 있는 영역..은 어떻게 할까? 불일치를 자동적으로 스팟해서 데이터를 클린하게



데이터를 구축에 참여하는 사람들은 ML의 모든 부분에 전문성이 있는 건 아니다. 따라서 복잡한 시스템 위에어 일어나는 피드백 시그널들을 자동적으로 처리할 수 있게 해야한다.



SDK

https://www.superb-ai.com/ko-blog/superbai-meets-valohai

---

# 당신의 모델에 날개를 달아드립니다

유홍근

Model development 위주 발표 <-> model serving

커피고래 https://coffeewhale.com



## 쿠버네티스를 이용한 MLOps

기계학습을 제대로 하기 위해

지속적 모델 개발/배포

컴퓨팅 자원 스케줄링

 ML 파이프라인 관리

다양한 모델 실험



기계 : 여러가기 경우의 수 계산-> 필연적으로 많은 기계가 필요

어떻게 하면 많은 기계들을 효율적으로 관리할 수 있을까?

근본적인 문제: 사람들이 직접 관리해서 할 일이 많은 것

이를 쿠버네티스에 맡기면 일이 더 간단해질 것



#### 쿠버네티스의 장점

컨테이너 스케줄링이 편해짐

확장성이 좋아짐. 클라우드 위에서의 경우. 서버 리소스 필요할 때 필요한 만큼만 사용

모니터링이 쉬워짐. 어떤 서버에 어떤 문제가 있는지 바로 확인 가능

장애에 견고해짐. 클러스터 시스템의 장점



### 쿠버네티스 단점

모두가 쿠버네티스에 익숙한 건 아니다. (컨테이너화 작업, manifest 파일(YAML 파일))

containerization: 모델 스크립트 한줄 수정할 때마다 재빌드...귀찮음

YAML 지옥(writing manifest): 쿠버네티스가 이해할 수 있게 작성해야하는데 좀 힘들 수 있음. 



### 해결

쿠버네티스 자체가 컨테이너 오케스트레이션인데 컨테이너화 작업 없이 가능?

처음부터 컨테이너 안에서 개발--JupyterHub on Kubernates

그럼 컨테이너 안에서 개발한 코드를 쿠버네티스에 어떻게 전달하나?

1) CI/CD 이용? git push->docker build & push

2)컨테이너로부터 이미지 생성? docker commit



ML학습에 필요한 3가지 요소

모델 실행환경, 모델 소스코드, 모델 파라미터 

k8s Manifest에서도 동일

어떤 실행환경, ...,...



JupyterHub를 이용하면 손쉽게 획득 가능

POD_info 로 확인 가능



jupyterhub를 통해 

JupyterFlow: 컨터에너화 작업 & Manifest 작성 없이 곧바로 학습할 수 있도록 도와주는 CLI툴

```bash
pip install jupyterflow
```



작성한 코드를 바로 클러스터로 실행 가능

쿠버네티스를 전혀 몰라도 됨...



예전에는...

모델 스크립트 작성, docker file 작성, 도커 빌드 푸시, k8s manifest 작성, kubectl apply



JupyterFlow 데모

jupyterhub에 들어감

나의 스크립트가 쿠버네티스 위에 실행됨-algo로 확인



![ml툴비교]



기존 wf 엔진을 사용해도 100% 대체 가능. 용도에 따라 유용한 툴이 될 수 있다.

최종, 배포용 느낌보다는, 실험 단계에서 주피터 사용할 때 쿠버 리소스를 잘 활용하기 위한 툴이군요!

---

# 모델을 데이터셋에 맞게 대량을 찍어내는 방법(w파이썬)

일반화 성능이 높은 모델하나로 모든 추론을 하는 것이 이상적이나 경우에 따라 지역적인 특성이 강하거나 개인화된 서비스가 필요하여 특성이 다른 데이터셋을 학습하고 추론해야하는 케이스도 많습니다. 만약 추론 대상이 1천개 혹은 1만개가 넘어가게 되면, 사람이 하나하나 모델링할 수 있는 수준이 넘어가기 때문에, 본 발표에서는 몇 개의 모델 아키텍처로 대량 추론을 할 수 있도록 학습 및 성능 모니터링을 할 수 있는 방안을 제시하고 이를 파이썬으로 구현한 프레임워크를 소개합니다.



발표자: 김태영 인공지능팩토리 대표이사

추후에 오픈소스로 만들어서 배포한다고 함



## 문제 정의

3천대 서버의 인공지능 기반 이상징후 탐지 모델을 만들어라!

데이터 입력 형식과 라벨 형식은 동일

데이터 분포는 3천대 서버마다 다름(유사한 것도 있음)



## 해결 방안

1. 3천대 서버의 모든 데이터셋을 학습시킨 글로벌 모델 생성

장점: 하나의 모델로 모든 서버에 대한 추론 가능

단점: 서버마다 패턴 및 데이터 분포가 다른 경우 높은 성능을 가진 글로벌 모델 생성이 쉽지 않음

2. 각 서버마다 개별 이상징후 탐지 모델 생성

장점: 각 서버 데이터에 맞는 모델 만들 수 있음

단점: 각 서버마다 구축하는 것이 일이 많고 어려움->MLOPs로 해보자



## 개념

Work Order--> traingen worker, testgen worker, predictgen worker/ train worker, test worker, predict worker

워커는 6종



오더는 워크플로우가 만듦(정책)

train workflow/test workflow/predict workflow -> WO -> workers



<예시>

데이터: mnist, fashion mnist

데이터셋 asset: train, test, real, arch, model, score

id 체계: task, train, test, arch, model

작업 요청서(workorder)와 작업자(worker)

WO 구성: wait, running, success, fail-파일로 함

6종의 worker: traingen_worker.py



1세대 모델을 만들기

Architecture(asset): arc00001_train.py, arc00001_test.py, arc000001_predict.py

```python
#arc00001_train.py
훈련, score
```

![train학습시험추론]

![test학습시험추론]

test에서는 스코어가 중요



1단계 -100개 중 10개 테스크의 모델1 스코어 확인 >> 60% 이상(가이드)

2단계-실패 테스크 중 한 개를 재학습시킴-> 모델2가 생김

3단계-신규모델로 실패 테스크를 평가 >>> 60%

4단계 - 실패 태스크 중 한개를 재학습시킴

5단계 신규모델로 ...

...

반복

9단계

이런식으로 훈련을 하면서 모델을 만들면 10개의 시험셋이었는데 훈련셋 생성 5개, 훈련 횟수 5, 모델이 5개가 만들어졌다. (아키텍처 갯수 1개)모델 개수나 훈련 횟수를 줄일 수 있다. (비슷한 데이터셋인 경우 한 모델이 여러 데이터셋에 활용 가능하나까)

하나 하고 전파한 다음 마지막까지 통과 못하면 사람이 한다던가 함



#### 케라스 튜너

아키텍처 안에 케라스 튜너나  auto keras 넣기도 함



다양한 시나리오는 workflow에서



**정리:** 우리가 관리할 것은 work order 뿐

추론 데이터 입력시, 시험셋 갱신시, 훈련셋 갱신시, 새로운 아키텍처 추가시

->work order만 추가하기. 이는 workflow로 관리

---

# KRSH: 선언형 kubeflow, Terraform처럼 KubeFlow 관리하기

발표자: 김완수 Riid

KRSH 오픈소스 소개



## KubeFlow Pipeline

쿠버네티스 위에서 컨테이너 기반으로 ML workflow를 구축하고 배포하기 위한 도구

주피터 노트북에서 노드를 공유

argo 기반으로 airflow처럼 워크 플로우 관리



#### 핵심 특징

1 조합 가능성

컴포넌트는 도커 이미지를 기반으로 구축된다. 여러 파이프라인에 걸쳐서 사용가능. (중복제거, 재사용가능)

2 이식성

클라우드 네이티브 아키텍처이기 때문에, 애저, aws, gcp, on premise 등 아무데나 디플로이 가능

3 확장성

리소스 스케줄링. <-> airflow 같은 파이프라인 도구와의 차이점



쿠버플로우 파이프라인

함수처럼 작동

유저 입력->파이프라인(컨테이너 Op-(volumeOp)->컨테이너 Op) ->

컨테이너 op: 특정한 동작을 하는 컴포넌트

VolumeOP: 컨테이너 op 사이에 마운트 되어서 공유하는 컨테이너



하나의 컴포넌트에는 도커이미지 하나 가짐. 볼륨 옵 등 다양하게 붙을 수 있음.



pipeline 작성

DSL compile --YAML 파일이 만들어짐

-tar 파일로..

수정할 때마다 배포 및 업로드하는 과정 자체가 귀찮음



## KubeFlow 단점

1. 형상관리가 어려움

쿠버플로우의 버전은 로컬, 깃헙레포, 쿠버플로우 다 다름. 복잡한 업데이트 과정 때문에 배포 주기가 딜레이됨

2. 업로드 과정이 불편함

3. CI/CD가 불편함



## KRSH, Declarative KubeFlow

terraform에서 많은 영향을 받음. write/plan/apply로 사이클을 단순화

pipeline.py: 실제 파이프라인

pipeline.yaml: 일반적으로 오토세팅되어 있음. config 파일 같은 것

---

전체적으로 느낀 점



Q&A

큰 데이터를 한번에 다운 받을 때 매번 노드 별로 레지스트리를 구성?

로컬 persistent volume이 있어야 하는데,

로컬에 있는 자원을 마운트할 수도 있음

볼륨옵으로 마운트(다운로드 아님) 로컬 스토리지를 nfs로 마운트하면 속도저하는 있을 것



실제로 발표자들이 개발하신 라이브러리에 대한 소개/홍보가 많았습니다. 기존 오픈소스 등과 비교해서 어떤 점이 더 우수한지 위주로 설명했습니다.



kubeflow의 단점 개선

git으로 형상관리를 통합

apply 기반으로 자동으로 repo에 업데이트

깃헙 액션에 krsh cli를 통합하여 쉽게 ci/cd 구축 가능



github.com/riiid/krsh



모든 발표자료: https://speakerdeck.com/mlopskr