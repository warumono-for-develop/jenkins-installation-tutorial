[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![Apache-2.0 License][license-shield]][license-url]
<!-- [![license](https://img.shields.io/github/license/:user/:repo.svg)](LICENSE) -->

<p align="center">
  <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial">
    <img src="https://github.com/warumono-for-develop/default/blob/master/logos/warumono-logo-492x500.png?raw=true" alt="Logo" width="292" height="300">
  </a>

  <h1 align="center">Jenkins Installation Tutorial</h1>

  <p align="center">
    Jenkins 프로그램 설치 관련 지침서
    <br />
    <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial">View Tutorial</a>
    ·
    <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial/issues">Report Bug</a>
    ·
    <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial/issues">Request Feature</a>
  </p>
</p>



## Table of Contents 목차

- [Getting Started](#getting-started)
- [Install](#install)
- [Plugin](#plugin)
- [Usage](#usage)
- [FAQ](#faq)
- [Contact](#contact)
- [Acknowledgements](#acknowledgements)
- [License](#license)



## Getting Started
<!--
<details> 
  <summary>Collapse</summary>

Expanded

---
</details>
-->

### Requirements 요구사항

본 작업에 앞서 관련 대상 프로그램, 데이터 및 서버 등은 반드시 백업 완료 후 작업할 것을 권장   
본 작업을 작업하는 시점, 프로그램 버전 및 환경 등이 달라 설명 글의 진행 절차 또는 결과가 다소 다르거나 생략 또는 추가 작업이 있을 수 있음    
잘못된 용어 또는 정보가 다분히 포함되어 있는 비전문 지식을 바탕으로 설명되어 있으므로 공식 사이트 등에서 관련 정보를 미리 숙지하고 작업할 것을 권장

### About The Tutorial 지침서에 대하여

<!--  - AWS EC2 를 사용한다면, EC2 내부의 리소스 관리는 물론 [CLI](https://namu.wiki/w/CLI) 이용 등은 필수적인 작업 중 일부   -->
  - 번거롭고 반복적인 작업으로 인해 효율성이 떨어지는 문제를 조금이나마 해소하고자 설치   
  - 보다 나은 작업환경을 만들어 보겠다고 생소한 용어들과 이해할 수 없는 원리 그리고 설정 등으로 인해 몇 차례 포기한 경험이 있지만    
이번에야말로 완료 해보겠다는 생각으로 여러번의 실패 끝에 나름 성공한 결과를 토대로 설정 정보 및 진행순서 등을 기록한 지침서

### Prerequisites 전제조건

#### Required 필수사항

  - [x] [AWS](https://aws.amazon.com/ko/) EC2 Instance    
    \[Free tier\] Ubuntu Server 18.04 LTS (HVM), SSD Volume Type

  - [x] [Docker](https://www.docker.com/)   
    Docker version 19.03.6    
    [Docker Installation Tutorial](https://github.com/warumono-for-develop/docker-installation-tutorial)

#### Optional 선택사항

  - [ ] [Jupyter Notebook](https://jupyter.org/)    
    [Jupyter Notebook Installation Tutorial](https://github.com/warumono-for-develop/jupyter-notebook-installation-tutorial)



## Install

### Step 1

Download Jenkins

Jenkins 설치 파일도 존재하나, 본 지침서에서는 Docker 이미지를 사용하여 설치하는 방법으로 설명   
Jenkins 공식 사이트에서 제공하는 이미지의 이름은 `jenkins` 이나, ~~업데이트 및 관리가 지속적으로 되고 있지 않다는 소문 (?)이 있으니~~    
다른 이미지 이름 **jenkins/jenkins** 이미지를 다운로드 (docker pull) 할 것을 권장

> docker pull jenkins/jenkins

```sh
your-terminal> docker search jenkins
NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
jenkins                                Official Jenkins Docker image                   4666                \[OK\]                
jenkins/jenkins                        The leading open source automation server       1920                                   
...
your-terminal> docker pull jenkins/jenkins
```

### Step 2

Install Jenkins

> docker run -d -p {your-jenkins-host-port}:{your-jenkins-port} -v {your-host-directory-path}:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins

`{your-host-directory-path}` 는 Jenkins 의 설정 정보 등이 로컬 (또는 Host) 디렉토리로 **백업**의 의미로 연결해 놓은 것이라고 이해하면 됨

```sh
your-terminal> docker run -d -p 8090:8080 -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins
```

### Step 3

Install Docker in Jenkins

기존 Docker ([Docker Installation Tutorial](https://github.com/warumono-for-develop/docker-installation-tutorial) 에 의해 설치한 Docker) 는 **`로컬` 또는 `호스트 서버` 에 설치되어 있는 Docker**    
지금 설치할 Docker 는 **`Jenkins 내부` 에 설치하는 Docker** 이니 혼돈하지 않아야 함

Docker 설치 Shell Script 파일을 다운로드 후, 해당 Shell Script 를 실행하여 **Docker 다운로드 및 설치, 설정을 한번에 일괄 작업**   
[Docker](https://www.docker.com/) 공식 [Github](https://github.com/docker/docker-install) 참조

Docker 명령어를 사용하여 **Jenkins 내부에 접속한 상태**에서 진행    
작업 완료 후, `exit` 을 입력하여 Jenkins 로 부터 나옴    
[Docker Installation Tutorial](https://github.com/warumono-for-develop/spring-boot-github-docker-jenkins-ci-cd-tutorial/blob/master/README.md) 의 [FAQ](https://github.com/warumono-for-develop/spring-boot-github-docker-jenkins-ci-cd-tutorial#faq) \> `Docker 명령어 간단 사용 방법` \> `About container` \> `컨테이너 내부 접속` 참조

> docker exec -it {your-docker-container-id} /bin/bash    
> curl -fsSL https://get.docker.com -o get-docker.sh    
> sh get-docker.sh

```sh
your-terminal> docker ps -a
CONTAINER ID   IMAGE                                     COMMAND                  CREATED        STATUS       PORTS                               NAMES
725486c2a607   jenkins/jenkins                           "/sbin/tini -- /usr/…"   21 hours ago   Up 1 hours   50000/tcp, 0.0.0.0:8090->8080/tcp   adoring_ptolemy
your-terminal> docker exec -it 725486c2a607 /bin/bash
jenkins-terminal> 
jenkins-terminal> cd ~
jenkins-terminal> curl -fsSL https://get.docker.com -o get-docker.sh
jenkins-terminal> ls
get-docker.sh
jenkins-terminal> sh get-docker.sh
...
jenkins-terminal> docker --version
Docker version 19.03.6, build 369ce74a3c
jenkins-terminal> exit
exit
your-terminal> 
```

### Step 4

Configure Jenkins

#### Jenkins 접속

웹 브라우져를 실행하여 URL 입력 창에 `{your-host-ip}:{your-jenkins-host-port}` 를 입력하여 Jenkin 접속

#### Unlock Jenins

Jenkins 설치 후 최초 화면에서 비밀번호 (Jenkins 계정 비밀번호 아님) 가 필요하며, 이는 Docker 의 로그 정보에서 찾을 수 있음   
*해당 비밀번호는 설치 시 Jenkins 내부 파일 (Jenkins 최초 화면에 표시되어 있는 경로 `{your-jenkins-home-path}/secrets/initialAdminPassword`) 에 저장되어 있음*

Docker 의 로그 정보 확인  

> docker logs {your-jenkins-container-id}

> ...   
> \<your-jenkins-password\>   
> ...

```sh
your-terminal> docker logs 725486c2a607
Running from: /usr/share/jenkins/jenkins.war
webroot: EnvVars.masterEnvVars.get("JENKINS_HOME")
...

*************************************************************
*************************************************************
*************************************************************

Jenkins initial setup is required. An admin user has been created and a password generated.
Please use the following password to proceed to installation:

016b9b01454f418caf2dab842474b351

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword

*************************************************************
*************************************************************
*************************************************************

...
your-terminal> 
```

<details> 
  <summary>Jenkins 내부에서 비밀번호 찾기</summary>

Docker jenkins 이미지를 사용하여 Jenkins 를 설치하는 경우에는 Docker container 즉, Jenkins container 내부로 접속하여야 내부 파일에 접근 가능   

#### Docker 명령어를 사용하여 Jenkins 내부 접속

> docker exec -it {your-jenkins-container-id} /bin/bash

```sh
your-terminal> docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS                               NAMES
367932a46403        jenkins             "/bin/tini -- /usr/l…"   1 hours ago         Up 45 hours              0.0.0.0:8080->8080/tcp, 50000/tcp   funny_golick
...
your-terminal> docker exec -it 367932a46403 /bin/bash
jenkins-terminal> 
```

Jenkins 설치 후 최초 화면에서 `Administrator password` 입력 창에 {your-jenkins-password} 를 복사하여 붙여넣기

#### VIM 파일 내용 보기 명령어로 비밀번호 찾기

> cat {your-jenkins-home-path}/secrets/initialAdminPassword

> <your-jenkins-administrator-password>

```sh
jenkins-terminal> cat /var/jenkins_home/secrets/initialAdminPassword
016b9b01454f418caf2dab842474b351
```

---
</details>

#### Customize Jenkins

`Install suggested plugins` 선택   
`Getting Started` 화면으로 이동되어 자동으로 plugin 설치    
*다소 시간이 걸리므로 설치 완료 될때까지 기다림*

#### Create First Admin User

사용자 계정 정보를 입력하여 저장 (`Save and Finish`)

> Username &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{your-jenkins-username}   
> Password &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; {your-jenkins-password}   
> Full name &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; {your-jenkins-full-name}   
> E-mail address &nbsp; {your-jenkins-email-address}
 &nbsp;
```sh
Username       warumono
Password       ********************
Full name      warumono-for-jenkins
E-mail address warumono.for.develop@gmail.com
```

#### Instance Configuration

`Jenkins URL: http://<your-host-ip>:8080/` 기본 설정 값으로 설정   

> http://\<your-host-ip\>:8080

\<your-host-ip\> 에는 AWS EC2 인스턴스의 상세정보 중 **IPv4 Public IP** 가 자동 설정 됨

```sh
http://54.081.311.162:8080
```

#### Jenkins is ready!

`Start using Jenkins` 버튼 클릭

정상적으로 설정이 완료되었다면, `Jenkins Dahsboard` 화면이 나타남

### Jenkis 원격 빌드 설정

Jenkins 에 접속하지 않고 `curl` 명령어 또는 `웹 브라우져` 를 사용하여 원격으로 빌드 작업을 수행할 수 있는 기능

#### Jenkins 원격 빌드 활성화 & Authentication Token 생성

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; job 목록 중 원격 빌드 대상의 `Name` 클릭 &nbsp; > &nbsp; `Project {auto-jenkins-job-name}` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Configure` 선택 &nbsp; > &nbsp; `General` 탭 화면 &nbsp; > &nbsp; `Build Trigger` 영역   
`Trigger builds remotely (e.g., from scripts)` **체크박스 활성화**
체크박스 활성화에 따라 나타나는 `Authentication Token` 입력 창에 {your-jenkins-job-build-authentication-token} 입력 및 메모    
`{your-jenkins-job-build-authentication-token}` 는 **비밀번호 처럼 사용하게 될 문자열** 이므로 암호화된 코드 등으로 가독성이 떨어지는 문자열 사용을 권장

> {your-jenkins-job-build-authentication-token}

```sh
hello-jenkins-build-authentication-token
```

#### CSRF 비활성화

~~`Jenkins Dashboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Manage Jenkins` 클릭 &nbsp; > &nbsp; 설치되어 있는 Plugin 목록 중 `Configure Global Security` 선택 &nbsp; > &nbsp; 화면 중간 부분 `Prevent Cross Site Request Forgery exploits` **체크박스 비활성화**~~   
*본 지침서 작성 초기, Docker 이미지 `jenkins/jenkins` 가 아닌 `jenkins` 를 설치하여 사용할 경우에는 CSRF 비활성화 설정을 하였지만 이 후, `jenkins/jenkins` 를 설치하여 사용해보니 CSRF 비활성화 설정은 기본으로 되어 있는 것인지(?) 해당 항목은 없는 것을 확인하고, 혹 Docker 이미지 jenkins 를 설치하여 사용하는 경우에는 설정해야하므로 설명에는 남겨 놓음*

> - [ ] Prevent Cross Site Request Forgery exploits

```sh
- [ ] Prevent Cross Site Request Forgery exploits
```

#### API Token 생성

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Manage Users` 클릭 &nbsp; > &nbsp; `Users` 화면 &nbsp; > &nbsp; 사용자 목록 중 임의의 사용자 오른쪽 `톱니바퀴 버튼` 클릭 &nbsp; > &nbsp; 사용자 상세 정보 화면 &nbsp; > &nbsp; `API Token` 영역 &nbsp; > &nbsp; `Show API Token...` 클릭 &nbsp; > &nbsp; 자동 생성된 `API Token` 값 {your-jenkins-access-api-token} 메모

> \<your-jenkins-access-api-token\>

```sh
a3t32p94xe400rr29fb34abc41doofee
```


## Plugin

Jenkins 의 job 빌드 결과를 실시간 알림으로 확인할 수 있는 Plugin

### On Jenkins

#### Install the Plugin in Jenkins

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Manage Jenkins` 클릭 &nbsp; > &nbsp; `Manage Plugins` 선택 &nbsp; > &nbsp; `Manage Plugins` 화면에서 `Available 탭` 클릭 &nbsp; > &nbsp; `Filter 입력 창` 에 `Websocket` 입력 &nbsp; > &nbsp; 조회 목록 중 - [x] `Websocket Notifier` 체크박스 활성화 &nbsp; > &nbsp; `Download now and install after restart` 버튼 클릭

> [Websocket Notifier](https://plugins.jenkins.io/websocket/)

```sh
Websocket Notifier
```

Websocket Notifier 가 정상적으로 설치 완료되었다면 Jenkins 는 재가동 되었으므로 다시 로그인

#### Configure Websocket Notifier

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Manage Jenkins` 클릭 &nbsp; > &nbsp; `Configure Systems` 선택 &nbsp; > &nbsp; `Configure Systems` 화면에서 맨 아래의 `Websocket Notifier` 영역

> Port {your-jenkins-websocket-port}   
> \[x\] Enable Websocket pings to keep connections alive    
> Ping interval	{your-jenkins-ping-interval}

기본 값 그대로 설정    
*사용자의 환경에 맞도록 설정 가능*

```
Port 8081
Use status format [ ]
[x] Enable Websocket pings to keep connections alive
Ping interval	20
```

#### Configure job

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; job 목록 중 원격 빌드 대상의 `Name` 클릭 &nbsp; > &nbsp; `Project {auto-jenkins-job-name}` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 중 `Configure` 선택 &nbsp; > &nbsp; `General` 탭 화면 &nbsp; > &nbsp; `Post-build Actions` 영역

`Add post-build action` 버튼을 선택하여 Drop Down 메뉴 중 `Websocket Notifier` 선택

### On Chrome

[Chrome 웹 브라우저](https://www.google.com/intl/ko/chrome/) 의 확장 프로그램을 사용하게 되면 간단 설치 및 설정만으로 사용 가능    
다만 모바일 등에서는 Chrome 웹 브라우저 확장 프로그램을 사용할 수 없게 되어 사용자 또는 관련자들이 이동중에 실시간으로 알림을 받을 수 없는 단점이 있음    
이러한 단점을 보완할 수 있는 [Slack](https://slack.com/intl/en-kr/) 등의 프로그램으로 이동중에도 실시간으로 알림을 받을 수 있음

#### Install the Extension in Chrome

[Chrome 웹 브라우저](https://www.google.com/intl/ko/chrome/) 의 확장 프로그램을 사용

`Chrome Extension` 으로 이동하여 `jenkins` 검색어로 조회하여 결과 목록 중 `Yet Another Jenkins Notifier` 확장 프로그램 선택 설치   
[Chrome web store - Yet Another Jenkins Notifier](https://chrome.google.com/webstore/detail/yet-another-jenkins-notif/cimdjdaglanfkpfpoemjkfkmjgkmahpg?utm_source=chrome-ntp-icon)

[GitHub - Yet Another Jenkins Notifier](https://github.com/ggirou/yet-another-jenkins-notifier) 참조하길 권장



## Usage

새로운 빌드 작업 (job) 생성, 설정 및 빌드 그리고 결과 확인

### Create new job

새로운 빌드 작업 생성

`Jenkins Dahsboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 `New Item` 선택 (또는, 생성된 job 이 없는 경우 화면 중앙에 `Create new job` 을 클릭하여도 동일함)    
`Enter an item name` 입력 창에 빌드 작업 이름 입력   

> {your-jenkins-job-name}

```sh
hello-jenkins
```

`Freestyle project` 선택    
`OK` 클릭

### Configure Build

빌드 설정

`General` 탭 화면 &nbsp; > &nbsp; `Build` 섹션 &nbsp; > &nbsp; `Add build step` Drop Down 메뉴 선택 &nbsp; > &nbsp; `Execute shell` 선택    
`Command` 입력 창이 나타나고, 입력 창에는 Jenkins 빌드 스크립트를 입력    

아주 간단하게 빌드 진행 로그 내용에 {your-comment} 가 나오는 스크립트를 작성     

> echo "{your-comment}"

```sh
echo "Hello Jenkins!"
```

`Save` 클릭

[Pligin](#plugin) 절차에 따라 [Yet Another Jenkins Notifier](https://chrome.google.com/webstore/detail/yet-another-jenkins-notif/cimdjdaglanfkpfpoemjkfkmjgkmahpg?utm_source=chrome-ntp-icon) 를 설치한 경우, Chrome 웹 브라우저 확장 프로그램 아이콘 바에서 `Yet Another Jenkins Notifier` 버튼 (Jenkins 공식 로고와 같음) 을 클릭하여 `Url` 입력 창에 `job` URL 을 입력하여 ` + ` 클릭    

`job` URL

> {your-host-ip}:{your-jenkins-host-port}/job/{your-jenkins-job-name}

```sh
http://54.081.311.162:8090/job/hello-jenkins
```

### Build

`Jenkins Dashboard` 화면 왼쪽 메뉴 아래 `Build History` 영역에는 빌드 진행 횟수와 과정 및 결과 이력이 `#\<your-jenkins-job-build-index\>` (빌드 진행 횟수) 와 `<your-jenkins-job-build-date>` (빌드 진행 날짜) 형식으로 나타남    
빌드 결과에 따라 실패는 `빨간색`, 정상은 `파란색` 으로 표시됨

> #\<your-jenkins-job-build-index\>   
> &nbsp; &nbsp; \<your-jenkins-job-build-date\>

```sh
#1
  Feb 29, 2020 11:06 PM KST
``` 

### Log

빌드 관련 로그 확인

`Jenkins Dashboard` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 아래 `Build History` 영역 `#\<your-jenkins-job-build-index\>` 클릭 &nbsp; > &nbsp; `Project {your-jenkins-job-name}` 화면 &nbsp; > &nbsp; 왼쪽 메뉴 `Build Now` 클릭 &nbsp; > &nbsp; `Build History` 영역 현재 빌드 `#\<your-jenkins-job-build-index\>` 클릭 &nbsp; > &nbsp; 상세 화면 &nbsp; > &nbsp; 왼쪽 메뉴 `Console Output` 클릭

로그 마지막 줄 **Finished: \<your-jenkins-job-build-result\>** 부분이 빌드 결과    
본 지침서에는 빌드 실패에 관한 설명은 없으니, 빌드 실패 사용자는 로그 정보에서 오류 원인을 확인하여 정상적으로 빌드가 되도록 해야 함

> Started by user \<your-jenkins-username\>   
> ...   
> \<your-comment\>    
> Finished: \<your-jenkins-job-build-result\>

```sh
Started by user warumono
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/hello-jenkins
[hello-jenkins] $ /bin/sh -xe /tmp/jenkins7074309935194145387.sh
+ echo Hello Jenkins!
Hello Jenkins!
Finished: SUCCESS
```

[Pligin](#plugin) 절차에 따라 [Yet Another Jenkins Notifier](https://chrome.google.com/webstore/detail/yet-another-jenkins-notif/cimdjdaglanfkpfpoemjkfkmjgkmahpg?utm_source=chrome-ntp-icon) 를 설치한 경우, Chrome 웹 브라우저로부터 알림을 확인할 수 있음  

본 지침서에서는 아주 간단한 빌드 작업을 진행하였지만 일반적으로 개발 과정 중 순차적으로 진행되는 리소스 형상관리, 빌드 및 배포 작업을 번거롭게 개발자가 직접 하나 하나 작업하던 것을 Jenkins 빌드 작업으로 일괄 처리할 수 있음    
Github 과 연동하여 Github repository 를 다운로드 (git pull) 하여 해당 repository 를 빌드 후 서버에 배포 한다거나,    
Docker Hub 의 image 를 다운로드 (docker pull) 하여 빌드 (docker build) 후 실행 (docker run) 하는 작업 진행을 스크립트 작성 및 Plugin 등을 사용하여 번거로운 작업을 일괄적으로 처리할 수 있음

### Jenkin 원격 빌드

> curl -X POST http://{your-jenkins-username}:{your-jenkins-access-api-token}@{your-host-ip}:{your-jenkins-host-port}/job/{your-jenkins-job-name}/build?token={your-jenkins-job-build-authentication-token}

|변수|설명|참조|비고|
|---|---|---|---|
|your-jenkins-username|Jenkins 사용자 Username|[Create First Admin User](#create-first-admin-user)|warumono|
|your-jenkins-access-api-token|Jenkins API Token|[API Token 생성](#api-token-생성)|a3t32p94xe400rr29fb34abc41doofee|
|your-host-ip|Jenkins 호스트 서버 IP|[Instance Configuration](#instance-configuration)|54.081.311.162|
|your-jenkins-host-port|Jenkins 호스트 서버 PORT|[Instance Configuration](#instance-configuration)|8080|
|your-jenkins-job-name|빌드 대상 Jenkins job Name|[Create new job](#create-new-job)|hello-jenkins|
|your-jenkins-job-build-authentication-token|Jenkins Authentication Token|[Jenkins 원격 빌드 활성화 & Authentication Token 생성](#jenkins-원격-빌드-활성화-&-authentication-token-생성)|hello-jenkins-build-authentication-token|

```sh
your-terminal> curl -X POST http://warumono:a3t32p94xe400rr29fb34abc41doofee@54.081.311.162:8080/job/hello-jenkins/build?token=hello-jenkins-build-authentication-token
```

정상적으로 원격 빌드 작업을 수행하였다면, [Log](#log) 설명과 같은 절차로 빌드 결과 확인    
단, [Log](#log) 설명 내의 로그 정보 중 **첫번째 줄의 정보**가 다음과 같은 차이점이 있음

Jenkins 외부에서 `원격 빌드` 한 경우 **Started by `remote host` \<your-remote-ip\>**   
Jenkins 내부에서 `직접 빌드` 한 경우 **Started by `user` \<your-jenkins-username\>**

```sh
Started by remote host 54.081.311.162
Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/hello-jenkins
[{hello-jenkins] $ /bin/sh -xe /tmp/jenkins2767083941166045842.sh
+ echo Hello Jenkins!
Hello Jenkins!
Finished: SUCCESS
```



## FAQ

<details>
  <summary>참조 (Reference) vs 예시 (Sample)</summary>

본 지침서는 작성자의 기준으로 설명하다보니 모든 작업의 내용대로 복사하여 사용할 경우 의도하지 않은 과정이나 결과를 도래할 수 있기에,   
사용자 자신의 환경에 맞추거나 또는 원하는 내용대로 작업할 수 있도록 설명하기 위하여 변수 형태로 사용


### 사용자 참조 내용 영역

본 지침서에 따라 작업하는 *사용자*가 아래와 같은 영역의 내용을 참조하여 **반드시 해야하는 작업**을 나열

> mkdir {your-directory-name}   
> cd {your-directory-name}    
> ls

> {your-directory-name}

> vi {your-file-name}.sh

### 작성자 예시 내용 영역

아래와 같은 영역에 *작성자*가 `사용자 참조 내용 영역`에 따라 직접 작업한 내용을 **예시**로 설명   
`사용자 참조 내용 영역` 외로 추가 작업 내용도 포함되어 설명

```sh
> cd ~
> mkdir mydir
> ls
mydir
> cd mydir
> pwd
/home/ubuntu/mydir
> sudo vi myfile.sh
```

---
</details>

<details>
  <summary>User variable 사용자 변수</summary>

본 지침서는 작성자의 기준으로 설명하다보니 모든 작업의 내용대로 복사하여 사용할 경우 의도하지 않은 과정이나 결과를 도래할 수 있기에,   
사용자 자신의 환경에 맞추거나 또는 원하는 내용대로 작업할 수 있도록 설명하기 위하여 변수 형태로 사용


- `{variable-name}` 사용자가 직접 입력해야하는 부분. http://`{your-host-ip}`:8080 &nbsp; - - - > &nbsp; http://`123.456.789.0`:8080    
- `<variable-name>` 사용자의 어떠한 행위에 따른 제공되는 결과 부분. `<your-cert-key-name>`.pem &nbsp; - - - > &nbsp; `my_cert`.pem

---
</details>

<details> 
  <summary>VIM 에디터 간단 사용 방법</summary>

본 지침서는 Linux 계열의 OS 기준으로 설명하다보니 VIM 에디터의 vi 명령어로 파일 내용을 편집하는 경우가 빈번하여 간단하게 설명   
사용자 자신의 환경과 맞지 않을 경우에는 프로그램 설치 등으로 이에 준하는 환경에서 작업할 것을 권장


### VIM 에디터 실행

vi 명령어 이용

> vi {your-file-name}

```sh
your-terminal> vi README.md
```

이 파일명의 파일이 존재하면 해당 파일을 열어서 보여주고 (아직 읽기만 가능한 상태) "{your-file-name}" file info aaa
존재하지 않을 경우 해당 파일명과 동일한 이름을 갖는 새 파일을 열음 (아직은 저장되지않은상태 ) "{your-file-name}" [New File]
이 상태에서 i또는 a또는 o를 누르면 ex 명령 부분
내용 편집 후 <kbd>esc</kbd> 를 누르면 ex 명령 모드 전환 (아직은 저장되지 않은 상태).


#### 명령 모드

**파일 내용 읽기만 가능**한 상태    
커서의 이동, 수정, 삭제, 복사, 붙여넣기 그리고 탐색 등의 기능 수행
<kbd>i</kbd>, <kbd>a</kbd>, <kbd>o</kbd>, <kbd>I</kbd>, <kbd>A</kbd>, <kbd>O</kbd> 등의 키를 눌러서 입력 모드로 전환
입력 모드 또는 ex 명령 모드 상태에서 <kbd>esc</kbd> 키를 누르면 명령 모드로 전환

|명령어 \| 키|설명|비고|
|---:|---|---|
|<kbd>i</kbd>|현재 커서 위치부터 입력||
|<kbd>I</kbd>|현재 커서 행 맨 앞에서부터 입력|<kbd>shift</kbd> + <kbd>i</kbd>|
|<kbd>a</kbd>|현재 커서 위치 다음 칸부터 입력||
|<kbd>A</kbd>|현재 커서 행 맨 끝에서부터 입력|<kbd>shift</kbd> + <kbd>a</kbd>|
|<kbd>o</kbd>|현재 커서 다음 행에 입력||
|<kbd>O</kbd>|현재 커서 이전 행에 입력|<kbd>shfit</kbd> + <kbd>o</kbd>|
|<kbd>s</kbd>|현재 커서 위치의 한 문자를 삭제하고 입력||
|<kbd>S</kbd>|현재 커서 행을 삭제하고 입력|<kbd>shift</kbd> + <kbd>s</kbd>|

#### 입력 모드 | 편집 모드 | input mode | insert mode

**파일 내용 편집이 가능**한 상태    
명령 모드에서 입력 전환키를 누르면 에디터의 왼쪽 아래 모서리 부분에 `-- INSERT --` 로 전환되었음을 알려주고 <kbd>esc</kbd> 키를 누르면 `-- INSERT --` 가 사라지므로써 입력 모드에서 명령 모드로 전환됨
자주 사용되는 명령어는 다음 표와 같으며 대부분 화면에 입력한 키가 나타나지는 않으므로 현재 모드를 확인하며 작업

|명령어 \| 키|설명|비고|
|---:|---|---|
|<kbd>x</kbd>|현재 커서가 위치한 문자 삭제|<kbd>del</kbd>|
|<kbd>dw</kbd>|단어 삭제||
|<kbd>dd</kbd>|현재 커서가 위차한 행 삭제||
|{숫자} + <kbd>dd</kbd>|현재 커서가 위치한 행부터 숫자만큼의 행 삭제||
|<kbd>yy</kbd>|현재 커서가 위치한 행 복사||
|{숫자} + <kbd>yy</kbd>|현재 커서가 위치한 행부터 숫자만큼의 행 복사||
|<kbd>p</kbd>|복사한 내용을 현재 커서가 위치한 행 이후 행에 붙여 넣기||
|<kbd>P</kbd>|복사한 내용을 현재 커서가 위치한 행 이전 행에 붙여 넣기|<kbd>shift</kbd> + <kbd>p</kbd>|
|<kbd>u</kbd>|직전 실행한 명령 취소|undo|
|<kbd>/exp</kbd> + <kbd>enter</kbd>|'exp' 문자열을 현재 커서 위치에서 오른쪽 또는 아래 방향에서 검색||
|<kbd>n</kbd>|찾은 문자가 여러개인 경우 현재 커서 위치에서 오른쪽 또는 아래 방향으로 일치하는 다음 문자 위치로 이동||
|<kbd>N</kbd>|찾은 문자가 여러개인 경우 현재 커서 위치에서 왼쪽 또는 위 방향으로 일치하는 이전 문자 위치로 이동|<kbd>shift</kbd> + <kbd>n</kbd>|

### ex 명령 모드 | 라인 명령 모드
명령 모드 상태에서 입력하는 키가 `에디터의 왼쪽 아래 모서리 부분`에 나타나며 ex 명령어를 이용하여 **에디터 저장, 종료, 탐색, 치환 및 vi 환경 설정 등이 가능**한 상태
명령 모드에서 <kbd>:</kbd> 키 입력으로 시작

|명령어 \| 키|설명|비고|
|---:|---|---|
|<kbd>q</kbd>|편집기 종료||
|<kbd>w</kbd>|저장||
|<kbd>!</kbd>|강제실행||

```
:q
```
편집한 내용이 없는 경우, 편집기 종료   
편집한 내용이 있는 경우, 저장하지 않았다는 에러 메시지를 보여주며 편집기는 종료되지 않음


```
:q!
```
내용을 편집하였든 하지 않았든 저장하지 않고 편집기 종료


```
:wq
```
편집한 내용은 저장하고 편집기 종료


### VIM 에디터 줄 번호 보기

지정된 파일 이름으로 생성하여 명령어 한 줄 입력 후 저장하는 것만으로 설정 가능

> vi ~/.vimrc

```sh
your-terminal> vi ~/.vimrc
```

.vimrc 파일에 명령어 한 줄 입력 후 저장

> set number

```sh
set number
```
* 또한, 파일 내용 보기 명령어 `cat` 은 옵션 `-n` 을 이용하여 줄 번호 보기 가능

> cat -n {your-file-name}

```sh
your-terminal> cat -n .vimrc
     1  set number
```

### sudo | su | su -

|명령어|설명|비고|
|----|---|---|
|sudo|다른 계정의 권한만 빌림|슈퍼유저 보안권한으로 프로그램을 구동할 수 있음. 기본 값 슈퍼유저.|
|su|다른 계정으로 전환|로그아웃을 하지 않고 다른 사용자 계정으로 전환|
|su -|다른계정으로 전환|다른 사용자 계정으로 전환하고 변경된 계정의 환경변수 이용|

---
</details>

## Contact

**email** warumono.for.develop@gmail.com    
**blog** https://warumono-for-develop.github.io

## Acknowledgements

* [안경잡이개발자](https://ndb796.tistory.com/)

## License

Distributed under the MIT License. See [`LICENSE`][license-url] for more information.

<!-- MARKDOWN LINKS & IMAGES -->

<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[spring-boot-restful-api-server-template]: https://github.com/warumono-for-develop/spring-boot-restful-api-server-template "Spring Boot RESTFul API Server Template"
[jenkins-installation-tutorial]: https://github.com/warumono-for-develop/jenkins-installation-tutorial "Docker Installation Tutorial"
[jenkins-installation-tutorial]: https://github.com/warumono-for-develop/jenkins-installation-tutorial "Jenkins Installation Tutorial"
[jenkins-installation-tutorial]: https://github.com/warumono-for-develop/jenkins-installation-tutorial "Jupyter Notebook Installation Tutorial"

[contributors-shield]: https://img.shields.io/github/contributors/warumono-for-develop/jenkins-installation-tutorial.svg?style=flat-square
[contributors-url]: https://github.com/warumono-for-develop/jenkins-installation-tutorial/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/warumono-for-develop/jenkins-installation-tutorial.svg?style=flat-square
[forks-url]: https://github.com/warumono-for-develop/jenkins-installation-tutorial/network/members
[stars-shield]: https://img.shields.io/github/stars/warumono-for-develop/jenkins-installation-tutorial.svg?style=flat-square
[stars-url]: https://github.com/warumono-for-develop/jenkins-installation-tutorial/stargazers
[issues-shield]: https://img.shields.io/github/issues/warumono-for-develop/jenkins-installation-tutorial.svg?style=flat-square
[issues-url]: https://github.com/warumono-for-develop/jenkins-installation-tutorial/issues
[license-shield]: https://img.shields.io/github/license/warumono-for-develop/jenkins-installation-tutorial.svg?style=flat-square
[license-url]: https://github.com/warumono-for-develop/jenkins-installation-tutorial/blob/master/LICENSE
[product-screenshot]: images/screenshot.png
