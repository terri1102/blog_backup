---
layout: default
title: 한 권으로 읽는 컴퓨터 구조와 프로그래밍 7장
parent: 한 권으로 읽는 컴퓨터 구조와 프로그래밍
Grand_parent: Book Reviews
nav_order: 7


---





{:toc}

# 8장 프로그래밍 언어 처리

컴퓨터는 프로그램을 어떻게 해석하고 변환하는가



## 어셈블리 언어

기계어로 프로그램을 구현하기는 너무 어렵기 때문에 프로그래머들은 어셈블리 언어를 만들었다.

**어셈블리 언어(assembly language)**에서는 모든 비트 조합을 외우지 않고 이해하기 쉬운 **니모닉(mnemonics)**을 통해 명령어를 쓸 수 있다. 어셈블리 언어에서는 주소에 이름(레이블, label)을 붙일 수 있다. 그리고 코드에 주석(comment)를 달 수 있다.

**어셈블러(assembler)**: 어셈블리 언어로 작성된 코드를 읽어서 동등한 기계어 코드(machine code)를 생성해주는 프로그램. 이런 변환 과정에서 어셈블러는 레이블이나 심볼(symbol)의 값을 결정해 채워 넣어준다.

```assembly
		load #0	; first를 0으로 설정
		store first 
		load #1 	; second를 1로 설정
		store second
again: 	load first	; first와 second를 더해서
		add	second 	; next를 계산
		store next
					; 계산한 수를 가지고 뭔가 흥미로운 일을 함
		load second	; second를 first로 옮김
		store first 
		load next	; next를 second로 옮김
		store second
		cmp	#200	; 다 끝났는가?
		ble again	; 다 끝나지 않았다면 again으로 가서 계속 진행
first:	bss	1		; first가 저장될 위치
second: bss 1		; second가 저장될 위치
next: 	bss 1		; next가 저장될 위치
```



**bss(block started by symbol)**라는 의사명령어(pseudo-instruction)는 메모리 덩어리(1워드)를 예약하되 메모리 안에는 아무 값도 넣지 않는다. 의사명령어는 기계어와 직접 대응되지는 않지만, 어셈블러에 지시를 내린다.

**부트스트랩(bootstrap, 부트)**: 컴퓨터를 부팅하는 과정은 롬 등에 있는 작은 프로그램을 메모리로 읽어오는 것부터 시작한다. 이 프로그램은 필요한 초기화를 진행한 후 더 큰 프로그램을(보통은 하드 디스크, USB 등 대용량 저장장치에서) 읽어오고, 이 프로그램은 더 큰 운영체제 등을 불러올 수 있다.

 

## 고수준 언어

**고수준 언어(high-level language)** : 어셈블리 언어보다 더 높은 추상화 단계에서 작동하는 언어. 고수준 언어의 소스 코드는 **컴파일러(complier)**라는 프로그램에 의해 실행된다. 컴파일러는 소스 코드를 기계어로 번역(컴파일) 해준다. 기계어 코드를 다른 말로 **목적 코드(object code)**라고도 한다.

ex) 포트란(FORTRAN) : 처음 생긴 고수준 언어. formula translator

고수준 언어는 변수(variable)를 사용하면 자동으로 메모리가 할당되기 때문에 명시적으로 사용하려는 메모리를 선언할 필요가 없다. 

**비구조적(unstructured) 언어:** 포트란이나 BASIC은 줄 번호와 GOTO로 이뤄진 연결을 통해 프로그램이 실행되기 때문에 레이블 번호를 순서대로 작성했다.

GOTO: 명시적인 분기

## 구조적 프로그래밍

비구조적 언어는 GOTO와 레이블을 조합할 때 구조가 없어도 된다. 하지만 이는 잘못된 GOTO 사용으로 인해 발생할 수 있는 스파게티 코드 문제가 있었고 이를 해결하기 위해 구조적 프로그래밍이 개발되었다.

**구조적(sturctured) 프로그래밍** : GOTO(명시적인 분기)가 없고 제어 흐름이 깔끔하기 때문에 프로그램을 더 쉽게 이해할 수 있다.

```javascript
//자바 스크립트 설명: while 뒤에 있는 괄호 속의 조건식이 참인 한 중괄호{} 사이에 있는 문장이 반복 실행된다. 조건이 거짓이 되면 } 뒤에 있는 문장부터 실행이 계속된다.

var first; // 첫 번째 수
var second; // 두 번째 수
var next;	// 수열의 다음 수

first = 0;
second = 1;

while ((next = first + second) < 200) {
    //수를 가지고 뭔가의 연산을 한다.
    first = second;
    second = next;
}
```



## 어휘 분석(texical analysis)

**어휘 분석**은 코드를 기호(문자들)로부터 단어와 같은 성격의 토큰(token)으로 변환하는 과정이다. 어휘 분석 과정은 먼저 입력값을 토큰들로 분리하여 추출한 다음 토큰을 연산자와 피연산자(변수와 상수)로 구분한다. 

실수를 이루는 요소를 도해로 나타내면 아래와 같다.

![real number recognition](https://image.slidesharecdn.com/ch04-161127121750/95/ch04-14-638.jpg?cb=1480249448)

동그라미는 상태(state), 화살표는 전이(transition)라고 부른다. 더 이상 따라갈 수 있는 화살표가 없으면 부동소수점 수가 끝난다.

베커스 나우르 표기법(BNF, Backus-Naur form)처럼 형식적으로 정의된 방법도 있다.

```bnf
<digit> 		::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
<digits> 		::= <digit> | <digits> <digit>

<e>				::= "e" | "E"
<sign>			::= "+" | "-"
<optional-sign> ::= <sign> | ""

<mantissa>		::= <digits> | <digits> "." | "." <digits> | <digits> "." <digits>
<floating-point> ::= <optional-sign> <mantissa> <optional-exponent>


// ::=   왼쪽에 있는 요소를 오른쪽에 있는 요소로 대치 가능
// ""    리터럴. 쓰여 있는 그대로 나타냄
```

**유한 상태 오토마타(FSA, finite state automata)** 

위의 어휘분석 도해에 시작 상태와 종료 상태, 입력값의 범위를 명확히 정하면 토큰을 BNF로 정의한 것과 똑같은 표현법이 되는데 이를 유한 상태 오토마타라고 한다.

유한 상태 기계 또는 유한 오토마톤는 컴퓨터 프로그램과 전자 논리 회로를 설계하는 데에 쓰이는 수학적 모델이다. 간단히 '상태 기계'라고 부르기도 한다. 유한 상태 기계는 유한한 개수의 상태를 가질 수 있는 오토마타, 즉 추상 기계라고 할 수 있다



### 1 상태 기계

언어 토큰을 추출하는 방법의 하나로 상태로 이뤄진 집합과 한 상태에서 다른 상태로 전이되는 원인의 목록으로 이뤄진 **상태 기계(state machine)**을 생각해볼 수 있다.

| 입력      | 1    | 2    | 3    | 4    | 5    | 6    | 7    |
| --------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0         | 3    | 3    | 3    | 4    | 7    | 7    | 7    |
| 1         | 3    | 3    | 3    | 4    | 7    | 7    | 7    |
| 2         | 3    | 3    | 3    | 4    | 7    | 7    | 7    |
| ...       | ...  | ...  | ...  | ...  | ...  | ...  | ...  |
| e         | 오류 | 오류 | 5    | 5    | 오류 | 오류 | 완료 |
| ...       | ...  | ...  | ...  | ...  | ...  | ...  | ...  |
| 기타 문자 | 오류 | 오류 | 오류 | 오류 | 오류 | 오류 | 오류 |

```javascript
state = 1;
while (state > 0)
    state = state_table[state][next_character];
```



### 2 정규식(regular expression)

정규식은 **패턴 매칭**을 통해서 입력을 토큰으로 처리한다.

`[+-]?(([0-9]*\.?[0-9]+)|(\[0-9]+\.?[0-9]*))([Ee][+-]?[0-9]+)?`

* \* 바로 앞의 패턴을 0번 이상 반복
* \+ 바로 앞의 패턴을 1번 이상 반복
* ? 패턴이 0번 또는 1번 나타남
* [] 그 집합 안에 있는 어느 한 문자와 매치됨

**lex(lexical analyzer, 어휘 분석기)** : 입력이 정규식과 매치될 때 사용자가 제공한 프로그램 조각을 실행해주는 상태 테이블 기반의 프로그램



## 단어에서 문장으로

문자 시퀀스를 단어로 분류하는 어휘 분석에서 이제 더 나아가서 여러 단어를 모아 문법에 맞는 문장으로 분석할 필요가 있다.

**yacc:** 스택을 사용하는 시프트-리듀스(shift-reduce) 파서(parser)

시프트: 토큰을 스택에 넣음

리듀스: 스택의 맨 위부터 매치된 토큰들을 다른 어떤 것으로 대치함



## :evergreen_tree: 파스 트리

고수준 언어는 컴파일할 수도 있지만 인터프리트할 수도 있다. 

* **컴파일 언어**: 소스코드를 구체적인 기계에 맞는 기계어로 번역함
* **인터프리터 언어** : 실제 기계에 사용할 기계어 대신 가상 머신에서 실행됨. 가상 머신은 소프트웨어로 작성된 기계.  일부 인터프리터 언어는 인터프리터에 의해 직접 실행되거나 나중에 해석될 수 있도록 중간어(intermediate laguage)로 컴파일되기도 함.

일반적으로 컴파일러나 인터프리터는 파스 트리(parse tree)를 구성한다. 파스트리는 언어 문법으로부터 만들어낸 DAG(directed acyclic graph)데이터 구조이다. 

```D
    node 						 union
|---------|                  |------------|   
|  code   |                  |부동소수점  .f|
|  leaf0  |         <-       |정수       .i|
|  leaf.. |                  |노드 포인터 .n| 
|  leafn  |                  |문자 배열   .s|
```

각 노드에는 노드 유형을 표시하는 code와 leaf의 배열이 있다. 각 leaf의 해석은 code에 따라 달라지며 leaf는 여러 타입의 정보를 담을 수 있는 공용체이다.

파스트리의 예시

![](data:image/png;base64,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)

yacc를 사용해 파스 트리 구성

```.
calculator  : statements			   {do_something_with($1);}
		    ;
statements  : /*비어있음*/
			| statement statements     {$$.n = makenode(2, LIST, $1, $2); }
            ;
operand     : INTEGER			       { $$.n = makenode(1, INTEGER, $1, $3); }
		    | VARIABLE			       { $$ = makenode(1, VARIABLE, $1);}
			;
expression 	: expression PLUS operand	{ $$.n = makenode(2,PLUS, $1, $3); }
			| expression MINUS operand	{ $$.n = makenode(2, MINUS, $1, $3);}
			| expression TIMES operand	{ $$.n = makenode(2, TIMES, $1, $3);}
			| expression DIVIDE operand	{ $$.n = makenode(2, DIVIDE, $1, $3);}
			| operand
			;
assignment	: VARAIBLE EQUALS expression { $$.n = makenode(2, EQUALS, $1, $3);}
			;
statement 	: expression				{ $$ = $1; }
			| assignment				{$$ = $1;}
			;
```



## :card_index: 인터프리터

파스 트리의 루트를 인자로 받는 do_something_with 함수는 인터프리터가 파스 트리를 실행하게 한다. 

실행의 첫번째 부분은 연결 리스트를 순회하는 것이고 두 번째 부분은 계산이다. 깊이 우선 순회를 통해 재귀적으로 계산을 한다.

```
	[node <- root]
	 	 |
  -><node != NULL?>-아니요-> 완료
  |	  	 | 예
  |	[evaluate node leaf0]
  |    	 |	  
  |	[node <- node leaf1]
  |	  	 |
  |-------
```

* 리스트 순회와 계산 코드를 yacc에 붙여넣으면 파스 트리를 즉시 실행할 수 있다. 다른 방법으로는 자바나 파이썬처럼 파스 트리를 파일에 저장했다가 나중에 실행하는 방법도 있다. 이때 파일에 저장되는 내용은 기계어 명령어들인데, 소프트웨어로 구현된 기계의 명령어를 저장한다. 그리고 모든 대상 기계마다 저장된 파스 트리를 실행하는 프로그램이 있어야만 한다. 똑같은 인터프리터 소스 코드를 여러 대상 기계에 대해 컴파일해 사용하는 경우도 자주 있다.

**인터프리터 구조**

![interpreter]

프론트 엔드는 파스 트리를 만들고 파스 트리는 어떤 중간 언어로 표현된다. 백엔드는 이 언어를 실행할 대상 환경마다 하나씩 존재한다.



## :computer: 컴파일러

컴파일러도 인터프리터와 비슷하지만 백엔드 실행 코드 대신 코드 생성기가 들어간다.

**컴파일러 구조**



코드 생성기는

## :star2: ​최적화

**최적화기(optimizer)** : 대부분의 언어 도구에는 최적화기라는 추가 단계가 파스 트리와 코드 생성기 사이에 들어간다. 최적화기는 파스 트리를 분석하고 이 결과를 활용해 더 효율적인 코드를 생성해내도록 파스 트리를 변환한다.



## 하드웨어를 다룰 때 주의하라





