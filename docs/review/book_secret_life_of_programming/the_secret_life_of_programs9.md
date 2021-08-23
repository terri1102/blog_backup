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

![parse_tree](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/parsetree.jpg?raw=true)

브라우저는 루트에서 시작해서 첫 번째 자식으로 내려가고, 그 자식의 첫 번째 자식으로 내려간다. 이런 식으로 종단 노드에 도달할 때까지 순회를 계속한다. 

여기서 방문하는 순서는 HTML을 작성한 순서에 따르며, 깊이 우선 순회는 스택을 사용한다.



## CSS(Cascading Style Sheets)

CSS는 HTML에서 스타일 정보를 분리해서 HTML을 한 번만 작성해도 대상 장치에 따라 여러 스타일을 적용할 수 있게 했다. 

CSS가 셀렉터라고 하는 정규식의 변형을 사용해 DOM의 엘리먼트 위치를 지정하므로 구성요소를 어떻게 수성하느냐가 중요하다.

CSS는 색, 글꼴 크기 등의 많은 프로퍼티(property)를 정의하며, DOM 엘리먼트와 결합된 프로퍼티를 애트리뷰트라고 부른다. 



CSS가 포함된 html

```HTML
<html>
    <head>
        <title>My first web page</title>
         <!-- css 추가된 영역 -->
        <style>                    
            body {
                color: blue;
            }
            big {
                color: yellow;
                font-soze: 200%;
            }
        </style>
        <!-- css 추가된 영역 -->
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

body와 big 엘리먼트를 선택해서 각 셀렉터(body, big) 뒤에는 프로퍼티 이름(color)과 값(blue)의 목록이 들어있다. 이름과 값은 콜론으로 구분하고, 값 뒤에는 세미콜론이 들어가며, 셀렉터에 대한 프로퍼티 목록은 중괄호 안에 들어 있다. 



## XML 등의 마크업 언어

HTML과의 차이는 XML은 일반적인 용도의 마크업 언어로 다양한 응용 분야가 있고, HTML은 웹페이지라는 구체적인 응용을 위해 만들어진다는 점이다. 

XML은 엘리먼트 이름을 원하는대로 정할 수 있는데, 다른 언어에서 같은 이름을 사용하는 이름 충돌이 일어날 수 있다. 이때 이 엘리먼트가 어느 언어(파일)에 속하는지 구분할 수 있게 이름 공간(name space)를 사용해 엘리먼트 태그 앞에 접두사를 붙인다. 

**이름공간** 또는 **네임스페이스**(Namespace)는 개체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 개체만을 가리키게 된다.

XML 문서로부터 파스 트리를 만들어주는 라이브러리도 많음 : DTD, Xpath, XSLT 등.. 나는 elementTree 쓰는데..

**DTD(문서타입정의, Document Type Definition)** 는 메타 마크업 언어로, 마크업 언어 문법에 맞는 엘리먼트가 어떻게 생겼는지 정의할 수 있다.

XML 경로 언어(Xpath)은 XML 문서에 대한 셀렉터를 제공한다. Xpath는 XSLT의 중요한 부분이다. XSLT(Xtensible Sytlesheet Language Transformation)와 Xpath를 결합하면 XML 문서를 표현한 파스 트리를 검색하고 변형시킴으로써 XML 문서의 일부분을 다른 형태로 변환하는 XML 문서 조각을 작성할 수 있다.  



## 자바스크립트

자바스크립트를 사용하면 서버가 아니라 (브라우저가 실행 중인) 실제 프로그램을을 웹 페이지에 포함시킬 수 있다. 

![slp11]

자바스크립트와 서버의 상호작용은 AJAX(비동기 자바스크립트와 XML)를 통해 이루어진다. 

**비동기:** 브라우저가 서버의 응답이 언제 일어날지에 대해(일어날지의 여부도) 아무 제어를 하지 않는다는 뜻이다. 브라우저 자바스크립트 프로그램이 서버에게 비동기로 XML형식의 데이터를 보낸다.



\<script> 엘리먼트 안에 자바스크립트 포함한 코드

```html
<html>
    <head>
        <title>My first web page</title>
        <style>                    
            body {
                color: blue;
            }
            big {
                color: yellow;
                font-soze: 200%;
            }
        </style>
        <!-- javascript 추가된 영역 -->
        <script>
        	window.onloade = function() {
                var big = document.getElementsByTagName('big');
                big[0].style.background = "green";
            }
        </script>
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



## jQuery

위의 DOM 함수의 문제는 1) DOM 함수 동작이 브라우저마다 다를 수 있다는 것과 2) DOM 함수를 사용하기가 불편하다는 것이다. jQuery를 통해 이 두 문제를 해결할 수 있다. jQuery를 통해서 엘리먼트가 클릭될 때 실행될 이벤트 핸들러를 연결할 수 있다. 근데 최근에 jQuery가 많이 안 쓰이는 것 같다.



## SVG(Scalable Vector Graphics)

SVG는 크기 변경이 가능한 백터 그래픽스로 브라우저가 지원하는 마크업 언어 중 하나이지만 다른 마크업 언어와 다르다. 

```html
<br>
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="400"?
     <circle id="c" r="10" cx="200" cy="200" fill="red"/>
	 <animate xlink:href="#c" attributeName="r" from="10" to="200" dur="5s"
repeatCount="indefinite"/>
</svg>
```



## HTML5

html5는 여러 시맨틱 엘리먼트들이 추가되었다. 또한 캔버스(canvas)가 추가되었는데 SVG와 비슷한 기능을 제공하지만 완전 다른 방식으로 제공한다.



## JSON(JavaScript Object Notation)

Json은 자바스크립트 객체를 사람이 읽기 쉬운방식으로 표현한 것이다. 

Json의 장점

1. 자바스크립트 객체를 Json 형식으로 쉽게 바꿀 수 있음
2. 자바스크립트를 사용할 때 XML보다 편리: 자바스크립트의 eval함수는 데이터인 Json을 자바스크립트 프로그램인 것처럼 직접 실행 가능. 따라서 Json을 사용하면 자바스크립트에서 데이터를 내보내고 들여올 때 추가로 코드 작성할 필요 없음





## 정리

![browser](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/browser.jpg?raw=true)

