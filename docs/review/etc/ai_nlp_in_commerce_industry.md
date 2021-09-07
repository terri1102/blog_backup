---
layout: default
title: "AI 콜로키움 커머스 서비스에서의 자연어 처리와 활용"
date: 2021-08-31
parent: ETC
grand_parent: Reviews
nav_order: 16
comments: true
---



## 커머스 서비스에서의 자연어 처리와 활용

AWS Sagemaker 사용한 자연어 처리



카카오 스타일 데이터 사이언티스트 진현두 toona.jin@kakaostyle.com



프로젝트 3개와 아마존 세이지메이커 소개할 예정

3개월 동안 Sagemaker 이용해서 모델 만들고 배포함



데이터 적재, 데이터 라벨링, 데이터 전처리 -- Sagemaker는 groundtruth라는 툴을 통해서 도와줌

Task 정의 - 모델 선택 : feature들의 distance를 잘 구하는 것이 필요

개발환경 구축(cuda, docker, venv) : 라이브러리 버전 --Sagemaker 도커 환경 구축 편리함

모델 학습, 하이퍼파라미터 튜닝 --Sagemaker studio 통해 주피터환경에서 실습 가능

모델 배포, 모니터링, 파이프라인 운영 --서버 증설이 쉬움



## 텍스트 데이터

* Tokenization : 코퍼스를 의미 단위로 나눠주는 전처리 단계

```python
from konlpy.tag import Mecab
tokenizer = Mecab()
text_1 = "AI 콜로키움에 초대해주셔서 감사합니다."
tokens_1 = tokenizer.morphs(text_1)
#형태소 단위로 토크나이징
```

* Text Classification

  해당 옷이 어느 클래스에 속할지 

* Sentiment Analysis : 감정 분석

  긍정, 부정 분류 등

* Text Segmentation

특정한 주제에 대한 표현을 찾는다고 할 때 단위는? 단어, 띄어쓰기, 문장 등

리뷰데이터를 이런식으로 나누기는 쉽지 않다.(문장 부호 잘 안 씀, ~여로 끝나는 문장인 경우 등) 

이를 해결하기 위해 text segmentation 사용해서 문장을 적절하게 나눈다. 토픽과 관련된 키워드를 찾거나 문서 내 중심 표현 찾음. 문장보다 작은 단위(절)로 나누기도 함.

* 그 외 Text autocomplete, Chatbot, Machine Translate

---

## 텍스트 분석 프로젝트 1

지그재그의 직진배송 리뷰 중 특정 키워드와 관련하여 긍정적인 감정을 느낀 리뷰를 추출

* 요구사항: 긍정적인 리뷰 추출, 특정 키워드(빠르다)와 관련한 리뷰 추출, 문장 내 관련 단어 하이라이트
* 해결: Word embedding과 코사인 유사도로 키워드 유사도 계산, classifier를 사용하여 토큰 importance 계산



단계

1. 유사 키워드 정의 및 데이터셋 구축

처음엔 zigzag의 인하우스 distilbert를 사용했음. 커머스에서 자주 사용하는 단어 잘 인식함. 하지만 잘못 분해하는 경우도 있었음.

데이터 양이 충분하면 새로 토크나이저를 학습하는 것이 좋고, 양이 적으면 pretrained된 토크나이저 사용.

fasttext를 이용해서 pretrain을 해서 사용함. oov에 대해 유연하게 대처해주기 때문에 fasttext 사용함

일정 threshold 이상의 연관성을 보이는 토큰이 있으면 라벨을 1로 달았음.

2. Dataset Augmentation

빠르다와 관련한 표현을 하이라이트하려고 했는데, 일단은 '빠르다'는 단어만 하이라이트 되었음. backbone 모델도 학습이 안 되어 있었기에 시간이 없었음. 그래서 데이터셋에서 해결책을 찾으려고 했음.   빠르다와 관련된 표현이 있으면서 커머스와 관련된 표현이 함께 있어야 1로 분류하게 만들었음. kc-bert 사용했음

3. 문장 하이라이트 1번 방법: LIME Explainer 사용.

   이미지를 부분 부분 자른다음에 부분을 이용한 예측을 하고, output probability가 크게 바뀌는 부분이 이미지의 중요한 부분으로 판단. 넥슨에서도 욕설 분류하려 사용함

4. 문장 하이라이트 2번 방법: 요슈아 벤지오가 발표한 self-attention 

첫번째 모델로 사용하여 학습했음. 현재 첫번째 방법으로 서비스되고 있으며, 두번째 방법으로 나아갈 예정임.



도커 이미지로 AWS ECR 레포지토리로 관리중

Sagemaker에선 SE에 저장된 것을 가져와서 도커 이미지로 가져와서 사용하였음



