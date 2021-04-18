---
layout: default
title: Section3 project 회고
parent: Projects
nav_order: 3
---



web api 개발 

Database: Postgres

web framework: Flask

Deployment: Heroku



## 작업 프로세스

목, 금: 모델 만들기

토: 모델 + MVT 만들기 시작

일: view 함수 만들기

월: view 함수 + html, css

화: view 함수, 배포



### 잘못했던 점 

**모델 설계**

진짜 모델만 빨리 만들었어도, ui 디자인이나 view 함수를 더 잘 다듬었을 텐데 하는 아쉬움이 있다.

1) 함수 설계 : 

date함수: 상장 후 1년 후의 날짜를 구함. 반환값은 start와 end값(pandas datareader는 주가 검색시 종목코드, 주식사이트, start, end 필요). end 값은 start로부터 7일 후(연휴 등 장이 안 열리는 날을넉넉히 고려)

price1 함수:  (price를 여러 군데서 쓰다보니 오류나서 price1으로 바꿈) start와 end를 정해서 나온 여러 값들 중 제일 처음 값 반환

a_year_later_price 함수: date 함수와 price1함수를 합쳐서 int형태로 1년후  종가를 반환

price1함수 안에 pandas datareader api가 들어가는데 이것 때문인지 몰라도 엄청 오래 걸렸다.

오류가 계속 났는데 원인을 계속 몰라서 범위를 끊어서 돌려봤다.

오류 1) 상장폐지된 회사들 주가가 안 불러와짐(2개 정도)

해결 1) 일일이 찾아서 제거해줌



이런식으로 문제를 해결해갔는데, 결국에는...이 모든 과정 대신에 lambda함수로 1년 후 종가를 아주 빠르게!! 구했음

```python
#오류난 것
df['1년후종가'] = a_year_later_price(df['회사명']) 

#성공한 것
df['1년후종가'] = lambda x: a_year_later_price(df['회사명'])
```



리스트 형식으로 만든 다음에 dict 형식으로 만들고 기존 데이터 프레임과 합쳐줌. 



api를 이용해서 1년 후 종가 구하는 함수를 더럽게 만들어서, 루프 도는데 엄청 오래걸렸다. 100개 도는 데 10분씩 걸림ㅠ

2) for loop로 함수가 제대로 안 만들어져서 lambda함수로 만듦(이건 딱히 잘못했다기 보다는 섹션 1부터 계속 데이터프레임 편집할 때 for loop으로 '못해서' lambda함수만 쓰는 것이 걱정된다.)



**웹 api 개발**

1) 데이터베이스가 접속자별로 분리가 안 됨. 모두가 같은 데이터베이스를 쓰기 때문에, 전 사람이 검색한 기록 같은 것이 그대로 남아 있다. 다른 분들은 로그인 기능으로 이를 분리하던데, 로그인 없이도 분리할 수는 없을까??



2)업데이트 함수가 뭔가 거의 다 된 것 같은데 잘 안 됨



3) 블루프린트 분리가 안 되서 delete 함수 하나더 만들어야 함



## 5F 회고

**Fact:** 이번 섹션 3 수업 들으면서도 느꼈지만, 정말 만만치 않은 프로젝트였다.  지난 프로젝트들과 마찬가지로 막판까지 시간에 쫓겨가며 작업을 했는데, 계획대로 코딩이 되지 않아서 스케줄 관리가 정말 힘들었다.

**Feeling:** 

엄청 지쳤지만, 그래도 일단 완성해서 뿌듯하다. 

처음으로 pickle 파일을 이용해서 모델을 colab 외부에서 돌렸는데, 잘 돌아가는 것이 신기했다. 

jinja2는 쓰면서 느낀 것이 jekyll의 liquid text랑 비슷하다는 것을 느꼈다.

heroku로 배포하면 사이트가 엄청 느리다. 특히 처음에 접속할 때 30초 이상 걸리는 것 같다.



**Finding:** 

1) view 함수 

데이터 베이스에 추가한 후에 바로 그 페이지에 있는 테이블에 추가하려면 Get method아래에 query 객체 만든 다음에 return html 하면됨

```python
@index('/', methods='GET', 'POST')
def add_user():
    if method == "GET":
        query1 = User.query.all()
        return render_template('index.html', userlist=query1)
    if method == "POST":
        form1 = requests.form['name']
        if not form1:
            return "이름을 입력해주세요", 400
       	query2 = User.query.filter_by(username= form1.username)
        if query2:
            return "이름이 데이터베이스에 존재합니다.", 400
        else:
            query3 = User(username=form1.username)
        	db.session.add(query3)
            db.session.commit()
        query_final = User.query.all()
        return render_template('index.html', userlist=query)
            
```



2) heroku에서 add-on으로 postgres 추가한 다음에 다시 config 파일로 돌아와서 변경해줘야 함(.env 파일에 적어도 됨)



**Future Action:** 현재 사이트에서 개선이 필요한 부분

0) 로딩이 느리니까 랜딩페이지만 사진 배경 넣고, 나머지 페이지에서는 사진 배경 삭제, 디자인도 왠만하면 부트스트랩 쓰기.

1) csv파일 편집하기:  2020.3월 이후 데이터도 다 넣되, 1년 후 수익률은 null값으로 두기

2)  html (입력 가능한 날짜 보여주기, 검색창이랑 버튼 디자인 변경)

3) 구글 캘린더 api 가져와서 상장일 달력에 표시하기

4) xml 파싱? 근데 만약에 xml파일을 실시간으로 읽어온다고 하더라도 개선되는 것은 훈련 모델이 아니라 예측 가능한 회사들의 개수가 좀 더 늘어나는 것.

5) 예측 페이지에 모델 설명, 아래에 shapley 그래프 넣기, 데이터베이스에 추가되는 멘트 변경(jinja2로 공모가랑 상장일, 공모규모 등 보여주고, 모델의 정확도 관련 설명은 위에 모델 설명하는 쪽에 넣고, 멘트에서는 삭제 )

6) 이거는 정말 어렵겠지만, 테스트 코드 작성해보기

7) dockerfile로 만들어보기



**Feedback:** 동기분들에게 받은 피드백 대부분 내가 생각했는데 고치지 못했던 것과 현실적으로 개선할 수 있는 것이어서 도움이 많이 되었다. 