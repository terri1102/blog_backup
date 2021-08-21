---
layout: default
title: 한 권으로 읽는 컴퓨터 구조와 프로그래밍 9장
parent: 한 권으로 읽는 컴퓨터 구조와 프로그래밍
Grand_parent: Reviews
nav_order: 9
use_math: true

---





{:toc}

# 9장 웹 브라우저

추상적인 컴퓨터를 이해하기 위한 가상 머신



> 웹 브라우저는 그 자체로 가상머신(virtual machine)이다. 즉 웹브라우저는 복잡한 명령어 집합을 완전히 소프트웨어로만 구현한 추상적인 컴퓨터이다. 다시 말해, 인터프리터에 속한다.



## 마크업 언어

마크업(markup)은 본문(텍스트)과 구분할 수 있는 마크를 추가할 수 있는(annotation) 시스템이다.



**마크업 언어들의 역사** <br>

* GML : IBM에서 만든 Generalized Markup Language
* SGML(Standard Generalized Markup Language) : GML을 확장한 언어로 국제 표준화 기구(ISO)에서 채택됨
* XML(eXtensible Markup Language) : 확장 가능한 마크업 언어. 좀 더 실용적인 SGML의 하위 집합 
* HTML : SGML에서 나왔지만 SGML 표준을 더이상 준수하지 않음. 
* XHTML : HTML을 변형한 형태로 XML 규칙을 준수하도록 만들어진 언어



## 균일 자원 위치 지정자(URL, Uniform Resource Locator)

최초의 웹브라우저(WorldWideWeb)은 팀 버너스리 경에 의해 1990년에 만들어졌는데, URL을 사용해 HTTP 프로토콜을 통해 서버에 문서를 요청한다. 서버는 문서를 브라우저에 보내고, 브라우저는 문서를 표시한다. 

![browser_server]

**URL :** `https://www.nostrach.com/catalog/general-computing` <br>

​	      `	스킴		호스트	        경로`

* 스킴(scheme) : 통신 매커니즘 표시. https는 안전한(암호화를 사용하는) 하이퍼 텍스트 전송 프로토콜 의미. file을 사용하면 로컬에 저장된 파일을 가리킬 수 있음

* 호스트(host) : 보통은 도메인 이름. 아니면 숫자로 된 인터넷 주소

* 경로: 문서가 추출되는 위치



## HTML 문서

HTML은 Hypertext를 활용. 웹 페이지 등 다른  대상에 대한 링크가 들어 있는 텍스트를 말함.

HTML에서 부등호(<)는 **마크업 엘리먼트(element)**를 시작한다. 이 \<tag> 등의 엘리먼트들은 서로 쌍을 등장한다.





## DOM: 문서 객체 모델



### 트리 관련 용어



### DOM 처리



## CSS



## XML 등의 마크업 언어



## 자바스크립트



## jQuery



## 
