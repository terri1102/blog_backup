---
layout: default
title: git 명령어 및 오류 정리
parent: Web Development
nav_order: 2
---
# git 명령어 정리

```bash
$ git init #폴더가 git의 관리 하에 들어감

$ git config --global user.name "내 이름"

$ git config --global user.email "내 메일주소"

$ git status # git정보 확인 현재 시점 저장

#add-commit-push: 보통 이렇게 세 가지 많이 씀
$ git add -A
$ git add --all

$ git commit -m "First Commit"

$ git push -u origin main

$ git log # 여태까지의 로그 표시
```


vi editor 빠져나오기
```bash
: + q 눌러서 빠져나오기
```


과거로 돌아가기

```bash

#1. 과감하게 돌아갈 과거 이후의 행적 복원 불가한 방법

#git log의 일련번호 앞 6자리 복사해서 적고

git reset 000000 --hard



#2. 소심하게..revert

#취소할 시점! 찾기

git revert 00000 --revert

```

### 브랜치 관리
<br>

브랜치 생성
```bash
$ git branch my-idea
```


브랜치로 들어가기
```bash
$ git checkout my-idea
```


master 브랜치로 돌아가기
```bash
git checkout master

```

my-idea에서 새 브랜치 생성
```bash

#my-idea로 checkout된 상태에서 
$ git branch my-another-idea
```


merge 하기
```bash

#1. 마스터 브랜치로 돌아오기 
$ git checkout master

#(파생된 브랜치는 마스터 브랜치의 변화가 이미 적용되어 있음)

#2.
git merge my-another-idea
```


여러 분기에서의 작업내역 보기
```bash
git log --graph --all --decorate
```

두 브랜치의 같은 파일에 같은 수정을 한다면 conflict

rebase: 분기들이 한 줄로 합쳐짐

브랜치 삭제
```bash
git branch -D old-branch
```
<br>

## 오류

```bash
$ git push origin git-merge
To https://github.com/terri1102/ds-sa-simple-git-flow.git
 ! [rejected]        git-merge -> git-merge (non-fast-forward)
error: failed to push some refs to 'https://github.com/terri1102/ds-sa-simple-git-flow.git'      
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
(n311a)
```
Git Bash End 해결하기
```bash
q 
```
설명: 화면 크기보다 많은 내용 표시해야 할 때 END를 누르면 한 줄 씩 더 나옴. q를 누르면 여기서 벗어남


참고
```bash
:wq #저장하고 종료

:q  #편집 종료
```

윈도우와 맥, 리눅스 호환 문제
- 윈도우의 경우
```bash
warning: LF will be replaced by CRLF in docs/web_development.md.
The file will have its original line endings in your working directory
```
```bash
$ git config --global core.autocrlf true 
#해당 프로젝트에만 적용하고 싶으면 --global 부분을 빼기
```
# 해결 못한 build fail
github 블로그의 테마를 바꾸고 또 다른 시련이 찾아왔는데...
git bash에서는 오류없이 push가 되는데 자꾸 build fail이라면서 이메일이 온다ㅜㅜ
css나 js 파일은 push에 성공했지만, Run failed: Master branch CI - master 라고 한다.
근데 블로그 사이트에 접속하면 commit하고 push한 내용들이 제대로 보인다.
아마 로컬에서 bundle exec jekyll serve 하면 build에 실패할 것 같긴 한데, 또 오류나면 config 설정부터 다시 시작해야 할 까봐 못하고 있다...
https://www.reddit.com/r/devops/comments/efmtep/silly_question_what_exactly_should_i_do_if_my/