---
layout: default
title: 
date: 2021-03-20
categories:
  - Web
tags:
  - Web_Development
comments: true
nav_order: 6
parent: Web Development
---



django와 달리 flask는 좀더 유연

django는 이미 만들어진 틀이 있어서 개발이 더 쉬운듯



스크래이핑할 때 http, restapi



과제

트위터 api를 써서 db에 저장->분석->사용자에게 보여줌



flask: 웹 프레임워크

배울 내용

flask-sqlalchemy 사용

flask-migration

데이터베이스 관리

flask 구조 작성



---

RESTful API 설명 가능해야 한다. 이게 좀 표준

데이터베이스에 저장...



https는 http보다 보안을 강조한 버전. 다른 사람이 데이터 전송 패킷을 볼 수 없게 해줌.

http로 들어가면 자물쇠 뜨기도 함. 인증 안 된 사이트



https처럼 s 붙으려면 조건을 맞춰서 인증을 해야함

우리는 http로 만들 것 

HTTP(Hyper Text Transfer Protocol)

통신규약: 컴퓨터끼리 소통(데이터 전송)을 하기위한 공통규약



그 외 규약들: 이메일 pop3, SMTP



멱동성?

**JSON**: JavaScript Object Notation

자바스크립트란 언어는 웹을 위해서 만들어진 언어

DOM 등에 편리하게 접근할 수 있음

Object는 파이썬의 딕셔너리와 비슷한 문법가짐

Notion: 표기법

Json을 많이 쓰는 이유는 웹에서 주로 자바스크립트 많이 써서 그렇다



### HTTP 요청(request)

지난 크롤링 시간에 requests get 사용했었음. 브라우저에서 주로 사용하는것이 get

요청: HTTP Request

응답: HTTP Response



### HTTP 요청 메서드

**GET**: 데이터 받음. 보통 HTML, CSS, JS 같이 옴

 json 형식 못 인식함

ex) url 주소(숫자, 문자열 조회)



**POST**: 데이터 추가, 데이터베이스 상태 변경

json 형식으로 보낼 수 있다.

ex)회원가입(새 유저 추가)



**PUT**: 업데이트, 모든 걸 다 바꿀 때 



**PATCH**: 업데이트, 일부 수정할 때

서버에서 허락을 해줘야 하는 애



**DELETE**: 데이터 지울 때 사용, json 형태로 보낼 수 있다

```json
{"username":'spongebob'}
```



예시

```json
http://spondeland.com/api/user?username=1
http://spondeland.com/api/user?username=sponge
http://spondeland.com/api/user{"username":"spongebob"}
```



**OPTIONS:** 우리가 아닌 브라우저가 사용. 브라우저에서 사용자의 요청을 서버에 미리 확인 후 응답



1. 사용자가 patch 요청을 보냄
2. 브라우저가 서버에 가능한지 options 물어봄
3. 서버가 status code 보냄 -200 
4. patch 가능?
5. 서버가 응답



OPTIONS로 미리 확인하는 이유

프리플라이트 요청??<> 

simple request(조건을 충족한 요청)

1)효율 위한 것. 기다리는 응답속도 줄어듦

2)CORS 보안과 관련된 것



*CORS Cross-origin requests

api는 서버 하나로 이루어지지 않음. 



대부분의 현대 브라우저는 OPTIONS 사용함

MDN 많이 찾아보기



서버 측에서 OPTIONS가 들어올 때 어떻게 해야하는지 로직을 구성해야함(서버 디자인할 때 설정)

브라우저에서 보내는 입장에서는 상관없다.



개발자도구에서 network 들어가면 네트워크 관련 정보가 뜸 ->새로고침하기

methods

Request URL: dfdfd

Request Method: GET

Status Code:

Remote Address: 현재 사이트의 주소



language.svg 누르면 언어번역 도구 아이콘 그림 뜸



깃헙

github.com/session

Post

Form Data

토큰

가끔가다가 options 가 뜨는데, 실제 요청을 보내기 전에 브라우저가 먼저 서버에 확인하는 것 





사용자가 문자열 주소 검색

DNS가 주소 찾음

컴퓨터가 쓰는 주소 ipv4, ipv6



내 로컬 컴퓨터 주소: 127.0,0.1

---

## 응답

MDN 사이트에 있음

100번대에서 500번대가 나뉘어져 있음

100: 정보응답

200: 성공응답 

300:redirection 우리 옮겼다

400: 클라이언트 에러 응답 (서버가 요청을 이해할 수 없음 ). 니가 잘못했다.

ex) 404 :not found, 403: Forbidden

500: 서버의 에러. 우리가 잘못했다



웹 어플리케이션, api 어플리케이션 등 어플리케이션 종류 나뉨



http://spongland.com/api/user

요청방법

get http://spongland.com/api/user/1

post {"username": "spongebob"}

put {"username" : "patrick"}

delete {"username" : "patrick"} http://spongland.com/api/user/1



보통 유저 추가할 때 주소처럼 안 쓰고 유저 하나에 매서드 활용



POSTMAN은 요청을 해주는 프로그램



```python
import requests
requests 라이브러리에서 메서드 검색해서 쓰면 됨
response = requests.get('api key')
response.status_code
response.json() #실제값
response.json #상태 알려줌
json_data = response.json()  
json_data['eather']['description'] #여기서 오류남...왜 그럴까
```



restful 검사-ibm에서 제공?

Gherkin

Batch operations

django 

![rest_api]