---
layout: default
title: Hate speech detection 프로젝트 week 3
grand_parent: Projects
parent: Hate speech detection 프로젝트
has_children : true
nav_order: 3
---



### Week3

이제 모델 코드를 짜야할 시간...



### BERT Model



### 읽은 논문

* Sentence Transformer

### 배운 점





### 아쉬운 점





### 궁금한 점



## Sentence transformer 모델 고르기

feature extraction인 것 중 가벼운 모델로 고르기

근데 mini 붙은 건 다 sentence similarity 여서 그냥 mini로 써보기

### Reference



## 모델 구조

context sentence를 어떻게 BERT 모델과 붙일까 계속 이 고민만 하고 있다...

그러다가 multiple inputs이라는 이 글에서 힌트를 얻어서 모델을 구상해보았다. 이 모델이 아직 돌아갈지 안 갈지는 모르겠다.

\#https://discuss.pytorch.org/t/nn-module-with-multiple-inputs/237/2

 ![model_structure]

```
#print(model)
Some weights of the model checkpoint at google/electra-small-discriminator were not used when initializing ElectraModel: ['discriminator_predictions.dense_prediction.weight', 'discriminator_predictions.dense.weight', 'discriminator_predictions.dense_prediction.bias', 'discriminator_predictions.dense.bias']
- This IS expected if you are initializing ElectraModel from the checkpoint of a model trained on another task or with another architecture (e.g. initializing a BertForSequenceClassification model from a BertForPreTraining model).
- This IS NOT expected if you are initializing ElectraModel from the checkpoint of a model that you expect to be exactly identical (initializing a BertForSequenceClassification model from a BertForSequenceClassification model).
```



```
HateSpeechClassifier(
  (electra): Sequential(
    (0): ElectraModel(
      (embeddings): ElectraEmbeddings(
        (word_embeddings): Embedding(30522, 128, padding_idx=0)
        (position_embeddings): Embedding(512, 128)
        (token_type_embeddings): Embedding(2, 128)
        (LayerNorm): LayerNorm((128,), eps=1e-12, elementwise_affine=True)
        (dropout): Dropout(p=0.1, inplace=False)
      )
      (embeddings_project): Linear(in_features=128, out_features=256, bias=True)
      (encoder): ElectraEncoder(
        (layer): ModuleList(
          (0): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (1): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (2): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (3): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (4): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (5): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (6): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (7): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (8): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (9): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (10): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
          (11): ElectraLayer(
            (attention): ElectraAttention(
              (self): ElectraSelfAttention(
                (query): Linear(in_features=256, out_features=256, bias=True)
                (key): Linear(in_features=256, out_features=256, bias=True)
                (value): Linear(in_features=256, out_features=256, bias=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
              (output): ElectraSelfOutput(
                (dense): Linear(in_features=256, out_features=256, bias=True)
                (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
                (dropout): Dropout(p=0.1, inplace=False)
              )
            )
            (intermediate): ElectraIntermediate(
              (dense): Linear(in_features=256, out_features=1024, bias=True)
            )
            (output): ElectraOutput(
              (dense): Linear(in_features=1024, out_features=256, bias=True)
              (LayerNorm): LayerNorm((256,), eps=1e-12, elementwise_affine=True)
              (dropout): Dropout(p=0.1, inplace=False)
            )
          )
        )
      )
    )
    (1): Dropout(p=0.3, inplace=False)
  )
  (lstm): Sequential(
    (0): Embedding(66000, 3)
    (1): LSTM(128, 2)
  )
  (out): Linear(in_features=256, out_features=2, bias=True)
)
```



문제 concat할 때 벡터사이즈 안 맞음

https://discuss.pytorch.org/t/concatenation-of-the-hidden-states-produced-by-a-bidirectional-lstm/3686

-> L2 pooling을 해라





성능 너무 안 나오면 클래스 비율 조정. 데이터셋 교체



클래스 비율이 처참하다ㅋㅋ 70:1 정도

그래서 NLP의 oversampling에 대해 찾아보았다. 텍스트 representation을 구한다음에 resampling하는 것 같다.

https://simonezz.tistory.com/92?category=892980



Data augmentation

https://towardsdatascience.com/how-i-handled-imbalanced-text-data-ba9b757ab1d8

1. document를 tokenize해서 sentence로 만듦
2. 이를 shuffle하고 rejoin해서 새로운 text를 만듦
3. 아니면 형용사, 동사 등을 유의어로 바꿈
4. 유의어로 바꿀 때 pre-trained word embedding이나 NLTK의 wordnet 등 사용



아니면 번역 후에 다시 영어로 번역함

여기 깃헙 보고 세 가지 방법 참고해보기

https://github.com/kothiyayogesh/medium-article-code/blob/master/How%20I%20dealt%20with%20Imbalanced%20text%20dataset/data_augmentation_using_language_translation.ipynb



미디엄 글 막히면 시크릿 창으로 보기



문제: 내가 참고한 ALBERT모델을 이용한 sentence pair classification 코드가 ELECTRA에 적용하니까 오류남



에러1: 

not enough values to unpack (expected 2, got 1)

```python
cont_reps, pooler_output = self.bert_layer(input_ids, attn_masks, token_type_ids)
```

bert layer의 output의 개수가 ELECTRA랑 ALBERT가 다름



위의 코드를 수정

```python
pooler_output = self.bert_layer(input_ids, attn_masks, token_type_ids)

```



에러2:

```python
dropout(): argument 'input' (position 1) must be Tensor, not BaseModelOutputWithPastAndCrossAttentions
```

```python
logits = self.cls_layer(self.dropout(pooler_output))
```

저 dropout에 들어갈 pooler_output이 BaseModelOutputWithPastAndCrossAttentions여서 생긴 문제다. Tensor여야 하는데..



즉 에러1과 에러2는 연관 되어 있는데, ELECTRA 모델의 output이 BaseModelOutputWithPastAndCrossAttentions 객체 하나만 나오기 떄문에 두 에러가 발생한 것이다. 



해결: x

config에서 return_dict=False로 설정하면 BaseModelOutputWithPastAndCrossAttentions가 아닌 tensor의 tuple이 나온다.

from transformers import ElectraConfig, PretrainedConfig 더 알아보기



https://huggingface.co/transformers/v2.9.1/main_classes/configuration.html#transformers.PretrainedConfig



configure하기

```python
config = ElectraConfig.from_pretrained('google/electra-small-discriminator') 
config.return_dict = False
model = ElectraModel(config)
```



---

## 07.03

https://zzsza.github.io/mlops/2021/04/18/bentoml-basic/

한국어 혐오발언을 분류하는 모델을 만든다음에 논문을 써보는 게 어떤지 제안해주셨습니다.



이창우님

데이터셋 구축 40만개 ㄷㄷ

cross-dataset의 성능이 낮았음

데이터셋을 많이 줄이고 조합을 중요하게 생각해봤음

랜덤한 수를 주고 그 수에 속한 데이터셋을 뽑아서 cross 검증을 했었음

데이터셋 조합을 아주 다양하게 

Glove+bilstm

aucroc

결론: 데이터셋이 큰 게 잘 나온다.

질문: 전처리? 

val factor: roc auc에 대한 평균을 낸다음에 데이터셋의 샘플 개수로 나누었음. net score: 모델의 효율성(작은 데이터셋으로 얼마나 성능 좋게 나오게 만드나?)

sem_eval, hate_eval의 데이터셋이 좋았음

sem_eval의 public dataset으로 했을 때는 낮았는데 샘플이 적어서 그런 거 같다.



데이터셋: bench mark dataset+sem eval + hate eval



Tokenizing을 어떻게 할 것인가? Embedding을 어떻게 할 것인가? 

Glove: 100d

임베딩 후에 FC 대신 Logistic이나 Randomforest, lgbm 등 써볼 것이다. (시간 효율)



35만 개 데이터셋을 밸런스를 맞춰서 줄였음.->15만 개로

꿀팁ㅋㅋ토크나이징을 한번에 하면 안 좋음..

Tokenizer = transformers.Autotokenizer.from_pretrained

Bert의 마지막 히든 레이어를 가져올 수 있음()

64개를 벡터로 바꿈

np.concat해서 일일이 벡터로 만든 다음에 붙여줌

그리고 pkl로 중간중간 저장하면 좋음



'detect all abuse' (인성님)

트위터로 훈련한 모델로 Reddit을 predict하면 정확도가 많이 떨어진다.

데이터셋을 이것저것 섞어서 하니까 좀 낫더라..



한 에폭 2시간ㅋㅋㅋㅋ

---

최인성님

BERT 사용. 데이터는 3만 개 조금 안 됨

데이터셋 클래스 비율? 

전처리를 더 해야 할 것 같다! 

전처리: strip, url 자르고, ~~stemming, stopword~~

kaggle에서 긁어 온 Reddit 데이터셋이 안 좋음..h e l l o, 이모티콘

---

kaggle jigsaw 15만개. 완전 imbalance

문장의 글자수를 줄이는 게 좋다!

BERT 계열은 무거워서 빨라야 하는데, 데이터도 GPU에 넣어야 연산이 빠리진다.

GPU 메모리가 얼마 안 되서 이를 개선하려고 한다.

전체 데이터를 CPU에 넣고, batch로 잘라서 가져올 때 Gpu로 넣은 다음에 써야함...

---

(보윤)



## 1. 데이터셋 선정

Toxicity Detection: Does Context Really Matter?

10000개의 샘플. 엄청난 클래스 불균형

0    0.9849 1    0.0151

A Benchmark Dataset for Learning to Intervene in Online Hate Speech: Gab, Reddit

Gab에서 3266개의 혐오 발언 일단 사용 (parent: non hate, text: hate)

GPT-NEO

## 2. 데이터 전처리

https://~~ → url

@dfdfjkejlp → username

🐀 → 'rat'

## 3. 모델

Albert base v2: sentence pair classification 모델, sentencepiece tokenizer

## 4. 결과

Toxicity Detection: Does Context Really Matter?

{'accuracy': 0.985, 'f1': 0.0} → 클래스 불균형이 심해서 한쪽으로만 찍는 모델

A Benchmark Dataset for Learning to Intervene in Online Hate Speech를 섞은 데이터셋

{'accuracy': 0.9868421052631579, 'f1': 0.974396488661302} → 너무 높아서 뭔가 잘못된듯..

167개 샘플 예측하는데 걸린 시간: 60.3411초

대략 1개당 0.3613초

50ms는 어떻게...?



Reddit 전처리를 할 때 [deleted] 같은 거랑 외국어 처리!

---

contextual data가 부족함

sub-task로 감정 분석을 훈련, hate speech 분류

두 개의 히든 벡터를 융햡(confusing)



퍼러비스, CRR 채용시 오픈소스를 얼마나 목적에 맞게 `빨리` 쓰느냐! 

transformer나 seq2seq 코드 

---

서비스로도 구현하고 싶다

악플 분류

사회적 가치 실현

데이터 수집: 각각 커뮤니티에서 수집, 건전 사이트에서 반대가 많이 찍히는 글들

필터링해야하는 단어들을 구함

영어로 프로젝트 한 것과 비교??

유튜브 댓글은 test set으로 구체적으로 예시까지

유튜브나 트위터 api를 사용해서 댓글 쓸 때 바로 감지

flask로 배포

---

나치 관련 단어는 독일어

언어 감지 라이브러리



Bert("multi-lingual-based")

---

Hate speech

context가 없어도 hate hate speech인 데이터가 의미가 있나?

AI가 toxic speech를 하게 될까봐 시키나?



---

out side is too hot. [sep] take off your jacket

hi i'm a teenager [sep] take off your clothes

---

경현님

jigsaw, gab, reddit: jigsaw 두 개 중에 1만 꺼내서 씀.

jigsaw?

전처리

distillroberta : roberta가 bert 보다 좋아서

TPU로 돌렸음



궁금점? 하이퍼 파라미터 or 데이터셋 크기에 모델 영향 받나?

cross-entropy 0.8, 0.3

데이터셋에 따라 혐오의 정의가 다르다. 데이터셋을 여러개를 섞어써서 정확도가 낮아졌나 생각중.



문장 2개는 리스트로 전처리 후 [sep]. 문장 1개는 그냥 넣었음

임베딩 레이어

fully connected layer

(창우님)

BERT 마지막 레이어의 히든 스테이트를 뽑아서 randomforest, lgbm 돌림.

Glove 보단 좋은데 BERT 보단 안 좋음



electra랑 distillbert 보다 logistic이 몇 만배 빠름.



Inference time 재는 법:https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f

