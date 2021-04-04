# Heroku로 배포하기



1. Heroku 가입

2. cli창에서 로그인

```bash
$ heroku login
```

4. app 프로젝트 폴더에 Procfile(process file) 만들기

```python
#Procfile
#application factory 패턴일 때
web: gunicorn --workers=2 'flask_app:create_app()'
```



5. app 프로젝트 폴더에 requirements.txt 와 runtime.txt로 개발 환경 저장

```bash
$ pip freeze > requirements.txt
```

```python
#runtime.txt
python-3.8.8 #소문자
```



#### Heroku 명령어

```bash
heroku apps #어플리케이션 목록
```

```bash
heroku create my_app #애플리케이션 만들기
```

```bash
heroku open -a my_app #애플리케이션 열기
```

```bash
heroku run bash #heroku 서버에 로그인

#상태 확인
$ whoami

$ cd / && ls -la
```

json 아니고 data 형태로 넘기는 것

data에 'username'만 넘겨짐