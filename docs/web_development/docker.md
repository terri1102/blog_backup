---
layout: default
title: Docker 정리
date: 2021-03-02
categories:
  - TIL
tags:
  - TIL
comments: true
nav_order: 3
parent: Web Development
---



# CLI 

명령어 정리

```bash
mkdir foldername #디렉토리(폴더) 생성

ls #폴더 및 파일 보기

ls -l #-l: 옵션, 파라미터
#권한, 패키지관리자 권한, 만들어진 시간, 파일 이름
#권한예시 drwxr-xr-x 
#d: 디렉토리 
#rwx: (유저권한) read, write, execute
#r-x: 그룹권한
#r-x: 유저 아닌 다른 사람 권한

pwd #현재위치 확인

clear #화면 지우기
ctrl + l #화면 지우기

cd foldername/folder2 #폴더로 들어가기
#윈도우: \로 경로 표현, linux, MAC, Bash: /로 경로 표현

#상대경로
cd ../ #상위경로로 이동
cd .. #위와 동일, 폴더 한 수준 위로 올라감

cd ./foldername #현재위치에서 foldername으로 이동

#절대경로
cd /F:/home/test
#bash에서는 윈도우라도 경로 표시할 때 일반 슬래시(/) 사용함
#윈도우 cmd의 경우 백슬래시를 쓰는데...절대 경로 표시하고 싶으면 \\ 두 개 써야함
#ex) F:\\home\test 이런식으로

cd ~ #홈으로

touch file.txt #file.txt 생성 

rm file.txt #file.txt 삭제

rm -r folder #folder 디렉토리 삭제

rm -rf #force 무조건 지우겠다

```



웹 브라우저(url주소의 ip주소 요청)-DNS(ip주소 응답)-웹 브라우저(ip주소 이용해 웹 페이지 요청)-웹 서버(html 응답)



*DNS(Domain Name Server)



*URL(Uniform Resource Locator)

```html
http://www.terri1102.github.io/doc/index.html
접근프로토콜://ip주소 또는 도메인 이름/문서경로/문서이름
```

*요청하는 쪽: 클라이언트, 요청 응답하는 쪽: 서버

*요청 메서드: GET, PUT, POST, PUSH, OPTIONS

GET: 정보를 요청(SELECT)

POST: 정보를 밀어넣기(INSERT)

PUT: 정보를 업데이트(UPDATE)

DELETE: 정보를 삭제(DELETE)

HEAD: (HTTP)헤더 정보만 요청, 해당 자원이 존재하는지, 서버에 문제 없는지 확인

OPTIONS: 웹서버가 지원하는 메서드의 종류 요청

TRACE: 클라이언트의 요청을 그대로 반환



포트: IP주소는 연결할 컴퓨터를 구분하는 데 사용되기 때문에 포트는 컴퓨터의 어떤 서버 프로그램을 실행할지 알려줌

포트는 숫자로 된 번호로서 서버 프로그램마다 구분되는 포트 번호를 사용하며, 클라이언트는 IP주소와 함께 포트 번호를 사용해서 원하는 서버 프로그램에 연결하게 된다.

---



# Docker 정리

<img src="https://cdn-media-1.freecodecamp.org/images/1*easlVE_DOqRDUDkVINRI9g.png" alt="Docker" style="zoom:80%;" />

https://www.freecodecamp.org/news/docker-quick-start-video-tutorials-1dfc575522a0/



## Docker

어플리케이션 실행 환경을 코드로 작성할 수 있고 OS를 격리화하여 관리하는 기술이다. 



#### 사용하는 이유: 

**1. 환경 표준화**

다양한 환경에 상관없이 어플리케이션 똑같이 동작하게 하려고. 

도커 이미지로 만든 컨테이너는 어떤 OS든지, 어떤 클라우드든지 똑같이 동작한다. 

컨테이너 기술을 발전시킨 프로그램(or 회사)



**2. 컴퓨터별 사용설정 표준화**

같은 OS라도 환경변수 등 컴퓨터별로 다른 설정이 필요할 수 있음 

컴퓨터 별로 홈 디렉토리가 다르듯 어플리케이션을 설치할 때 방화벽 설정, 사용자 권한 설정, Port 설정 등 컴퓨터에 맞게 변경해줘야하는 부분들이 있다.



**3. 리소스 격리성**

도커는 하나의 컴퓨터에서 여러 개의 컴퓨터를 이용하는 것처럼 사용할 수 있게 해준다.

예를 들어 IP주소는 컴퓨터의 고유 주소인데 웹서버 별로 IP 다르고 포트번호도 방화벽 규칙도 다르다면 충돌이 일어나게 된다.



**가상머신과 Docker**

리소스 격리성을 더 잘 제공하는 것은 가상머신, Docker는 리소스 격리성 제공이 가상머신보다는 못하지만 더 빠름



VirtualBox, VMware 와 같은 해당하는 가상머신은 내가 개발 혹은 사용하는 환경을 이미지로 저장하고 Host OS 위에 게스트 OS를 띄우는 방식입니다





## Linux Container

Docker의 기반이 되는 기술

1. 프로세스의 구획화

- 특정 컨테이너에서 작동하는 프로세스는 기본적으로 그 컨테이너 안에서만 액세스 할 수 있습니다.
- 컨테이너 안에서 실행되는 프로세스는 다른 컨테이너의 프로세스에게 영향을 줄 수 없습니다.

1. 네트워크의 구획화

- 기본으로 컨테이너 하나에 IP 주소가 할당되어 있습니다.

1. 파일시스템의 구획화

- 컨테이너 안에서 사용되는 파일 시스템은 구획화되어 있습니다. 그렇기 때문에 해당 컨테이너에서의 명령이나 파일 등의 액세스를 제한할 수 있습니다.



## Docker 사용법



### 가상환경 켜고 사용하기

```bash
$ conda create --name new_env   #vs code에선 알아서 터미널 언어 바꿔줌
$ conda activate new_env
#가상환경의 위치와 docker에서 사용할 폴더의 위치는 달라도 상관없다.
#내 가상환경의 위치: ~/anaconda3/envs

#단, requirements.txt나 pipfile를 사용하려면 가상환경 실행하고 그 폴더 안으로 들어간 후에
#pip install -r requirements.txt 나 pipenv lock로 패키지나 플러그인 설치해야 함
```



프로그램을 실행하면 프로세스가 되듯이, 도커 이미지가 실행되면 도커 컨테이너가 된다.

이미지 하나로 여러 도커 컨테이너 실행해서 동시 개발 가능



데몬이 켜졌나 확인하기...

도커 데스크탑 실행하면 켜진다.

### Docker Image 읽는 법

```bash
Registry_Account/Repository_Name:Tag
#예시: docker/whalesay:latest
#registry 이미지가 배포된 공간 ex)Docker hub
```



### Docker image 가져오기

```bash
$ docker image pull docker/whalesay:latest
```



### Docker image 리스트

```bash
$ docker image ls
$ docker images
```



### Docker 이미지 실행->컨테이너

```bash
$ docker container run --name myName docker/whalesay:latest cowsay boo
#{container} run : 컨테이너 실행
#--name: option, --name은 컨테이너 이름 할당하는 옵션
#cowsay: command, 컨테이너에서 cowsay를 호출
#boo: ARG.., command인 cowsay에 넘겨질 파라미터
```



### Docker 컨테이너 리스트

```bash
$ docker container ps -a #-a 실행 중이지 않은 컨테이너도 보이기
#ps: 컨테이너의 리스트 출력
```



### 실행 중인 컨테이너 내부로 들어가기

``` bash
#인터렉티브 모드로 run하기
$ docker run -it image   #아예 처음부터 인터렉티브 모드로 들어가서 바로 명령 내릴 수 있음

#백그라운드 모드로 컨테이너를 시작해 놓았을 때 docker exec를 써서 들어가기
$ docker exec -t -i containername /bin/bash

#도커 데몬이 응답이 없고 docker exec를 쓸 수 없는 상황->nsenter 사용해 서버에서 직접 컨테이너에 들어갈 수 있다.
```



### Docker 컨테이너 삭제

```bash
$ docker container rm myName
```



### Docker 이미지 삭제

```bash
$ docker image rm docker/whalesay
$ docker rmi docker/whalesay #도 같은 기능 수행
```



### 모든 container 삭제

```bash
$ docker stop $(docker ps -a -q)
$ docker rm $(docker ps -a -q)
```



### 모든 이미지 삭제

```bash
$ docker rmi $(docker images -q) 
```



### 모든 volume 삭제

```bash
$ docker volume rm $(docker volume ls -q)

$ docker volume prune
#WARNING! This will remove all local volumes not used by at least one container.
```



### Docker 컨테이너 일회성 실행

이미지 받아오고 컨테이너 실행 후 컨테이너 종료시 컨테이너 삭제

```bash
$ docker container run --name my_name --rm docker/whalesay:latest cowsay boo
#--rm 컨테이너를 일회성으로 실행
#이미지는 남아있음
```



### Docker logs

```bash
$ docker logs id #주로 오류났을 때 확인, 난 exited된 컨테이너 조사할 때 사용 
```



### 모든 환경변수 보기

```bash
$ docker exec container env
```



### 기타

```bash
#도커 버전 출력
$ docker version

#서버 정보
#도커 서버가 어떤 파일 시스템 백엔드에서 동작 중인지, 커널 버전이 무엇인지, 어떤 OS 쓰는지, 얼마나 많은 이미지와 컨테이너가 저장되어 있는지
$ docker info

#기본 루트 디렉터리는 /var/lib/docker이지만 이를 바꾸고 싶다면 데몬을 시작하는 시작 스크립트에서 --grpah 옵션으로 새로운 디렉터리를 가리키도록 수정
$ sudo docker daemon\
-H unix//var/run/docker.sock\
-H tcp://0.0.0.0:2375 --graph="/data/docker" #/data/docker 디렉토리로 변경

```

```bash
$ docker container run -it image:tag
#>>컨테이너가 실행되서 직접 명령 가능
env
#>>HOSTNAME, ENV, PATH, PWD, SHLVL, HOME 나옴
#ctrl + D 누르면 종료
```



```bash
#Options:
      --config string      Location of client config files (default
                           "C:\\Users\\Boyoon Jang\\.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "C:\\Users\\Boyoon Jang\\.docker\\ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "C:\\Users\\Boyoon Jang\\.docker\\cert.pem")
      --tlskey string      Path to TLS key file (default
                           "C:\\Users\\Boyoon Jang\\.docker\\key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

#Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Build with BuildKit (Docker Inc., v0.5.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., v0.5.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

#Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
```



-it: -i, -t를 동시에 사용한 것, 사용자와 컨테이너 간에 인터렉션이 필요하다면 해당 옵션을 사용. ctrl+d로 종료

docker image를 통해서 다양한 OS의 어플리케이션 환경 구성 가능



Echo?

---



#### 내가 겪은 문제

**No directory found 오류:** 

docker 이미지는 var/lib/docker/overlay2에 저장되는데 위의 코드를 치니까 스크립트를 C:/program files/git 디렉토리 아래에서 찾음

실패한 코드

```bash
$ docker run -it -d -e envari=v1 -e envari=v2 image:tag python script.py
```



**원인:** 윈도우에서 bash의 Mingw이 경로 수정함

**해결방법1**: MSYS_NO_PATHCONV=1 를 매번 코드 앞에 치거나 //bin/bash 이런식으로 이스케이프

**해결방법2:** docker_profile 수정   

```bash
docker()
{
        (export MSYS_NO_PATHCONV=1; "docker.exe" "$@")
}
#이 코드를 docker_profile 맨 아래에 복붙했음
```

근데...자꾸 다른 오류가 났다가 갑자기 해결돼서 뭐가 문젠지 왜 해결된 건지 잘 모르겠다...

**성공 코드**

```bash
$ docker run -it --env envariable=v1 --env envariable2=v2 image:tag python script.py  
#성공 했을 때 이미지 pull된 상태에서 코드 실행함
#이미지 pull했을 때 
#환경변수 쓸 때 = 옆에 띄어 쓰면 안 됨
```

wsl2 path

https://gmyankee.tistory.com/307



### 컨테이너에서 파일 추출하기

```bash
$ docker cp 991d72cb738b:/etc/xattr.conf ~/section3/ds-sa-docker
#docker cp <containerId>:/file/path/within/container /host/path/target
```



---



## Docker-Compose로 Docker Container 사용

Docker- Compose는 여러 개의 도커 컨테이너 관리할 수 있게 해준다.

서버를 구동하기 위해서는 중간에서 동적인 기능을 처리하는 웹 서버, 데이터를 관리하기 위한 데이터베이스, 이미지 및 파일 관리를 하기 위한 파일 서버 세 가지가 있어야 한다. 

Docker-Compose 정의 파일은 컨테이너들의 의존관계를 설정 할 수 있습니다. 그리고 정의 파일에 구성된 내용대로 컨테이너를 관리할 수 있습니다.

1. Web Server
2. Database
3. File Server

위 세 개의 도커 컨테이너를 관리하기 위한 방법으로, 또 세 가지를 생각할 수 있습니다

- 방금 전에 배운 docker run을 세 번해서 3 개의 컨테이너를 띄울 수도 있습니다. => 명령어 수행
- docker run에 대한 쉘 스크립트를 작성해서 관리할 수도 있습니다. => 스크립트화
- 여러 개의 도커 컨테이너를 관리하기 위해 docker-compose 를 사용하는 방법



#### Docker 명령어로 컨테이너 관리(스포: 별로임)

네트워크 생성

```bash
docker network create -d bridge my-net
```

데이터베이스 서버 컨테이너 실행

```bash
#-d 데몬(daemon)으로 실행하여 컨테이너의 출력결과를 확인
docker container run --rm -d --name mongodb --network=my-net jmuppala.mondodb
```

생성된 컨테이너 확인

```bash
docker container ps -a
```

웹 서버 컨테이너 실행

```bash
docker container run --rm --name express_server --network=my-net -p 3000:3000 jmuppala/node-server:latest ./wait-for-it.sh mongodb:27017 -- npm start
```



를 했을 때 





#### docker-compose를 이용하여 컨테이너 관리

docker-compose.yaml 혹은 docker-compose.yml (yaml이나 yml이나 같음)

- **하나의 docker-compose 에서 관리되는 컨테이너끼리는 동일한 docker network에서 구동됩니다.** (docker run 명령과 다르게 docker-compose 파일 안에서 기본 network가 사용됨)

```bash
MSYS_NO_PATHCONV=1 docker run -it --restart unless-stopped --name vol3 -e POSTGRES_DB=Chinook -e POSTGRES_USER=postgres_admin -e POSTGRES_PASSWORD=pg_pwd -v /var/lib/docker:/data postgres:12
```

```bash
MSYS_NO_PATHCONV=1 docker run -it --restart unless-stopped --name vol3 -e POSTGRES_DB=Chinook -e 
POSTGRES_USER=postgres_admin -e POSTGRES_PASSWORD=pg_pwd -v /var/lib/docker:/data/docker-entrypoint-initdb.d/init.sql postgres:12
```



docker-compose.yml 파일 만들기

```bash
touch docker-compose.yml
#현재 폴더에 docker-compose.yml 파일 생김
#여기에 docker-compose 버전
#사용할 컨테이너 지정해주기

# docker-compose 버전
version: '3.7' #최신 버전에서는 네트워크 버전 지정 안해도 같은 네트워크에서 실행됨

# 사용할 container
services: # services 문법이기 떄문에 변경하면 안됩니다
  express: # 변경 가능합니다
    # 웹 서버
    image : jmuppala/node-server
    container_name: express_server
    ports:
      - "3000:3000"
    depends_on:
    # 지정한 서비스가 실행되어야 해당 서비스를 실행
      - mongodb
    command : ["./wait-for-it.sh", "mongodb:27017","--","npm","start"]

  mongodb:
    container_name: first_database
    image : jmuppala/mongo-server
```

```bash
version: '3.7'

services:
  postgresql_new:  #(json에 정해진 host 이름이어야 함)
    image: postgres:12
    container_name: postgres_c

    environment:
      - POSTGRES_DB=dbname
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=pg_pwd
    volumes:
      - ~/anaconda3/a1/a2/init.sql:/docker-entrypoint-initdb.d/init.sql
      #/host_path:/containerpath
      #host_path를 ./init.sql로 해도 원래는 되는데...내 컴퓨터는 절대경로로 설정해야지 됨
    restart: unless-stopped

  pgadmin_new:
    image: dpage/pgadmin4:4
    container_name: pgadmin_c

    environment:
      - PGADMIN_DEFAULT_EMAIL=user1@google.com
      - PGADMIN_DEFAULT_PASSWORD=secret
    volumes:
      - ~/anaconda3/a1/a2/servers.json:/pgadmin4/servers.json
    ports:
      - "5050:80"
    restart: unless-stopped
    depends_on:
      - postgresql_new
```



docker-compose 컨테이너로 실행

```bash
docker-compose up
```

**주의사항**: docker-compose up을 한 번에 성공하지 못한다면 다음에 시도하기 전에 docker-compose down, volume 다 지우고, container 다 지우고 시도하기!



docker-compose 컨테이너 종료

```bash
docker-compose down
```



---

```bash
MSYS_NO_PATHCONV=1 docker run -it -d --env S3S1N2_Name=장보윤 --env github_username=terri1102 aibcontents/n312:1.0 python google_sheeet.py
```

docker run -it -d --env S3S1N2_Name=장보윤 --env github_username=terri1102 aibcontents/n312:1.0 python google_sheeet.py



docker-compose up freeze in window

https://github.com/docker/compose/issues/2176



-----



개념정리

인터프리터

프로그래밍 언어의 소스 코드를 바로 실행하는 컴퓨터 프로그램 또는 환경

컴파일러와 대비

번역단위: 행(줄), 목적프로그램: 생성하지 않음, 실행속도: 느림, 번역속도: 빠름:, 관련언어: python, BASIC<



컴파일러

고급언어로 작성된 프로그램들을 실행하는 데에는 두 가지 방법이 있다. 가장 일반적인 방법은 프로그램을 컴파일 하는 것이고, 다른 하나는 프로그램을 인터프리터에 통과시키는 방법이다. 인터프리터는 고급 명령어들을 중간 형태로 번역한 다음, 그것을 실행한다. 이와는 대조적으로, [컴파일러](https://ko.wikipedia.org/wiki/컴파일러)는 고급 명령어들을 직접 [기계어](https://ko.wikipedia.org/wiki/기계어)로 번역한다.



번역단위: 전체, 목적프로그램: 생성함, 실행속도: 빠름, 번역속도: 느림, 관련언어: ,C, JAVA



------

시간이 되면 docker image 만드는 법 알아보기

kubernates





-------

qna 시간



-미세먼지관련 애플리케이션 만든다면...

지금까지는 csv 파일로만 했지만

더 큰 데이터를 다루게 된다면

rdb 등 웹어플리케이션

nosql 관련 데이터베이스 등 사용할 수도

big data 플랫폼 까지도 연결해야 할 수 있음



CSV -> 데이터베이스 -> NoSQL  -> 빅데이터 플랫폼



도커 컨테이너 이용하면 환경설정에 대한 이슈가 훨씬 줄어듦



도커 기능(컨테이너 기술)

개발환경, 배포환경의 차이 때문에 컨테이너 기술 필요함

AWS에서 컴퓨터를 대여받는 다고 생각하면 컨테이너를 만들어서 띄우는 것이 유용함



1 어플리케이션 사용 측면에서의 도커

데이터베이스, 어플리케이션 구축하는데 사용

미세먼지 분석 보고서 어플리케이션



2 어플리케이션 배포 측면에서의 도커

서버에 올려서 배포할 때 AWS, GCP, heroku 등 서버에 올릴 때 어플리케이션 배포할 때 도커 많이 씀



3 클라우드 서비스 사용 측면에서의 도커

많은 회사들이 클라우드 서비스 많이 이용: 쿠버네티스, 베이그랜트



가상환경

개발 환경 설정

 

가상머신

어플리케이션 환경 설정



스프린트3의 목표

데이터를 모아서 어떻게 보여주나

파이썬 코드 작성 연습



Reference

https://deftkang.tistory.com/119

https://www.youtube.com/watch?v=hNdAQQeqkYU

https://coding-factory.tistory.com/303

https://futurecreator.github.io/2018/11/16/docker-container-basics/



---

레포 포크 후 클론 후 작업

problem1 요구사항에 맞게 명령어 입력

명령 맞게 실행하면 실행 메시지가 뜸&google sheet에 업데이트 됨

->1점



part2 두 개의 도커 이미지 활용

docker compose yml 파일 정의

docker-compose.yml이 빈 파일로 되어 있음

문제에 맞게 작성 후 docker-compose up하면 

컨테이너가 2개 띄워짐

localhost 가면 나옴

설명 페이지에 나온 아이디 비번 입력하면 됨



part3 

2번 과제의 컨테이너를 끄지 말고 컨테이너 안에 있는 파일을

로컬로 가져오기 (postgres 이용)

이를 과제 제출 폼으로 제출



컨테이너를 로컬로 뽑아서 과제 제출..



docker file 부분은 제외하고 정보를 습득하기. 설명 안 했으니까..



submit하고 pr 날려야 함!



도커는 앞으로도 다른 세션에서 계속 쓰게 될 것임



aws나 lambda 같은 서버에 파일 코드 올리면 서버 구동됨



공식 다큐먼트 볼 때

docker run option image command

모든 옵션을 여러 개 넣을 수 있나?

보편적으로는 여러 개 쓸 수 있고, 하나만 쓸 수 있으면 doc에 나와있음

---

컨테이너 최초 선언할 때 

docker exec -it container_name /bin/bash  #/bin/bash 는 쓸모가 있을 때만 쓰기. 무조건 들어갈 수 있는 거 아님

run /bin/bash   #run할 때 bin/bash 



docker run image_name 명령어



명령어를 안 주면 exited됨



pg admin 웹 사이트 

숫자만 있는건 localhost



part2

docker-compose.yaml 이 작성된 경로에서 docker-coimpose down

아니면 따로따로 docker stop



yml파일 수정

오늘도 1번풀고 결과를 commit한다음 pr날려야되는건가요?

안녕하세요 여러분. 제가 2시간동안 이거로 삽질했는데, 너무 눈물이 나서 올려요...

혹시 localhost:5050 들어가시고, Chinook 로그인하려는데  could not translate host name "postgresql_codestates" to address 오류가 발생하시면
servers.json에 있는 host가 어떻게 적용되어야하는지 찾아보시면 금방 해결하실겁니다 ㅠㅠ

docker container run -e 환경변수 [images] [command] 이런 식으로요!



그거 제가 그랬는데 volume탭으로 마운트가 잘 안되어서 그런거였어요!!





2번에서 db 안 뜸

volume탭으로 마운트가 잘 안되어서 그런거였어요!!

volume에서 로컬에 있는 server.json과 init.sql이 컨테이너에 복사해주셔야해요..!!

postgresql의 volume의 경우에는 로컬의 init.sql 파일 주소를 서버의 /docker-entrypoint-initdb.d/init.sql 요 주소와 연결해주셔야 하는 거고, pgadmin4은 로컬의 servers.json 주소를 /pgadmin4/servers.json 이 주소와 연결해주시도록 수정해주시면 될거에요!



-명령어

CMD ["python", "/src/google_sheeet.py"]



---

경로 설정

https://github.com/moby/moby/issues/24029

https://stackoverflow.com/questions/7250130/how-to-stop-mingw-and-msys-from-mangling-path-names-given-at-the-command-line#34386471

https://github.com/git-for-windows/git/issues/577

path conversion https://stackoverflow.com/questions/7250130/how-to-stop-mingw-and-msys-from-mangling-path-names-given-at-the-command-line#34386471

https://gist.github.com/borekb/cb1536a3685ca6fc0ad9a028e6a959e3

https://stackoverflow.com/questions/48427366/docker-build-command-add-c-program-files-git-to-the-path-passed-as-build-argu

bash profile에 경로 추가 

https://m.blog.naver.com/PostView.nhn?blogId=jsky10503&logNo=220727987865&proxyReferer=https:%2F%2Fwww.google.com%2F

리눅스 path

https://jacking75.github.io/OS_linux_path/

https://stackoverflow.com/questions/42250222/where-is-docker-image-location-in-windows-10



```shell
cd /var/lib/docker/overlay2/585...9eb/
```

internal structure of docker root folder

```shell
 ls -la /var/lib/docker
```

docker image store

/var/lib/docker/overlay2

```shell
 "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/585...9eb/diff:
                             /var/lib/docker/overlay2/585...9eb/diff",
                "MergedDir": "/var/lib/docker/overlay2/585...9eb/merged",
                "UpperDir": "/var/lib/docker/overlay2/585...9eb/diff",
                "WorkDir": "/var/lib/docker/overlay2/585...9eb/work"
            },
```

https://www.freecodecamp.org/news/where-are-docker-images-stored-docker-container-paths-explained/

```shell
#docker volume
$ docker run --name nginx_container -v /var/log nginx
```

https://forums.docker.com/t/where-are-images-stored/9794/23

윈도우 run 문제

https://docs.microsoft.com/ko-kr/virtualization/windowscontainers/manage-docker/manage-windows-dockerfile

volume 문제

https://github.com/docker/toolbox/issues/673#issuecomment-355275054

https://www.daleseo.com/docker-volumes-bind-mounts/

https://docs.docker.com/storage/volumes/