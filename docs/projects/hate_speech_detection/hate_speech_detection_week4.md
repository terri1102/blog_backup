---
layout: default
title: Hate speech detection 프로젝트 week 3
grand_parent: Projects
parent: Hate speech detection 프로젝트
has_children : true
nav_order: 3
---



### Week4 프로젝트 끝나고 다시 수정



할 일

1. 훈련 코드 수정: grayscaler 쓰는 게 성능은 더 좋은 것 같은데 아직 내가 이해하고 쓰는 느낌이 아니어서 삭제
2. TPU로 훈련 
3. Wandb sweep으로 하이퍼 파라미터 튜닝
4. 경량화 -- QAT는 forward 인자 개수가 자꾸 안 맞는다고 떠서, 모델 훈련 후 동적 Quantization을 사용할 예정



### 1. 훈련 코드 수정

와..grayscaler를 빼니까 성능이 엄청나게 떨어졌다. 사실 이것 말고도 토크나이저 설정도 바꿨어서 다시 원래대로 했는데도 성능이 떨어진 것을 보면 grayscaler가 엄청 중요했나 보다. 이것에 대한 공부를 더 해야할 것 같다. 정확도 0.55에 f1 score 0.1이라니... 데이터셋에 심각한 문제가 있는 걸지도.



### 2. TPU 훈련

TPU로 굳이 훈련할 필요는 못 느껴서 GPU로 하다가 Wandb sweep까지 넣으니까 램이 부족해져서 TPU로 돌리게 되었다. 2 에폭으로 돌렸는데, 이상하게 1에폭은 빠르게 진행되다가 2에폭의 20step에서 20분 정도 멈춰있다가 다시 시작되었다. 왜 그럴까..? 두번째로 돌려도 똑같은 부분에서 지연된다.



### 3. Wandb Sweep

첫 시도 실패

TypeError: train_bert() missing 9 required positional arguments: 'net', 'criterion', 'opti', 'lr', 'lr_scheduler', 'train_loader', 'val_loader', 'epochs', and 'iters_to_accumulate

## 
