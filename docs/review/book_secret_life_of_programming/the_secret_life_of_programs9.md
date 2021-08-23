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

![browser_server](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/browser_server.jpg?raw=true)

**URL :** `https://www.nostrach.com/catalog/general-computing` <br>

​	      `	스킴		호스트	        경로`

* 스킴(scheme) : 통신 매커니즘 표시. https는 안전한(암호화를 사용하는) 하이퍼 텍스트 전송 프로토콜 의미. file을 사용하면 로컬에 저장된 파일을 가리킬 수 있음

* 호스트(host) : 보통은 도메인 이름. 아니면 숫자로 된 인터넷 주소

* 경로: 문서가 추출되는 위치



## HTML 문서

HTML은 Hypertext를 활용. 웹 페이지 등 다른  대상에 대한 링크가 들어 있는 텍스트를 말함.

HTML에서 부등호(<)는 **마크업 엘리먼트(element)**를 시작한다. 이 \<tag> 등의 엘리먼트들은 서로 쌍을 등장한다.

**엔티티 참조(entity reference)** : 문자를 다른 문자열로 표현. ex) `&lt;`=='<'

**애트리뷰트(attribute) **: 이름과 값을 =로 연결한 쌍. 애트리뷰트 이름에 따라 미리 정의된 동작이 있는 경우도 있음.(hidden..등)

```html
<tag name1="value" name2="value2">
	엘리먼트 내용
</tag>
```



아래의 html문서를 예시로 DOM 설명을 할 것이다.

```html
<html>
    <head>
        <title>My first web page
        </title>
    </head>
    <body>
        This is my first web page.
        <b>
        	<big>
            cool!
            </big>
        </b>
    </body>
</html>
```





## DOM: 문서 객체 모델

DOM(Document Object Model): 웹 브라우저는 문서를 DOM에 의해 처리한다.  이는 일련의 엘리먼트들이 다른 엘리먼트들로 둘러싸고 잇는 것으로 생각할 수 있다. 이는 DAG이기도 하고 트리 구조이기도 하다. 



HTML 문서를 트리 구조로 생각한 모습

![dom](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/dom.jpg?raw=true)



### 트리 관련 용어

| 용어                    | 의미                                                         | 예제                          |
| ----------------------- | ------------------------------------------------------------ | ----------------------------- |
| 노드(node)              | 트리를 이루는 엘리먼트들                                     | html, head, body              |
| 내부(interior) 노드     | 자신에게서 나가는 화살표가 존재하는 노드                     | title                         |
| 종단(terminal) 노드     | 자신에게서 나가는 화살표가 없는 노드 (leaf node)             | Cool!                         |
| 루트(root)              | 트리의 최상위 노드                                           | html                          |
| 부모(parent)            | 두 노드를 연결하는 화살표가 있는 경우 화살표의 출발점에 있는 노드 | html은 head와 body의 부모     |
| 자식(child)             | 두 노드를 연결하는 화살표가 있는  경우 화살표의 머리(촉)에 있는 노드 | head와 body는 html의 자식임   |
| 후손(descendant)        | 직간접적으로 다른 노드의 자식이거나 자식의 자식 등의 관계가 있는 노드 | title의 html의 후손(자손)노드 |
| 선조(ancestor)          | 직간접적으로 다른 노드의 부모이거나 부모의 부모, 부모의 부모 등의 관계가 있는 노드 | body는 big의 후손             |
| 형제 또는 자매(sibling) | 한 부모의 자식인 노드 간의 관계                              | head는 body의 자매            |



### DOM 처리

깊이 우선 순회를 하면서 트리를 해석함.



## CSS



## XML 등의 마크업 언어



## 자바스크립트



## jQuery



## 
