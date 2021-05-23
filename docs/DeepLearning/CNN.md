---
layout: default
title: Image Classification Pipeline
date: 2021-05-23
parent: Deep Learning
nav_order: 5
comments: true
---



Table of Content

# Image Classification

첫 번쨰 예시는 전이학습 없이 그냥 레이어를 처음부터 쌓는 방식의 pipeline이다. 전이학습을 한다면 모델링 부분에서 이미 학습된 레이어를 새롭게 만든(분석하려는 데이터셋에 fine tuning한) 레이어와 합쳐주면 된다.

## 1. Loading Dataset

```python
import zipfile  
import glob  
from skimage.io import imread 
from skimage.transform import resize 

with zipfile.ZipFile('train.zip', 'r') as zip_ref:
    zip_ref.extractall('train/') #train 폴더를 생성하고 여기에 압축 풀음
    
img_list = sorted(glob.glob('train/train/*.jpg'))

IMG_SIZE = 227 #이미지의 픽셀 크기 
train_data = np.empty((len(img_list), IMG_SIZE, IMG_SIZE, 3), dtype=np.float32) 

for i, img_path in enumerate(img_list):
    img = imread(img_path)
    img = resize(img, output_shape=(IMG_SIZE, IMG_SIZE, 3), preserve_range=True)
    train_data[i] = img
```



## 1-2) Labeling(Optional)

data augmentation을 안 할 것이라면 이때 라벨링을 해도 된다.  아래의 예시는 클래스별로 이미지가 저장되어 있었는데, 이를 각각 numpy array로 만든 것을 data_list로 묶어서 사용했다.

```python
data_list = [dog_data, elephant_data, giraffe_data, guitar_data, horse_data, house_data, person_data]
y_train = []
for idx, x in enumerate(data_list):
  print(idx, x.shape) #x.shape으로 각 클래스의 데이터 개수 확인
  y_train.extend([idx] * x.shape[0]) #y_train에 dog_data의 len만큼 0이 들어가고, elephant_data의 len만큼 1이 들어가고...각 클래스마다 클래스의 데이터 개수만큼 리스트에 숫자로 들어가게 된다. 
```



## 1-3) Shuffle(Optional)

나중에 train_test_split 할 때 섞어도 되고, 이렇게 따로 데이터를 섞어도 된다.  shuffle 함수에 x와 y를 같이 넣으면 x와 y의 연결이 유지되니까 섞는다고 라벨이 달라지지 않는다.

```python
from sklearn.utils import shuffle
import numpy as np

#X = np.array([[0, 0, 0], [1, 1, 1], [2, 2, 2], [3, 3, 3], [4, 4, 4]]) X_train은 이미 numpy array여서 안 해도 됨
y = np.array(y_train) #리스트를 numpy array로 변환
X_train, y_train = shuffle(X_train, y)
```



## 2. Preprocessing

## 2-1) Dataset Split

```python
from sklearn.model_selection import train_test_split

X_train, X_val, y_train, y_val = train_test_split(X_train, y_train, test_size=0.25, stratify= y_train, random_state=42)
print(X_train.shape, X_val.shape, y_train.shape, y_val.shape)
```



## 2-2) Data Augmentation

이미지 데이터를 여러 가지 변형을 통해서 데이터를 늘리는 작업을 할 수 있다. keras 에서 아주 편리하게 구현되어 있다.

```python
from keras.preprocessing.image import ImageDataGenerator
datagen = ImageDataGenerator(
        rotation_range=40, #이미지 회전 #이미지 말고 다른 형태의 데이터일 때는 잘 안 쓰는듯
        width_shift_range=0.2, 
        height_shift_range=0.2,
        rescale=1./255, #rescaling이 여기에 들어가 있다. 픽셀값을 0~1로 스케일링
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True,
        fill_mode='nearest')
```



#### 사용례1) flow_from_directory

directory에 train, val, test 데이터가 나뉘어서 저장되어있을 때 사용한다.

```python
generator = datagen.flow_from_directory(
        'train/train',
        target_size=target_size,
        batch_size=batch_size,
        class_mode=None,  #  라벨은 불러오지 않음
        shuffle=False)  
```



#### 사용례2) flow

이미 ipynb나 py파일에서 train, val, test 데이터가 변수로 선언되어있을 때 사용한다.

```python
generator2 = datagen.flow(X_val, y_val,
        batch_size=batch_size, 
        shuffle=False) 
```



#### ## 사용례3) flow _from_dataframe





#### 사용례) 병목 특징을 추출

```python
batch_size = 16
target_size = (227, 227)
generator = datagen.flow(X_train, y_train,
        batch_size=batch_size, 
        shuffle=False)  # 출력되는 병목 특징이 어디서 왔는지 알 수 있도록 입력 데이터의 순서를 유지합니다 

# 이미지를 모델에 입력시켜 결과를 가져옵니다. 본래 어떤 예측 결과가 출력되어야 하지만 모델의 일부만 가져왔기 때문에 병목 특징이 출력됩니다.
bottleneck_features_train = model.predict_generator(generator, 954)
# 출력된 병목 데이터를 저장합니다.
np.save(open('bottleneck_features_train.npy', 'wb'), bottleneck_features_train)
#원래 가져온 코드에서는 'w' 였는데 바이너리 파일을 wb로 저장해야 한다고 해서 변경
```



## 3. Modeling

```python
#sequential 모델
model = tf.keras.models.Sequential([
  #첫번째 convolution
  tf.keras.layers.Conv2D(64, (3,3), activation='relu', input_shape=(227, 227, 3)),
  tf.keras.layers.MaxPooling2D(2, 2),
   #여기에 batch norm을 넣어도 됨
    
  # 2번째 convolution
  tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2,2),
  # 3번째 convolution
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2,2),
  # 4번째 convolution
  tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
  tf.keras.layers.MaxPooling2D(2,2),
  # Flatten 해서 DNN에 넣음
  tf.keras.layers.Flatten(),
  tf.keras.layers.Dropout(0.5),
  # 512 neuron hidden layer
  tf.keras.layers.Dense(512, activation='relu'),
  tf.keras.layers.Dense(7, activation='softmax')
])


model.summary()

model.compile(loss = 'categorical_crossentropy', optimizer='rmsprop', metrics=['accuracy'])
```

```python
history = model.fit(train_generator, epochs=100, steps_per_epoch=50, validation_data = validation_generator, verbose = 1, validation_steps=3)

model.save("rps.h5")
```



## 4. Evaluation 

plot으로 훈련 셋과 검증 셋의 정확도를 볼 수 있다.

```python
import matplotlib.pyplot as plt
acc = history.history['accuracy']
val_acc = history.history['val_accuracy']
loss = history.history['loss']
val_loss = history.history['val_loss']

epochs = range(len(acc))

plt.plot(epochs, acc, 'r', label='Training accuracy')
plt.plot(epochs, val_acc, 'b', label='Validation accuracy')
plt.title('Training and validation accuracy')
plt.legend(loc=0)
plt.figure()
plt.show()
```

```python
from sklearn.metric import classification_report

```



## 5. Prediction

위에서 한 번도 사용되지 않은 테스트 데이터를 이용해서 예측해 본다.

```python
test_data = test_data/ 255
```

```python
yhat = model2.predict_classes(test_data) #predict_classes를 써야 클래스 예측. 그냥 predict하면 클래스별 확률이 나온다.
```

```python
#optional 데이터프레임으로 보기
yhat_df =  pd.DataFrame(yhat)
```

---



# 전이학습

