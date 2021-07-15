---
layout: default
title: Hate speech detection 프로젝트 week 2
grand_parent: Projects
parent: Hate speech detection 프로젝트
has_children : true
nav_order: 2
---



### Week2

둘째 주에는 모델을 이것저것 살펴보고, 재학습에 대해 고민해보았다.



### BERT Model

경량화된 BERT 모델을 찾다보니까 squeezeBERT, TinyBERT, ELECTRA 같은 모델들을 배우게 되었다. 응답속도가 빨라야 하니까 정확성을 조금 포기하더라도 가벼운 모델을 계속 찾아가는 것 같다. 



### 읽은 논문

* HateBERT: Retraining BERT for Abusive Language Detection in English https://arxiv.org/pdf/2010.12472.pdf
* ELECTRA
* Contextualizing Hate Speech Classifiers with Post-hoc Explanation  https://arxiv.org/abs/2005.02439



### 배운 점

Pytorch를 이번에 처음 써보게 되었는데, 확실히 huggingface도 그렇고 많은 논문들에서 구현을 Pytorch로 해서 배워두는 게 좋을 것 같다.  



양자화: 부동소숫점 연산을 정수 연산으로 바꿔서 약간의 정확도를 희생해 모델 사이즈를 4배정도 줄일 수 있다고 한다.



Ray tune: 하이퍼 파라미터 튜닝 라이브러리. 예전에 MLOps 커뮤니티 행사에서 들었었는데, pytorch로 써볼 수 있는 것 같다.





### 아쉬운 점

아쉽다기 보다는 걱정되는 점인데, 딥러닝 모델을 만들 때 개인적으로 가장 어려운 점은 레이어를 쌓으면서 tensor의 모양을 맞추는 것과 predict 함수를 만드는 것이다. 특히 예전에 QA 모델을 만들 때는 디코딩이 안 돼서 결과를 확인할 수 없었던 적도 있고...tensor 모양을 np.reshape나 torch.view로 막막 바꿔도 되는 건지도 궁금하고...



### 궁금한 점

논문들 보면 항상 모델을 패키지 형태로 올려놓던데, (ipynb 대신) 어떤 순서로 동작하는지 엄청나게 헷갈린다.  그리고 huggingface를 쓰는 논문도 있겠지만, 내가 읽었던 논문들은 BERT도 직접 클래스로 구현을 하던데...왜 그렇게 할까? pre-trained된 checkpoint를 써야해서?



전처리할 때 큰따옴표, 작은따옴표를 지울까 말까 고민 중이다. 이게 sarcasm을 의도하려고 쓰기도 하는 것 같은데, 데이터셋에 너무 많기도 해서...



### BERT Huggingface tokenizer와 embedding의 차이? 

HuggingFace의 tokenizer는 그냥 tokenizer일 뿐 임베딩을 하지 않는다. 각 token에 대응되는 숫자들의 벡터인 one-hot vector를 뱉어냄



## Sentence transformer 모델 고르기

feature extraction인 것 중 가벼운 모델로 고르기

근데 mini 붙은 건 다 sentence similarity 여서 그냥 mini로 써보기

### Reference

딥러닝으로 동네생활 게시글 필터링하기 https://medium.com/daangn/%EB%94%A5%EB%9F%AC%EB%8B%9D%EC%9C%BC%EB%A1%9C-%EB%8F%99%EB%84%A4%EC%83%9D%ED%99%9C-%EA%B2%8C%EC%8B%9C%EA%B8%80-%ED%95%84%ED%84%B0%EB%A7%81%ED%95%98%EA%B8%B0-263cfe4bc58d



Sentence embedding https://velog.io/@ysn003/%EB%85%BC%EB%AC%B8-Sentence-BERT-Sentence-Embeddings-using-Siamese-BERT-Networks





 



class HateSpeechClassifier(nn.Module):

\#https://discuss.pytorch.org/t/nn-module-with-multiple-inputs/237/2

 

```
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
