---
layout: default
title: Hate speech detection 프로젝트 week 1
grand_parent: Projects
parent: Hate speech detection 프로젝트
has_children : true
nav_order: 1
---



### Week1

첫째 주에는 주로 논문을 읽고 Hate speech detection이라는 도메인에 대해 조사하는 시간을 가졌다. 또한, BERT 계열의 경량화된 모델을 찾아봤다.



### Hate speech

혐오 발언의 종류도 다양하게 있어서 내가 타겟으로 할 혐오 발언에 대한 정의를 명확히 내리고 여기에 맞는 데이터셋을 찾아서 써야할 것 같다. HateBERT 논문에 나온 정의는 hate speech를 세 종류로 분류하고 있는데, 주로 특정 집단에 대한 혐오발언을 Hate speech라고 하는 것 같고, Aggresiveness는 공격성, Offensiveness는 무례한 발언으로 더 넓은 범위를 가리킨다.



### 읽은 논문

* [Toxic Speech Detection](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/custom/15744362.pdf)
* Toxicity Detection: Does Context Really Matter?
* A Benchmark Dataset for Learning to Intervene in Online Hate Speech

* HateBERT



### 배운 점

NLP 과제를 맡게 되어서 Transformer와 BERT 구조를 다시 공부했었는데,  생각보다 너무 얕게 알고 있었어서 새롭게 알게 된 내용을 정리해보았다. T academy에서 BERT 강의를 들었는데, 실습도 있고 transformer 부분을 아주 자세하게 설명해주셔서 좋았다.



### pooler layer

BERT 모델은 Embedding layer, Encoder, Pooler 이렇게 세 파트로 나눠서 볼 수 있다. Embedding layer는 단어들의 one-hot 인코딩한 값의 임베딩이고, Encoder는 self-attention head들을 가진 transformer 블럭이다. Pooler는 첫번째 토큰에 대응되는 output representation을 가지고 downstream tasks에 사용한다. 첫번째 토큰의 representation의 선형 변환을 수행하며, ㅜNext sentence prediction에 사용된다.



출처: https://github.com/google-research/bert/issues/1102



### 아쉬운 점

각자 프로젝트를 하는 것이어서 다른 분들의 코드를 참고하기 어렵고, 내 코드도 물어보고 싶었는데 아쉽다.



그리고 생각보다 시간이 너무 빨리가고, 프로젝트 진척 속도가 나지 않아서 조바심이 났다. 거의 논문 읽고 BERT 공부만 하면서 1주일을 보낸 것 같다.



그리고...딥러닝에 대한 이해가 너무 부족하다는 생각이 들었다. 물론 배운지 얼마 안 되서 그런 것도 있지만, 레이어를 자유자재로 쌓아서 모델을 만들기 보다는 huggingface에 있는 task 위주로만 복붙해서 사용해왔다는 느낌이 들었다.



궁금한 점...



nn.Embedding은 정수가 input으로 들어가는데 BERT는 원핫인코딩값이 들어가나?
