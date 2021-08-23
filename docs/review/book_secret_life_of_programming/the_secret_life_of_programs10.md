---
layout: default
title: 한 권으로 읽는 컴퓨터 구조와 프로그래밍 10장
parent: 한 권으로 읽는 컴퓨터 구조와 프로그래밍
Grand_parent: Reviews
nav_order: 10
use_math: true

---





{:toc}

# 10장 애플리케이션 프로그래밍과 시스템 프로그래밍 

고수준 언어와 저수준 언어 프로그래밍 방식 비교

![slp10]

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



















