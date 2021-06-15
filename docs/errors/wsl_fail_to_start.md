---
layout: default
title: VS Code Server for WSL closed unexpectedly 오류
date: 2021-06-01
parent: Errors
comments: true
nav_order: 1


---



VS Code에서 우분투를 오랜만에 실행하려니 시작부터 에러가 떴다. 도커도 켜져 있는데 왜 안 될까 찾아봤는데 연결이 잘못 되어서 그런 것이었다.

![ubuntu_error](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/errors/ubuntu_errors.jpg?raw=true)

```bash
[2021-06-01 11:18:20.215] Resolving wsl+docker-desktop-data, resolveAttempt: 1
[2021-06-01 11:18:20.348] Starting VS Code Server inside WSL (docker-desktop-data)
[2021-06-01 11:18:20.348] Extension version: 0.56.4, Windows build: 19042. Multi distro support: available. WSL path support: enabled
[2021-06-01 11:18:20.349] No shell environment set or found for current distro.
[2021-06-01 11:18:20.673] Probing if server is already installed: C:\WINDOWS\System32\wsl.exe -d docker-desktop-data -e sh -c "[ -d ~/.vscode-server/bin/054a9295330880ed74ceaedda236253b4f39a335 ] && printf found || ([ -f /etc/alpine-release ] && printf alpine-; uname -m)"
[2021-06-01 11:18:20.942] Unable to detect if server is already installed: Error: Command failed: C:\WINDOWS\System32\wsl.exe -d docker-desktop-data -e sh -c "[ -d ~/.vscode-server/bin/054a9295330880ed74ceaedda236253b4f39a335 ] && printf found || ([ -f /etc/alpine-release ] && printf alpine-; uname -m)"
[2021-06-01 11:18:20.942] 
[2021-06-01 11:18:20.943] Launching C:\WINDOWS\System32\wsl.exe -d docker-desktop-data sh -c '"$VSCODE_WSL_EXT_LOCATION/scripts/wslServer.sh" 054a9295330880ed74ceaedda236253b4f39a335 stable .vscode-server 0  '}
[2021-06-01 11:18:21.132] VS Code Server for WSL closed unexpectedly.
[2021-06-01 11:18:21.132] For help with startup problems, go to
[2021-06-01 11:18:21.132] https://code.visualstudio.com/docs/remote/troubleshooting#_wsl-tips
[2021-06-01 11:18:21.145] WSL Daemon exited with code 0
```



### 원인

현재 wsl 디폴트가 docker-desktop-data로 설정되어 있어서 이를 Ubuntu로 바꿔줘야한다. powershell에서 wslconfig의 파일들을 확인했을 때, Ubuntu가 아닌 것을 확인해보자.

```powershell
#powershell
wslconfig /L
```

![docker-desktop-default](https://github.com/terri1102/terri1102.github.io/blob/master/assets/images/errors/docker_desktop_default.jpg?raw=true)

### 해결

```powershell
#powershell
wslconfig /setdefault Ubuntu-20.04
```



이제 다시 New WSL Window를 열면 제대로 열린다!

![image-20210602073654548](C:\Users\Boyoon Jang\AppData\Roaming\Typora\typora-user-images\image-20210602073654548.png)

## Reference

https://stackoverflow.com/questions/44049070/bash-on-ubuntu-on-windows-not-starting/46147036

[WSL2에서 ip 주소 확인 및 도커 데스크탑으로 서버 실행하기](https://www.44bits.io/ko/post/wsl2-install-and-basic-usage)