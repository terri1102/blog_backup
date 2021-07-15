---
layout: default
title: Flask
date: 2021-03-27
parent: Web Development
comments: true
nav_order: 8

---



# 아주 간단한 Flask app 만들기

너무 오랜만에 Flask 앱 만들려고 하니까 하나도 기억이 안나서...다시 정리해 볼 겸 조금씩 기본적인 내용을 정리해보려고 한다. 최종 목표는 Flask와 MongoDB, Dash로 간단한 대시보드를 만드는 것이다.



## Flask 설치

새로운 가상환경을 생성한 다음에 flask 라이브러리를 깔아준다.

```bash 
$ pip install flask
```



## 앱 폴더 생성

flask_project 폴더를 생성하고 또 그 안에 flask_app 폴더를 생성한 후에 `__init__.py` 파일을 만든다. 보통 flask_project과 flask_app 이름을 동일하게 하는 경우도 있지만 여기서는 설명의 편의성을 위해 다르게 쓸 것이다.

```python
flask_project
└── flask_app
	└── __init__.py
```



## init 파일 작성

`__init__.py` 파일을 작성한다.

```python
#__init__.py

from flask import Flask

app = Flask(__name__)
```



## Flask 실행

`__init__.py`  작성 후 flask run으로 실행한다. 이때 flask_app 밖에서 실행해야 제대로 실행이 된다. 즉 flask_project 폴더 안에서 실행하면 된다.



1) app.run() 매서드로 실행

```bash
$ FLASK_APP=flask_app flask run
#FLASK_app 환경변수
#쉘이나 터미널에서 실행할 때 환경변수 설정가능
```

매번 FLASK_APP=flask_app을 치는 것이 귀찮다면, 환경 변수를 설정해 export해도 된다. 

```bash
$ export FLASK_APP=flask_app
$ flask run 
```





2) python flask_app 로 직접 실행가능



flask run 대신에 python 스크립트 실행하듯이 실행 가능하다.

---

## 라우트 추가

기본 페이지를 만들기 위해 `__init__.py`에 라우트를 추가해보았다. 

```python
#__init__.py

from flask import Flask
app = Flask(__name__)

@app.route('/') #엔드포인트 설정
def main_page():
    return "Main Page"
```



## 블루프린트

라우트가 많아질수록 한 파일에서 관리하기 복잡해지기 때문에, 기능별로 라우트를 나누어서 사용할 수 있다.



## 어플리케이션 팩토리 만들기

circular import를 피하기 위해 사용한다. 



---



one과 first는 엄청 다르다!

Query 객체의 all(), one(), first() 메소드는 즉시 sql을 호출하고 non-iterator 값을 반환

-all(): 리스트 반환

```python
[<User('1','username1')>, <User('2','username2')>]
```

-first(): 첫째값을 scaler로 가져옴

```python
<User('1','username1')>
```

-one(): 모든 행을 참조해 식별자를 값으로 가지고 있지 않거나 여러 행이 동일한 값을 갖는 경우 에러를 만든다

```python
from sqlalchemy.orm.exc import NoResultFound

try:
    user = query.filter(User.id == 2).one()
except NoResultFound, e:
    return e
```



출처: https://edykim.com/ko/post/getting-started-with-sqlalchemy-part-2/





```python
def get_user():
      username = request.args.get('username', None) #none 대신 ' ', 0 가능#또는 requests.args["num"]
            #breakpoint() #디버깅
      user = db.session.query(User).filter(User.username==username).one_or_none() #아니면 one() 아니면 first()  #session 안 쓰는 쿼리..
     
      if username is None: #username이 입력되지 않은 경우
            return "No username given", 400
      if user is None:  #username이 db에 없는 경우
            return "User '{}' doesn't exist".format(username), 404
      
      id = str(user.id)
            #return id, 200
            #db.session.add(name)
            #str(user.id)
      return id, 200           #인덴트 위치 확인
```



### flask.Flask class

```python
class flask.Flask(import_name, static_url_path=None, static_folder='static', static_host=None, host_matching=False, subdomain_matching=False, template_folder='templates', instance_path=None, instance_relative_config=False, root_path=None)
```

```python
from flask import Flask
app = Flask(__name__)
```

----

n333

flask 기본방식->application factory

환경변수: 개발시, 배포시 데이터베이스가 달라야 함..문제 있을 수도 있으니까. api키 다른 거 사용하기도

flask-sqlalchemy



1.가상환경 설치

2.pip install로 flask 설치



3-1)python으로 직접 실행

python __init__.py

3-2)FLASK_APP=flask_app flask run

export FLASK_APP=flask_app 환경변수 고정

export FLASK_ENV=development

-->environment: development로 바뀜.

 코드의 변경이 있으면 서버를 다시 껐다 켜준다.



breakpoint()로 pdb(python debugger)에 접속하면 python 코드 쓸 수 있다.

근데 for loop은 안 돌아감(여러 줄 코드 안 돌아감)

나가기: ctrl + d 나 exit()



print()



```python
#init.py

from flask import Flask, Blueprint

app = Flask(__name__) #flask_app 폴더의 이름을 가져옴

@app.route('/')
def index():
    print('hello spongebob')
    return 'spongebob', 200


############블루프린트를 views 폴더 아래 main_views.py로 이동시킴
main_bp = Blueprint('main', __name__)


@main_bp.route('/') #엔드포인트가 중복 매핑된다면?
                    #@app.route('/'), @main_bp.route('/') #보통 첫 번째로 찾은 url을 등록함
def main_index():
    return 'patrick', 201

#블루프린트를 추가해야 실행됨
app.register_blueprint(main_bp)

################

if __name__ == '__main__': #없어도 flask run에서는 실행됨
    app.run(debug=True)
    
 
```

---분리!

```python
#init.py

from flask import Flask
from flask_app.viws.main_view import main_bp

app = Flask(__name__) #flask_app 폴더의 이름을 가져옴

@app.route('/')
def index():
    print('hello spongebob')
    return 'spongebob', 200

app.register_blueprint(main_bp)


if __name__ == '__main__': #없어도 flask run에서는 실행됨
    app.run(debug=True)
```



```python
#main_view.py
#views 폴더 안에는 라우트 관련 파일만 넣는다
from flask import Blueprint

main_bp = Blueprint('main', __name__)
#블루프린트를 추가해야 실행됨

@main_bp.route('/') #엔드포인트가 중복 매핑된다면?
                    #@app.route('/'), @main_bp.route('/') #보통 첫 번째로 찾은 url을 등록함
def main_index():
    return 'patrick', 201

#app.register_blueprint(main_bp) 얘
```



### 파일과 폴더 분리를 통해 의존성을 제거

MVC 패턴: model(데이터), view(유저가 보는 페이지), controller(라우트)

--모델, 뷰, 컨트롤러가 분리됨! 하나의 폴더에 작성하면 복잡하니까. 디버깅도 힘들고

MVT 패턴: model, view, template

--우리는 MVT 패턴에 가깝..

--얘도 분리하긴 함



-----application factory 사용해야 하는 이유

app = Flask(__name__)

필요할 때마다 어플 뱉는 함수 만들어서 app 찍어냄

(매번 app변수 만들 수 없으니까)

각 서버마다 실행하는 어플 하나?



app=create_app() 없어도 되는 이유는 flask가 자동으로 create_app() 찾기 때문에



#앱이 여러개 필요할 때가 있음

반드시 클라이언트 한명당 한개의 app이 필요한 건 아닌데 app이 많아질수록 많은 클라이언트를 다룰 수 있게 해줌

서버에 접속하는 클라이언트들이 많으면 같은 설정의 복수의 app이 필요한거라 이해하면 될까요? 일단 yes



같은 설정의 복수 app을 계속 실행하면 아까 예처럼 한개의 예만 출력



플라스크 특유의 개념...

나중에 가서는 로드밸런싱의 인원?이 많아서 신경쓸 필요가 없어짐..?

워커..

gunicorn

application cloning으로 여러 워커 복사

---

내가 생각하는 flask_app 파일 작성 순서

```python
$ tree

-flask_app
--__init__.py
->views
  |-main_view.py
  |-user_view.py
->models
  |-tweet_model.py
  |-user_model.py
 ->services
  |-api.py
->templates
  |-index.html
->static
  |-style.css
```

1) flask_app의 init.py 파일

2) view 폴더 안의 파일들(circular import 주의)

3) models 폴더 안의 파일들(class)

4) templates 폴더 안의 html

5) 그외 css나 services 폴더 안 api 파일



### init.py 파일 예시

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

db = SQLAlchemy()
migrate = Migrate()

def create_app():
    app = Flask(__name__) #flask_app 폴더의 이름
    app.config['SQLALCHEMY_DATABASE_UTI'] = "sqlite:///flask_db.sqlite3"
    #app.config['SQLALCHEMY_TRACK_MODIFICATION'] = False  #deprecation warning 생략
    
    db.init_app(app) #함수 밖에 있던 app을 init함 ->import 문제 안 생김
    migrate.init_app(app, db)
    
   @app.route('/')
	def index():
        print('hello spongebob')
        return 'spongebob', 200
    
    from flask_app.views.main_view import main_bp
    app.register_blueprint(main_bp)
    
    return app
```



### view 폴더 안 main_view.py 예시

```python

```









### Flask-sqlalchemy와 sqlalchemy의 차이

1. Flask-SQLAlchemy 는 다음 사항들에 접근 가능하도록 해줍니다 :
   - `sqlalchemy` 와 `sqlalchemy.orm` 의 모든 함수 및 클래스
   - `session` 이라는 사전에 설정된 세션 객체
   - SQLAlchemy 의 엔진
   - `SQLAlchemy.create_all()` 과 `SQLAlchemy.drop_all()` 은 정의된 모델들을 기반으로 테이블을 생성 및 드롭합니다.
   - `Model` 이라는 declarative base 로 설정된 `baseclass`
2. `Model` 클래스는 파이썬의 일반 클래스와 동일하게 작동하고 `query` 라는 특성이 있어 모델로부터 쿼리를 할 수 있습니다.
3. 세션을 커밋해야 기록이 되지만 작업 뒤에는 Flask-SQLAlchemy 에서 자동으로 제거해주기 때문에 따로 제거하지 않아도 됩니다.

flask-sqlalchemy는 sessionmaker 없이 session 바로 생성, 엔진 자동 생성

sqlalchemy.create_all(), drop_all()사용 가능

model도 전달 model 클래스는 query라는 특성이 있음



설치시 유의사항:

sqlalchemy를 1.3버전으로 설치하기, 1.4로 사용시 flask-sqlalchemy 사용시 에러 발생 



flask_sqlalchemy의 db.Model 은 sqlalchemy의 Base다

```python
app = Flask(__name__)
db = SQLAlchemy(app) #flask_sqlalchemy와 sqlalchemy가 연동됨
```



www에서 요청시 gunicorn이 서버에 접속함. gunicorn이 알아서 요청을 여러 app에 나눠서 보낸다.

gunicorn: 서버 담당

flask: 웹 어플리케이션 담당



flask로 만들고 서버 실행하면 http wsgi 라이브러리로 서버 생성. 개발단계 서버이기 때문에 여러 문제가 있다.

gunicorn을 통해서 이제 서버를 돌린다..

```python
gunicorn -w 4 'flask_app:create_app()' 
#127.0.0.1:8000으로 접속
```

워커를 통해서 작업

여러 워커로 여러 개의 트래픽을 감당할 수 있다.

보통 1코어당 2개 워커 권장



windows 에서 안 되면 flask run으로 하자



참고

Web Server와 WAS의 관계

https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html



하둡과 스파크?



---

```python
class Tweet(db.Model):
    __tablename__ = "Tweet"
    id = db.Column(db.BigInteger, primary_key=True)
    text = db.Column(db.String)
    embedding = db.Column(db.PickleType)
    user_id = db.Column(db.BigInteger, db.ForeignKey("Users.id"))

    user = db.relationship("Users", foreign_keys = user_id, backref=db.backref('tweets', lazy=True))

    def __repr__(self):
        return f"<Tweet {self.id} {self.text}>"

    

def parse_records(db_records):
    parsed_list = []
    for record in db_records:
        parsed_record = record.__dict__
        print(parsed_record)
        del parsed_record["_sa_instance_state"]
        parsed_list.append(parsed_record)
    return parsed_list
```



## 참고

flask request 종류 https://velog.io/@sangmin7648/%EC%98%A4%EB%8A%98%EC%9D%98-%EB%B0%B0%EC%9B%80-045
