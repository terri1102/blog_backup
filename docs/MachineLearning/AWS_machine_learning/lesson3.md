---
layout: default
title: Lesson 3
date: 2021-07-01
grand_parent: Machine Learning
parent: AWS Machine Learning course
nav_order: 3
---



## Machine Learning with AWS



### AI Services 예시

* Amazon Transcribe Medical: Health AI, medical 스피치를 텍스트로 변환, 음성 인식(ASR 서비스)

타겟층: 의사 - 텍스트로 증상 적는 데 드는 시간에 환자들에게 더 많이 신경 쓸 수 있음.

* Amazon Monitron: industrial AI
* Amazon Lookout for metrics: anomaly detection
* Amazon Lex: chatbot, public voice or text bots
* Amazon Personalize: highly customized recommendation
* Amazon Forecast: 예측 모델
* Amazon Fraud Detector: 사기, 횡령 등 검출
* Amazon Codeguru & Develops guru : 코드 퀄리티 개선, 가장 비경제적인 코드를 찾아줌.

* Amazon Rekognition: Vision
* Amazon Polly: 음성합성
* Amazon Translate & Amazon Textract: OCR 후 번역
* Contact Lens: 고객 문의 사항 듣고 감정 분석, 문제 분류
* Amazon Kendra: 비정형 데이터에서 검색 가능



## Amazon Sagemaker

1. Amazon Sagemaker Studio: 모델 생성, 훈련, 배포까지 가능. scalable
2. Amazon Sagemaker Distributed Training: 분산 훈련도 지원. 자동으로 모델과 데이터를 파이션해줌.
3. Amazon Sagemaker Clarify: ML workflow의 bias를 검출해주고 모델 설명해줌



---

## Computer Vision 

1. Computer Vision : 기계가 이미지와 비디오에서 패턴을 인식하고 고수준에서의 이해를 할 수 있게 하는 분야
2. 기대효과: 생산성 향상, 창의적 문제 해결

딥러닝 이전의 CV: use case에 한정되어 있었음. 일일이 그림의 피처를 annotate했어야 했음

인공신경망 구조

input layer

hidden layer: 숨어있는 중요 피처들을 찾음. 각 레이어별로 다른 특성들을 찾음.

초기 레이어: Edge detection 등 베이직 패턴을 찾음. 그 후 깊어질 수록 텍스처 등의 특징 찾고 이렇게 찾은 고수준 특성은 output layer에 들어가서 분류됨

output layer



## Use cases

* Image Classification: text detection이나 OCR(Optical Character Recognition), content moderation
* Object detection: 다양한 오브젝트를 검출. 오브젝트가 여러 개 있으면 여러 개 검출
* Semantic segmentation: 픽셀 단위로 오브젝트 검출. 오브젝트의 위치를 알려줌
* Activity recognition : video



---



# AWS DeepLens

아쉽게도 DeepLens device가 있어야 이 서비스를 제대로 이용할 수 있다!

그래서 이번 단원에서는 AWS를 이용한 분석에 도움되는 정보들만 적어봐야겠다.

* 데이터 수집: 데이터를 수집해서 AWS S3에 적재한다
* 모델 훈련: Amazon sagemaker 안의 주피터 노트북을 이용해서 모델을 훈련한다.
* 모델 배포: AWS Lambda를 이용해서 기기에 배포한다.
* 모델 결과 확인 : Amazon IoT Greenrass를 이용해 배포된 모델의 결과를 확인한다.

!!! 모델 훈련 후 모델 저장하고 세션 clear해야 함. 버켓도 지워야하고, sagemaker 노트북도 지워야함. 그래야 돈이 안나감.



Lambda : function을 만들어서 모델 inference 가능하게 해줌.



