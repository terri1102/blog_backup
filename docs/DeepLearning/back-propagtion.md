---
layout: default
title: 역전파
date: 2021-04-17
parent: Deep Learning
comments: true
last_modified_date: true
nav_order: 3
---

t

역전파 개념을 확실하게 이해하고 싶어서 ppt로 만들어 보았는데도 아직도 어려운 거 같다.

![bp1](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C2.JPG?raw=true)

![bp2](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C3.JPG?raw=true)

![bp3](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C4.JPG?raw=true)

순전파가 일어날 때 가중치는 랜덤하게 정해지기 때문에 처음에는 모델은 제대로 된 분류를 하지 못한다. 강아지 사진이 들어가 있긴 하지만 노드가 2개인 예시이기 때문에 사진이 아닌 특성을 넣었다고 가정합니다.

![bp4](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C5.JPG?raw=true)

이제 W31의 가중치가 역전파를 통해서 업데이트되는 과정을 설명하겠습니다.

![bp5](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C6.JPG?raw=true)

![bp6](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C7.JPG?raw=true)

![bp7](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C8.JPG?raw=true)

![bp8](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C9.JPG?raw=true)

![bp9](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C10.JPG?raw=true)

![bp10](https://github.com/terri1102/blog_backup/blob/master/assets/images/dl/Bp/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C11.JPG?raw=true)



