---
layout: default
title: "요약태스크 중간 정리"
parent: Projects
nav_order: 4
nav_exclude: true
search_exclude: true
---



## 여태까지 한 것(~2021.10.15)

PEGASUS 논문 읽기

* https://www.youtube.com/watch?v=6WunfWyMadY
* 

BART 논문 읽기

* DBSA





해보고 싶은 것

- pegasus의 중요 문장 골라내는 알고리즘 적용
- TPU에서 T5 돌려보기
- 다른 데이터셋으로 pre-train하기
- longformer https://www.youtube.com/watch?v=i7aiBMDExmA
- bart 가중치로 longformer 돌려보는 거 찾아보기
- Long-short transformer https://www.youtube.com/watch?v=YzfkwFXafbI



의문점 해결...

Dataset class 만들 때 label을 오른쪽으로 안 밀어도 되는 이유

also, you won’t need to manually call `shift_tokens_right` to prepare `decoder_input_ids`, if you just pass `labels` the model will prepare the `decoder_input_ids` by correctly shifting them. 

https://discuss.huggingface.co/t/train-bart-for-conditional-generation-e-g-summarization/1904/2



## 나의 진행 흐름

1. PEGASUS 논문 읽음. 읽고 좋은 방법이라고는 생각했는데 한국어 pre-trained 모델이 없어서 패스
2. BART 논문 읽고 KoBART 돌려봄 -> fine tuning만 했는데 나름 준수한 성적
3. ~~ROUGE score로 데이터셋을 줄인다음에 돌려봄. 데이터 대략 66%정도로 감소했고, rouge score도 조금 떨어짐~~ huggingface dataset의 rouge_score가 한국어는 안 됨...아마 숫자로 인식한 것 같다고 하심.
4. Pre train 알아보기 (Don't stop pretraining 논문--VAMPIRE 이걸로 임베딩 스페이스 시각화하면 좋을 것 같음. DAPT나 TAPT 둘 다)

- 질문 : 어떤 데이터로 pre-train할 것인가?
- ~~tokenizer는 기존 bart_tokenizer에 추가했나??~~





내가 진행한 거 질문

pre train용 데이터 명세.

VAMPIRE로 문서 유사도 구해서 DAPT논리 만들기

Pre-train masking 잘 되고 있는지...? 



BART_large 모델의 세팅 : text-infilling 30%, permute all sentences



1. Text infilling



2. Permute all sentence -> 일단 모델이 input으로 문장 순서를 받는지 확인해야 함





---

Huggingface 관련 지식들

* `model.eval()`의 의미 : (Dropout modules are deactivated) -> inference할 준비됨

* `The warning Weights from XXX not initialized from pretrained model` means that the weights of XXX do not come pretrained with the rest of the model. It is up to you to train those weights with a downstream fine-tuning task.

* `save_pretrained`**(***save_directory: Union[str, os.PathLike], save_config: bool = True, state_dict: Optional[dict] = None, save_function: Callable = <function save>, push_to_hub: bool = False, **kwargs***)** :transformers.PreTrainedModel.from_pretrained` class method로 로드할 수 있게 저장
