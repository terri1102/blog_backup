---
layout: default
title: 한 권으로 읽는 컴퓨터 구조와 프로그래밍 10장
parent: 한 권으로 읽는 컴퓨터 구조와 프로그래밍
Grand_parent: Reviews
nav_order: 10
use_math: true

---



<img src="https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/review/slp1.jpg?raw=true" class="center" alt="slp1" style="zoom:33%;" />

{:toc}

# 10장 애플리케이션 프로그래밍과 시스템 프로그래밍 

고수준 언어와 저수준 언어 프로그래밍 방식 비교

![slp10](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/slp10.jpg?raw=true)

운영체제는 사용자 프로그램이 I/O 장치의 복잡도를 상당 부분 볼 수 없게 가려준다. 비슷한 방식으로 브라우저 같은 복잡한 사용자 프로그램은 그 프로그램 위에 만들어진 다른 애플리케이션 프로그램들이 운영체제를 다루는 복잡도를 볼 수 없게 가려준다. -->자바스크립트 <->C



## :dog: 동물 추측 프로그램 버전 1: HTML과 자바스크립트 프로그램



DOM은 2진 트리와 마찬가지로 DAG(유향 비순환 그래프)의 하위 집합인 `트리`다. 그래서 지식을 저장하는(지식 기반, knowledge base) 2진 트리를 눈에 보이지 않는 \<div> 를 사용해 DOM으로 구성할 것이다.



### 애플리케이션 수준의 뼈대

```html
<html lang="ko">
    <head>
        <!-- jQuery 포함시키기 -->
        <script type="text/javascript" src="http://code.jquery.com/jquery-3.1.1.min.js"></script>
        
        <title>동물 추측 게임</title>
        
        <style>
        	<!-- CSS를 여기 넣는다 -->
        </style>
        
        <script type="text/javascript">
        <!-- javascript를 여기 넣는다 -->
        
         $(function() {
            <!-- 문서가 준비되면 실행될 자바스크립트 코드 -->
        })
        </script>
    </head>
    <body>
        <!-- HTML을 여기 넣는다 -->
    </body>
</html>
```



### 웹 페이지 본문

위의 뼈대의 <!-- HTML을 여기 넣는다 --> 부분을 대신 함

```html
<!-- 이 부분은 지식 트리이며 결코 표시되지 않는다 -->

<div id="root" class="invisible">
    <div string="동물이 짖습니까?">
        <div string="개"></div>
        <div string="고양이"></div>
    </div>
</div>

<div id="dialog">
    <!-- 대화가 여기 들어간다 -->
</div>

<!-- 새로운 동물 이름을 얻는 대화 -->

<div id="what-is-it" class="start-hidden">
    <input id="what" type="text"/>
    <button id="done-what">이름 입력 완료</button>
</div>

<!-- 새로운 동물에 대한 질문을 얻는 대화 -->

<div id="new-question" class="start-hidden">
    <span id="new"></span>와(과) <span id="old"></span>을(를) 구분하기 위해 던질 수 있는 질문이 무엇입니까? 답은 "예"여야 합니다.
    <input id="question" type="text"/>
    <button id="done-question">답 입력 완료</button>
</div>


<!-- "예"와 "아니요" 버튼 -->

<div id="yesno" class="start-hidden">
    <button id="yes">예</button>
    <button id="no">아니요</button>
</div>
```



### 자바 스크립트 

node 변수를 먼저 선언한다.

```javascript
var node; // 지식 트리상의 현재 위치
// 제공받은 html을 대화에 추가한다. 새 노드에 자신이 없다면
// 더 이상 질문할 내용이 없으므로 끝난다. 새 노드에 자식이 있다면,
//새 노드를 현재 노드의 자식으로 만들고 노드의 string 애트리뷰트를 사용해
// 질문을 던진다. 노드가 잎 노드이면 질문으로 동물 이름을 던진다.
// 새 노드가 잎 노드이면 true를 반환한다.

function
question(new_node, html)
{
    $('#dialog').append(html); //html을 대화에 추가함
    
    if ($(new_node).length == 0) { //자식이 없으면 질문도 없음
        return (true);
    }
    else {
        node =new_node; //새 노드로 내려가기
        
        if ($(node).children().length == 0)
            $('dialog').append($(node).attr('string') + '입니까?');
        else
            $('dialog').append($(node).attr('string') + '?');
        
        return (false);
    }
}

// 게임을 다시 시작함. 모든 버튼과 텍스트 필드를 감추고, 텍스트 필드를
// 지우고, 초기 노드와 인삿말을 설정한 다음, 첫 번째 질문을 던지고,
// 예, 아니요 버튼을 표시한다.

function
restart()
{
    $('.start-hidden').hide();
    $('#question,#what').val('');
    question($('#root>div'),'<div><b>동물을 생각해보세요.</b></div>');
    $('#yesno').show();
}
```

<!-- 문서가 준비되면 실행될 자바스크립트 코드 --> 부분을 아래의 다섯 가지 요소로 바꾼다.

```javascript
restart(); //처음에 모든 설정을 다시 한다.

//플레이어가 새 질문을 입력했다. 이 질문이 들어 있는 노드를 만들고
//예전의 '아니요'에 해당하는 노드를 새 노드 안에 넣는다. 그 후
// 새 동물이 들어 있는 노드를 만들고 이 노드를 '아니요'에 해당하는 노드 바로 앞에
// 넣어서 '예'에 해당하는 노드가 되게 만든다. 그 후 처음부터 다시 시작한다.

$('#done-question').click(function() {
    $(node).wrap('<div string="'+ $(#))
})

```



### CSS

클래스를 사용하면서 엘리먼트에 레이블을 붙여서 여러 엘리먼트를 쉽게 선택 후 관리할 수 있게 해준다.

```css
invisible {
	display: none; /*이 클래스에 속하는 엘리먼트는 표시되지 않음 */
}
```



## 동물 추측 게임 버전 2: C 프로그램

위의 자바스크립트로 작성한 것과 다르게 시스템 프로그래밍은 문자열 관리, 메모리 관리, 버퍼 관리 등을 일일이 설정해줘야 한다. 



### 프로그램 빌드

C는 컴파일 언어이기 때문에 자바스크립트를 인터프리터로 실행한 것처럼 그냥 소스코드를 실행할 수 없고, 컴파일해야 한다. 

```c
cc gta.c -o gta
//c컴파일러 소스파일이름 -o 출력파일이름
```

컴파일 이후에는 출력 파일 이름을 터미널에서 입력하면 실행된다.



### 터미널과 장치 드라이버

터미널은 I/O장치이며, 사용자 프로그램은 직접 I/O 장치와 통신하지 않고 운영체제가 중간에서 통신을 중재한다. 터미널과 컴퓨터는 소프트웨어를 통해 마치 직렬 연결된 것처럼 연결되어 있다.

![slp12](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/slp12.jpg?raw=true)



### 문맥 전환(context switch)

> 장치 드라이버는 복잡하다. 복잡한 주된 이유는 운영체제가 한 번에 사용자 프로그램을 하나 이상 실행할 수 있기 때문이다. 컴퓨터는 레지스터 집합이 하나뿐이므로 운영체제는 한 사용자 프로그램을 다른 사용자 프로그램으로 바꿀 때마다 레지스터를 저장하고 복구해야 한다. 

**프로세스 문맥(process context) :** 레지스터처럼 저장하고 복구해야 하는 모든 내용

문맥의 크기가 커서 문맥 전환 비용이 크기 때문에, 문맥 전환을 최소화하는 것이 필요하다.

\<시스템 콜이 이뤄지는 과정>

![slp13](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/slp13.jpg?raw=true)



문맥 전환이 필요하지 않은 경우 문맥 전환이 일어나지 않게 OS는 사용자 프로그램을 슬립시킨다. (예: ENTER를 누르기 전까지의 키보드 입력) 사용자 프로그램은 시스템 콜을 사용해 터미널에서 입력을 읽고 싶다는 의사를 표시한다. (키 입력이 없기 때문에) 사용자 프로그램은 슬립 상태가 된다. 그 결과 OS는 또 다른 프로그램을 실행하는 등의 다른 동작을 수행할 수 있다.

물리적 장치와 관련한 처리를 담당하는 장치 드라이버(device driver)는 터미널에서 들어오는 문자를 **버퍼(buffer)**에 저장하고, 사용자가 키를 누를 때가 아니라 ENTER를 누를 때만 사용자 프로그램을 **깨운다**. 



**버퍼**

소프트웨어 영역: 선입선출의 데이터 구조. 컴퓨팅에서, 버퍼는 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 메모리의 영역이다. 버퍼링이란 버퍼를 활용하는 방식 또는 버퍼를 채우는 동작을 말한다. 다른 말로 '큐'라고도 표현한다

하드웨어 영역: 연약한 구성요소를 보호하기위해 사용하는 회로

> 터미널에서 키보드와 디스플레이 사이에는 직접적인 연결이 없다. 키보드는 컴퓨터에 데이터를 보내고, 디스플레이는 컴퓨터에서 데이터를 받는다. 원래는 키보드와 디스플레이를 위한 선이 따로 있었지만, 전이중 모드를 사용하면 선 하나로 양방향 통신을 동시에 진행할 수 있다. 사용자가 누른 키가 무엇인지 혼동할 수 있으므로 키 입력에 대한 버퍼를 사용하는 것만으로는 충분하지 않고, 사용자 입력을 즉시 화면에 표시하는 **에코(Echo)**가 필요하다. 그리고 사용자 키 입력은 프로그램이 디스플레이에 보내는 출력보다 느리기 때문에 입력 버퍼(input buffer) 외에도 출력 버퍼(output buffer) 필요하다. 출력 버퍼가 꽉 찼는데도 프로그램이 터미널에 출력을 시도하면 프로그램을 슬립 상태로 전환한다. 입력 버퍼가 꽉 찬 경우에는 드라이버가 사용자에게 화면을 깜빡이거나 삐 소리를 울리는 등의 피드백을 제공할 수도 있다.



\<터미널 장치 드라이버의 버퍼링과 메아리>

![slp14](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/slp14.jpg?raw=true)

실제 장치 드라이버는 훨씬 복잡하며, 에코나 버퍼링을 끄거나 켤 수 있다. 버퍼링이 꺼져 있는 경우를 로 모드(raw mode)라고 부르고, 버퍼링이 켜져 있는 경우를 쿡드 모드(cooked mode)라고 한다.



### 표준 I/O

표준 입력/출력 라이브러리가 만들어진 이유: 장치 드라이버가 입력을 버퍼링해도 사용자 프로그램이 사용자가 문자를 입력할 때마다 시스템 콜을 호출하면 장치 드라이버 쪽 입력 버퍼가 아무 쓸모가 없다. 마찬가지로 사용자 프로그램이 문자를 기록하기 위해 매번 시스템 콜을 호출하면 장치 드라이버에 출력 버퍼가 아무 역할도 못한다. 이런 상황이 자주 있기에 표준 I/O 라이브러리(stdio)는 사용자 프로그램이 쓸 수 있는 버퍼 I/O함수가 들어있다.

> stdio 라이브러리는 버퍼 입력을 지원한다. 버퍼 입력을 사용하면 시스템 콜을 한 번만 사용해 장치 드라이버에서 읽은 데이터를 버퍼에 넣을 수 있다. 사용자 프로그램은 버퍼가 빌 때까지 버퍼에서 입력 문자를 얻는다. 버퍼가 소진되면 다시 시스템 콜을 통해 입력을 더 가져온다. 출력하는 경우에는 버퍼가 꽉 차거나 중요한 문자(newline 등)가 출력될 때까지 문자가 버퍼에 들어간다.

[stdio 버퍼를 사용하는 사용자 프로그램]

![slp15](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/slp15.jpg?raw=true)



**사용자 프로그램을 터미널 장치 드라이버와 연결**

open 시스템 콜은 파일 이름을 파일을 참조할 수 있는 핸들(handle)이나 파일 디스크립터(file descriptor)로 바꿔준다. 이 핸들을 close 시스템 콜로 닫으면 더 이상 핸들을 사용할 수 없다. `핸들은 마치 박물관에 들어가면서 가방을 맡기고 받는 보관증과 비슷하다.` stdio는 fopen과 fclose라는 함수를 제공한다. 이 두 함수는 open과 close 시스템 콜을 사용하되, 파일을 열어서 핸들을 얻을 때 버퍼를 설정하고 파일 핸들을 닫을 때 버퍼를 해제해준다. 



### 원형 버퍼

긴 상자로 된 버퍼 대신 원형의 버퍼를 사용하면 새로 들어오는 정보를 자동적으로 빈 공간으로 유도할 수 있다. in 화살표가(책엔 out이던데 오타겠지?) 시계 방향으로 out 화살표를 따라잡기 전까지는 큐에 원소를 추가할 수 있다. 비슷하게 out 화살표가 시계 방향으로 in 화살표를 따라잡기 전까지는 큐에 있는 원소를 제거할 수 있다. 

원형 버퍼(circular buffer), 원형 큐(circular queue), 링 버퍼(ring buffer)등으로 불린다. 



### 추상화를 활용한 코드 개선

동물 추측 게임의 마지막 상태를 저장했다가 불러와서 사용할 수 있다. 이는 C 프로그램의 파일 추상화의 결과이다. 파일에서 입력을 받는 코드를 장치에서 입력을 받는 부분에 쓸 수 있기 때문이다. 자바스크립트의 경우 이것이 어렵다.



### 런타임 라이브러리와 표준 입출력

C 프로그램을 실행하려면 우리가 작성한 코드를 컴파일한 다음, 컴파일된 코드와 이 코드가 사용하는 stdio 라이브러리 등의 다른 코드를 링크해야 한다. 이때 런타임 라이브러리도 포함해야한다. 런타임 라이브러리는 스택과 힙을 설정해서 사용할 수록 준비하는 등의 설정을 담당한다. 런타임 라이브러리는 추가로 터미널 장치 드라이버와 연관된 파일을 하나는 입력을 위해, 다른 하나는 출력을 위해 연다. 

> stdio 라이브러리는 (런타임 라이브러리가 연) 시스템 파일 디스크립터를 파일 포인터(file pointer)와 연관시킨다. 파일 포인터는 버퍼링이나 파일 관리에 필요한 데이터 구조를 가리킨다. stdio는 기본적으로 세 가지 파일 포인터를 제공한다. stdin는 표준 입력, stdout은 표준 출력, stderr는 표준 오류 출력 파일 포인터다. 중요한 정보를 출력해야 하면 stdout 대신 stderr에 내용을 출력해야 한다. stdout과 stderr는 똑같은 대상으로 데이터를 보내지만, stderr는 버퍼를 사용하지 않고 stdout은 버퍼를 사용한다. 오류 메시지를 stdout으로 보내면 메시지가 버퍼에 들어가기 때문에 프로그램이 중단될 때 표시되지 않을 수 있다. stdout과 stderr는 파일 디스크립터를 공유한다.



### 버퍼 오버플로(buffer overflow)

시스템 프로그래밍의 오류 중 하나. stdio의 함수 중에 gets함수는 입력이 버퍼 끝을 벗어나는지 검사하지 않았기 때문에 문제였다. 이 문제는 stdio에 버퍼 경계를 검사하는 fgets함수를 추가해 수정되었다. gets함수는 c11에서 삭제되었다.



### C 프로그램

stdio 외에도 많은 c 라이브러리가 있다. 예를 들어 string 라이브러리는 문자열 비교와 복사를 위한 함수를 제공하고, stdlib에는 메모리 관리 등을 위한 함수가 있다. 



```c
#include <stdio.h> //표준 I/O
#include <stdlib.h> //exit와 malloc을 제공함
#include <string.h> //문자열 처리

struct node {
    struct node *no; //'아니요'에 대응되는 노드를 참조
    sturct node *yes; //'예'에 대응되는 노드를 참조
    char string[1]; //질문이나 동물 이름
}

//메모리 할당 함수
struct node *
make_node(char *string)
{
    struct node *memory; //새로 할당할 메모리를 가리킬 포인터
    
    if ((memory = (stuct node *) malloc(sizeof (struct node) + strlen(string))) ==
        (stuct node *)0) {
        (void)fprintf(stderr,"gta: out of memory.\n");
        exit(-1);
    }
    (void)strcpy(memory->string, string);
    memory->yes = memory->no = (struct node *)0;
    
    return (memory);
}
```

* fprintf: 오류 메시지 출력. stdeff에 보내지는 데이터는 버퍼를 사용하지 않고 출력되기 때문에 프로그램이 예기치 못하게 실패하는 경우에도 메시지가 표시될 가능성이 더 높음
* (void): 강제 타입 변환(cast) 연산자는 fprintf의 결과를 void 타입으로 변환. 코드에서 fprintf가 반환하는 값을 무시하기 때문에 이렇게 강제로 void 타입으로 변환해서 컴파일러에게 우리가 깜빡하고 반환값을 처리하지 않은 것이 아니고, 일부러 반환값을 무시하고 있음을 알려줘서 컴파일러가 경고를 표시하지 않게 해야 한다. 



