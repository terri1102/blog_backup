---
layout: default
title: "NLP pipeline"
date: 2021-09-13
grand_parent: Deep Learning
parent: NLP
comments: true
nav_order: 4
---



d

자연어 처리의 태스크

1.  token classification

2. sentence classification





## **Data**

## **Vocab**

## **Train, Valid, Test 데이터셋 생성**

## **Modeling**

```python
class MiniClassifier(tf.keras.Model):
    """
    모델 클래스 생성
    """
    def __init__(self, args, name='MiniClassifier'):
        super().__init__(name=name)
        
        self.embedding = tf.keras.layers.Embedding(args.n_vocab, args.d_model) #vocab 개수, 모델 차원
        self.linear = tf.keras.layers.Dense(args.n_out) #추론 클래스 개수
        
    def call(self, inputs, training=False):
        tokens = inputs
        hidden = self.embedding(tokens)
        logits = self.linear(hidden)
        
        return logits
    
```

## **Train**

```python
#model 인스턴스화
model = MiniClassifier(args)

#train에 필요한 변수들 선언
EPOCHS = 100
BATCH_SIZE = 256

#Loss function, optimizer, metric 선언
optimizer = tf.keras.optimizer.Adam()
loss_function = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

train_loss = tf.keras.metrics.Mean()
train_acc = tf.keras.metrics.SparseCategoricalAccuracy()

val_loss = tf.keras.metrics.Mean()
val_acc = tf.keras.metrics.SparseCategoricalAccuracy()

```

```python
#train step, test step 함수 작성
@tf.function
def train_step
```



## **Evaluate**

## **Predict**
