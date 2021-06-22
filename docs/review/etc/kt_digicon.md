---
layout: default
title: "KT Digital-X Summit 2021 후기"
date: 2021-06-16
parent: ETC
grand_parent: Reviews
nav_order: 3
comments: true
---



<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/digital_x.jpg?raw=true" alt="ditigal_x" style="zoom:80%;" />

KT Digital-X Summit을 듣고서 관심있는 분야만 간단히 정리해보았다. 



1. 기업의 digital transformation

기업에게 주는 조언

작게 시작하라. 직원들이 작은 성공을 경험하게 하라. 

파트너십을 체결해서 다른 기업과 협력하라.

애자일한 조직 문화



2. 금융 DX 모델과 적용 사례

1) 환경 변화

모바일, 디지털 기반 비대면 고객

규제: 소비자의 권한 강화

규제완화: 오픈뱅킹, 데이터3법

2) 금융 DX 모델

통신 인프라 서비스 - 현재 1위이지만 지속적으로 개선

핵심 DX서비스: AICC

금융융복합 서비스: 불완전판매방지, 시장예측

3) 사례

닥터로렌(AI 네트워크 품질 관리), HCX, 콜체크인, 모바일통지

AICC-기가지니, 온라인 챗봇을 통해 상담 인입콜 감소

금융전용 cloud

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/kt_cloud.jpg?raw=true" alt="kt cloud" style="zoom:80%;" />

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/%EA%B8%88%EC%9C%B5%EC%9C%B5%EB%B3%B5%ED%95%A9.jpg?raw=true" alt="금융융복합" style="zoom:80%;" />

KT 잘나가게 앱 -> 소상공인 특화 맞춤 서비스 제공





![불완전판매방지](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/%EB%B6%88%EC%99%84%EC%A0%84%ED%8C%90%EB%A7%A4%EB%B0%A9%EC%A7%80.jpg?raw=true)

실제로 개발된 솔루션 설명을 해주는데 어떤 기술들이 들어갔는지 꽤 자세히 적혀있어서 



# KT 금융 DX 플랫폼

![AI개요](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/AI%EA%B0%9C%EC%9A%94.jpg?raw=true)

하나의 서비스 개발을 위해서는 여러 기술을 융합해야한다. 진짜 AI 모델 만드는 건 아주 작은 부분일 것 같다.



KT의 AI 플랫폼: AI Centro

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/ai%EA%B0%9C%EB%B0%9C%ED%94%8C%EB%9E%AB%ED%8F%BCai_centro.jpg?raw=true" alt="kt ai centro" style="zoom:80%;" />

<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/aicentro2.jpg?raw=true" alt="kt ai centro2" style="zoom:100%;" />



<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/centro3_deploy.jpg?raw=true" alt="centro3" style="zoom:80%;" />



진짜 편해보이긴 한데, 유료여서...kubeflow나 wandb랑 비슷해 보이긴 한다.

최근에 MLOps 행사에서도 그렇고 대부분 AI많이 활용하는 기업에서는 오픈소스만 쓰진 않고 각자 라이브러리 만들어서 쓰던데, 그만큼 투자하지 않는 기업들에게는 유용한 서비스일 것 같다. 



* 마이데이터로 인한 환경 변화

올해 8월부터 마이데이터 사업자는 표준 api를 통한 개인신용 정보를 수집, 활용 필요

앞으로 금융 산업에 엄청난 영향을 미칠 것임..그래서 우리 KT가 마이데이터 공통플랫폼을 만들었다. 그러니까 써라...

---

# AICC를 통한 컨텍솔루션 변화

* 채팅상담

인력 배치 효율화를 위해 적용함. 1대 다로 상담 가능. 인입콜 줄지 않을까? 하지만 뜻밖의 결과. 인입콜은 동일. 온라인 커머스의 경우 채팅 상담에 대한 만족도가 높아짐 (옴니 채널)채팅 상담을 유도하는 프로모션을 진행중.

* AI 챗봇 상담, 보이스봇

20대 고객들에게 보이스봇 먼저 오픈. 상담사와 상담하는 간단한 질문이 줄은 것으로 보임.

보다폰(Vodafone)의 예 : 영국의 이동 통신 사업자

ai가 고객의 intent를 분석. 44%의 콜을 ai가 담당



봇과의 대화의 문법: 단답으로 질문. 상담사에게 물어볼 때랑 다르게

고객들이 봇에 최적화된 질문을 함;;

그래서 목표: 고객의 intent를 빨리 찾기. 고객의 이력 검색해서

챗봇이 고객과 상담할 때 시나리오 작성에 엄청난 노력이 들어감. intent를 빨리 찾기 위한 시나리오도 계속 개선중



KMS(지식관리시스템)가 중요하다. 기술도 중요하지만 컨텐츠, 데이터 축적이 중요하다.

[VOC 분석](https://brunch.co.kr/@beusable/180)을 통해서 중복된 인입 감소시킴. 상담 프로세스 뿐 아니라 새로운 상품 개발에도 사용가능

---

콜체크인: 전화로 방문 등록

이것이 KT에서 만든 서비스였군...

---

# C-ITS & 자율주행

C-ITS(Cooperative-Intelligent Transport Systems)



차선 단위 HDD 분석(CCTV 정보 분석)

cits 신호정보

보행자와 돌발정보 ai로 감지

울산에서 구축 중. 올해 12월말에 도입