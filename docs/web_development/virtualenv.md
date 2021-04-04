---
layout: default
title: "가상환경과 아나콘다, pipenv"
parent: Web Development
nav_order: 1
tag: -가상환경
     -아나콘다
     -pipenv
---

# 가상환경

## 가상환경을 사용하는 이유
작업별로 가상환경을 구성하면 패키지 관리가 편해서! 프로젝트별로 사용하는 패키지와 패키지의 버전이 다른 경우가 많기 때문에 이렇게 묶어서 하나의 가상환경으로 만들어 놓으면 그 가상환경을 활성화만 시키면 되기 때문에 편리하다.

colab 같은 경우는 파일마다 pip install로 패키지를 깔아서 사용하면 되는데, 파일을 열 때마다 다시 패키지를 깔아야 해서 번거로움이 있었다. 하지만, jupyter notebook이나 파이썬 스크립트로 작업할 때는 환경을 한번 구성해놓으면 나중에 가져와서 쓰면 되니까 작업별로 환경을 다르게 구성해서 쓸 수 있는 장점이 있다.

가상환경 당 패키지는 한 가지 버전만 설치 가능하며 프로젝트별로 가상환경 하나씩 만들어서 사용하는 습관 들이는 것이 좋다고 한다.

사족이지만, jupyter가 julia + python + r인 것을 최근에 알았다ㅎ colab에서도 r 작업이 가능해서 가끔 쓰고 있다. 


### 패키지 버전 확인
```python
import pandas as pd

pd.__version__
```

<br>
---

# 가상환경 구축

## 1. 아나콘다로 가상환경 설정
### 1) 일단 가상환경을 만들기(anaconda prompt나 bash 셸에서 진행)
```bash
$ conda create -n new_env
$ conda create --name new_env

#파이썬 버전 명시
$ conda create -n new_env python=3.8 #3.8번 대 파이썬 깔림. 내 컴퓨터에서는 보통 3.8.8 깔림

#정확한 파이썬 버전 명시
$ conda create -n new_env python==3.8.0 #3.8.0 파이썬 깔림
```
주의점: 현재 있는 가상환경을 다 비활성화 시키고 만들기. 가상환경 활성화된 상태에서 가상환경 또 만들면 켜져 있던 가상환경의 영향을 좀 받게 되는 것 같다.
```bash
#가상환경 비활성화
$ conda deactivate
```
### 2) 가상환경 활성화하기
```bash
$ conda activate new_env
```

### 3) 가상환경에 패키지 설치
```bash
$ conda install pandas

$ pip install sqlalchemy==1.4.4
```
### 4) 가상환경 삭제
```bash
$ conda env remove -n new_env
```

### 가상환경 리스트 보기
```bash
$ conda env list
```
안 지우고 계속 가상환경 만들었더니 벌써 가상환경이 20개가 넘었다. 이번 섹션 마무리하고 다 지워야지

### 가상환경에 깔린 패키지 리스트 보기
```bash
$ pip list
```

### 가상환경에 깔린 패키지 통째로 저장하기
```bash
$ pip freeze > requirements.txt
```

### requirements.txt 파일을 통해 패키지 깔기
```bash
$ python -m pip install -r requirements.txt

# -m: 모듈 실행
# -r: 읽어와라
#앞에 python -m은 생략가능
```

<br>

## 2. pipenv로 가상환경 설정
### pipenv의 장점
- pip와 virtualenv를 따로 쓸 필요가 없다. 동시에 사용이 된다.
- Pipenv는 Pipfile와 Pipfile.lock을 requirements.txt를 대신하여 사용한다.
- 해쉬가 자동생성된다. (보안)
- 의존성 그래프를 제공함으로서 insight를 제공한다 (e.g. `$ pipenv graph`).
- .env 파일들을 사용한 스트림라인 개발 워크플로우

- 락이 걸린 의존성에 대해 해쉬 파일을 생성하고 확인한다.
- pyenv가 사용 가능하다면, 필요한 python도 자동으로 설치한다.
- Pipfile을 찾으면서 자동으로 프로젝트 홈을 찾아준다.
- Pipfile이 없다면 자동으로 생성해준다.
- 자동으로 virtualenv 환경을 생성한다.
- 패키지를 설치/삭제하면, 자동으로 Pipfile에서 추가/삭제한다.
- 자동으로 .env 파일을 인식한다.
  

출처: https://medium.com/@erish/python-pipenv-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-961b00d4f42f

conda 가상환경과 차이는 conda로 가상환경을 만들면 경로가 anaconda3>envs>new_env 이런식으로 anaconda 디렉토리 아래로 들어가는데, (설정을 통해서 위치를 바꿀 수 있긴 하다.) pipenv는 가상환경을 만드는 폴더 안에서 가상환경이 생성된다. 즉, 프로젝트 폴더 안에 가상환경 폴더가 생성된다.


### pipenv 설치
```bash
$ pip install pipenv
```

### 설치 확인
```bash
$ pipenv
```

### 여러 명령어
```bash
#Usage Examples:
   #Create a new project using Python 3.7, specifically:
   $ pipenv --python 3.7

   #Remove project virtualenv (inferred from current directory):
   $ pipenv --rm

   #Install all dependencies for a project (including dev):
   $ pipenv install --dev

   #Create a lockfile containing pre-releases:
   $ pipenv lock --pre

   #Show a graph of your installed dependencies:
   $ pipenv graph

   #Check your installed dependencies for security vulnerabilities:
   $ pipenv check

   #Install a local setup.py into your virtual environment/Pipfile:
   $ pipenv install -e .

   #Use a lower-level pip command:
   $ pipenv run pip freeze
```
가상환경 비활성화

```
$exit
```

패키지 설치

```bash
$ pipenv --python 3.8.5
```

패키지 설치

```bash
$ pipenv install numpy

$ pipenv install "numpy<=1.18.1"
```


lock 파일 생성

```python
pipenv lock

#pipfile이 없는 상태에서 requirements.txt가 있는 폴더에서 pipenv lock을 하면 패키지가 깔리면서 pipfile과 piplock 파일 둘 다 생긴다.
```


## 그 외
visual studio code로 작업하고 있는데, 가상환경을 activate 한 후에 which python을 해서 보면 분명 가상환경이 활성화 되어 있는데 interpreter는 변경이 안 되어 있다. 내 컴퓨터 문제인지...원래 이런건지 모르겠지만, 항상 매뉴얼하게 바꿔주고 있다. 보통 인터프리터가 내 가상환경과 달라도 파이썬 버전이 같으면 큰 문제는 없는 것 같지만 import 오류가 날 때가 있으니 interpreter를 꼭 바꿔주자.