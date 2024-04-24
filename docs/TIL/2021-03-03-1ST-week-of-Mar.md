---
layout: default
title: 2021년 3월 첫째주 TIL
date: 2021-03-02
categories:
  - TIL
tags:
  - TIL
comments: true
nav_order: 6
parent: TIL
---



## 2021.03.02(화)

**Fact:** 한 달 배운 것 정리 & 블로그 공사

**Feeling:** 블로그...:sassy_woman:

**Finding:** 동기분께서 공유해준 코드에 모르는 부분 공부했다.

```python
#zfill: zeros fill, zfill의 attribute가 숫자의 자릿수보다 모자라면 숫자 앞에 0 붙여줌
"2".zfill(3)
#>>"002"
"300".zfill(3)
#>>"300"

#ffill
#fillna(method='ffill')과 동일
#forward propagation
#bfill과 반대

#rjust: 앞에 붙여줄 숫자/문자 지정 가능
"2".rjust(3,"0")
#>>"002"
"123".rjust(5,"a")
#>>"aa123"
```

**Future Action:** 블로그 공사...더 늦기 전에 테마 바꾸기

**Food for thought:** :shallow_pan_of_food: 이번에 새로 설치한 프로그램들 언제 쓰는 건지 생각해보기



---

## 2021.03.03(수)

**Fact:** 오늘도 블로그 정리...:construction_worker_woman:

**Feeling:** :expressionless:

**Finding:** 

블로그 파일 push하려는데 원격저장소에 있는 파일이 로컬에 없다면서 에러가 나서, git force push를 처음 써봤다. 나중에 협업할 때는 이런식으로 쓰면 안 된다는데...그건 그때 가서 알아봐야지^^

```python
git push -f origin master
```



Git 에러 CRLF will be replaced by LF

맥 또는 리눅스를 쓰는 개발자와 윈도우를 쓰는 개발자가 협업할 때 white space 때문에 나는 오류

이 테마를 만든 사람이 맥이나 리눅스를 썼나 보다.

```python
git config --global core.autocrlf true #이 프로젝트에서만 쓸 거면 global 빼기
```

바꿨는데도 여전히 서버 구동이 안 되네ㅠㅠㅠ

```python
 (C:/Users/Boyoon Jang/Desktop/Repository/blog_backup/_config.yml): did not find expected key while parsing a block mapping at line 16 column 1
```

https://ychae-leah.tistory.com/14

yml이 아니라 yaml 문제였네

이전 테마에선 yaml 제목에 특수문자 쓰는 거 가능했는데 여긴 안 되나 보다.

**Future Action:** 

**Food for thought: :pancakes:**