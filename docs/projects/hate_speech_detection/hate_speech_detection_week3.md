---
layout: default
title: Hate speech detection 프로젝트 week 3
grand_parent: Projects
parent: Hate speech detection 프로젝트
has_children : true
nav_order: 3
---



### Week3



## Sentence transformer 모델 고르기

feature extraction인 것 중 가벼운 모델로 고르기

근데 mini 붙은 건 다 sentence similarity 여서 그냥 mini로 써보기

### Reference



## 모델 구조

context sentence를 어떻게 BERT 모델과 붙일까 계속 이 고민만 하고 있다...

그러다가 multiple inputs이라는 이 글에서 힌트를 얻어서 모델을 구상해보았다. 이 모델이 아직 돌아갈지 안 갈지는 모르겠다.

https://discuss.pytorch.org/t/nn-module-with-multiple-inputs/237/2



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



클래스 비율이 처참하다.  70:1 정도

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



7월 3일의 결과

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





Inference time 재는 법:https://towardsdatascience.com/the-correct-way-to-measure-inference-time-of-deep-neural-networks-304a54e5187f

