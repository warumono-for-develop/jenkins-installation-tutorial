[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![Apache-2.0 License][license-shield]][license-url]
<!-- [![license](https://img.shields.io/github/license/:user/:repo.svg)](LICENSE) -->

<p align="center">
  <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial">
    <img src="https://github.com/warumono-for-develop/default/blob/master/logos/warumono-logo-492x500.png?raw=true" alt="Logo" width="292" height="300">
  </a>

  <h1 align="center">Jupyter Notebook Installation Tutorial</h1>

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
Jenkins 공식 사이트에서 제공하는 이미지의 이름은 `jenkins` 이나, ~~업데이트 및 관리가 지속적으로 되고 있지 않다는 소문(?)이 있으니~~    
다른 이미지 이름 `jenkins/jenkins` 이미지를 다운로드 (docker pull) 할 것을 권장

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

> docker run -d -p {your-host-port}:{your-jenkins-port} -v {your-host-directory-path}:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins

`{your-host-directory-path}` 는 Jenkins 의 설정 정보 등이 로컬 (또는 Host) 디렉토리로 **백업**의 의미로 연결해 놓은 것이라고 이해하면 됨

```sh
your-terminal> docker run -d -p 8090:8080 -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins
```

### Step 3

#### Caution

Jenkins 설치 후 최초 로그인 화면에서 비밀번호가 필요한데, 해당 비밀번호는 설치시 임의의 해시 값으로 지정되어 있어 Jenkins 내부 파일에서 찾을 수 있음   
Jenkins 최초 로그인 화면에서 경로 정보 (/var/jenkins_home/secrets/initialAdminPassword) 를 사용   

> docker exec -it {your-jenkins-container-id} /bin/bash

> cat {your-jenkins-home-path}/secrets/initialAdminPassword
<your-jenkins-password>

```sh
your-terminal> docker exec -it 367932a46403 /bin/bash

jenkins-terminal> cat /var/jenkins_home/secrets/initialAdminPassword
d193e7948249066e97b34ade37d00f33@15
```

또는, Jenkins 설치 시 Docker 의 로그 정보에서도 찾을 수 있음    

> docker logs {your-jenkins-container-id}

> ...   
> \<your-jenkins-password\>   
> ...

```sh
your-terminal> docker logs 367932a46403
...
d193e7948249066e97b34ade37d00f33@15
...
```
>>>>>>











Configure Jupyter Notebook

> http://{your-host-ip}:{your-jenkins-port}

AWS EC2 인스턴스 **IPv4 Public IP** 와 `{your-host-port}` 를 웹 브라우져를 실행하여 URL 입력 창에 입력    

```sh
http://54.081.311.162:8090
```

사용자가 입력한 비밀번호를 자동변환하여 나온 해시 값은 **메모하여 이 후 설정 단계에서 사용**   
작업 완료 후, <kbd>control</kbd> + <kbd>Z</kbd> 또는 <kbd>control</kbd> + <kbd>D</kbd> 키 입력으로 python3 에서 나옴

> python3   
> from notebook.auth import passwd    
> passwd()

> 'sha1:\<generated-your-password-hash-value\>'

```sh
your-terminal> python3
Python 3.6.5 ...
.....
>> from notebook.auth import passwd
>> passwd()
Enter password:
Verify password:
'sha1:g0bf6y023f60:75h6h014f68d70c03175et61p4675c497b83u63'
```

### Step 4

Configure Jupyter Notebook

### Caution

> 사용자가 Jupyter Notebook 을 외부 (웹 브라우져) 에서 접속할 경우 AWS EC2 인스턴스의 상세정보 중 **IPv4 Public IP** 를 사용하고, AWS EC2 인스턴스 내부에 설치된 Jupyter Notebook 은 해당 인스턴스의 상세정보 중 **Private IPs** 를 사용하므로 미리 메모    
> AWS EC2 인스턴스의 **Security Group** 에 *Inbound* 로 *TCP* `{your-jupyter-notebook-port}` 값을 추가해줘야 정상적으로 접속할 수 있음   	
> *작성자의 AWS EC2 인스턴스는 Elastic IP 미사용, 도메인 미사용 상태로 기본으로 제공되는 IP 정보만으로 설명하고 사용*

설정 파일을 생성하고 VIM 에디터를 사용하여 비밀번호 등의 정보를 입력    
Jupyter Notebook 은 아이디에 해당하는 계정정보는 없고 **비밀번호만으로 접속**		
*설정 파일은 Jupyter Notebook 기본 설정 정보가 포함되어 생성 됨*

> jupyter notebook --generate-config    
> vi ~/.jupyter/jupyter_notebook_config.py

```sh
your-terminal> jupyter notebook --generate-config
Writing default config to: /home/ubuntu/.jupyter/jupyter_notebook_config.py
your-terminal> sudo vi ~/.jupyter/jupyter_notebook_config.py
```

<details>
  <summary>[Optional] SSL 사설 인증서 생성</summary> 

SSL 을 사용하지 않아도 Jupyter Notebook 을 사용하는데 문제는 없으나 보안성을 높이기 위하여 사용하는 것을 권장   
사설 인증서보다는 정상적인 인증기관으로부터 발급받은 인증서를 사용할 것을 권장


> openssl req -x509 -nodes -days {valid-days} -newkey rsa:1024 -keyout "{your-juypter-notebook-ssl-keyfile-name}.key" -out "{your-jupyter-notebook-ssl-certfile-name}.pem" -batch

```sh
your-terminal> mkdir ~/{your-ssl-file-directory-name}
your-terminal> cd {your-ssl-file-directory-name}
your-terminal> sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout "my-jupyter-notebook-key.key" -out "my-jupyter-notebook-cert.pem" -batch
your-terminal> ls
my-jupyter-notebook-key.key  my-jupyter-notebook-cert.pem
```

---
</details>

설정 파일 기존 내용 마지막 아래에 설정 내용 추가 입력   
`{your-host-ip}` 는 AWS EC2 인스턴스 상세정보의 **Private IPs**   
`{your-host-begin-path}` 는 Jupyter Notebook Dashboard 화면 (파일 탐색기와 같은 화면) 에서 AWS EC2 인스턴스 내부 스토리지 중 **실행 시 보여지게 될 경로**로써 자신이 원하는 정확한 경로를 입력    

> c = get_config()    
> c.NotebookApp.password = u'{generated-your-password-hash-value}'    
> c.NotebookApp.ip = '{your-host-ip}'    
> c.NotebookApp.notebook_dir = '{your-host-begin-path}'

```sh
...
# ============================================================
# my jupyter-notebook configuration
# ============================================================
c = get_config()
c.NotebookApp.password = u'sha1:g0bf6y023f60:75h6h014f68d70c03175et61p4675c497b83u63'
c.NotebookApp.ip = '172.31.35.203'
c.NotebookApp.notebook_dir = '/home/ubuntu'
```

<details>
  <summary>[Optional] SSL 사설 인증서 적용</summary> 

SSL 을 적용하고자 사설 인증서를 생성하였다면, 다음 설정 내용을 추가로 입력


> c.NotebookApp.keyfile = u'{your-juypter-notebook-ssl-keyfile-name}.key'   
> c.NotebookApp.certfile = u'{your-jupyter-notebook-ssl-certfile-name}.pem'

```sh
...
c.NotebookApp.notebook_dir = '/home/ubuntu'
c.NotebookApp.keyfile = u'my-jupyter-notebook-key.key'
c.NotebookApp.certfile = u'my-jupyter-notebook-cert.pem'
```

---
</details>

### Step 5

Jupyter Notebook 백그라운드 실행 설정

정상적으로 Jupyter Notebook 의 설치 및 설정이 완료된 후 터미널을 이용하여 실행하고, 해당 터미널을 닫거나 임의로 끊기는 경우에는 Jupyter Notebook 프로그램도 중지가 되므로 이를 방지하기 위하여 백그라운드에서 실행되도록 설정할 것을 권장

> bg    
> disown -h

```sh
your-terminal> bg
[3]+ sudo jupyter-notebook --allow-root &
your-terminal> disown -h
```



## Usage

정상적으로 Jupyter Notebook 설치 및 설정이 완료되었다면 실행하여 웹 브라우져로 접근 후 확인

### Caution

> 앞서 [Step 4](#step-4) 에서 설명한대로 사용자가 Jupyter Notebook 을 외부 (웹 브라우져) 에서 접속할 경우 AWS EC2 인스턴스 상세정보 중 **IPv4 Public IP** 사용

### Run Jupyter Notebook

> jupyter-notebook --allow-root   

> \[...\] {your-jupyter-notebook-scheme}://{your-host-ip}:{your-jupyter-notebook-port}/    

작성자는 SSL 을 적용하여 작업하는 상태이므로, 다음 예시에서 `{your-jupyter-notebook-scheme}` 은 `https` 로 나타남

```sh
your-terminal> sudo jupyter-notebook --allow-root
...
[...] https://172.31.35.203:8888/
...
```

### Connect to Jupyter Notebook on Web Browser

> {your-jupyter-notebook-scheme}://{your-host-ip}:{your-jupyter-notebook-port}

Jupyter Notebook 실행 로그 정보 중 접속 URL {your-jupyter-notebook-scheme}://**{your-host-ip}**:{your-jupyter-notebook-port} 에서 `{your-host-ip}` 는 AWS EC2 인스턴스의 **Private IPs** 즉, EC2 내부 IP 로 제시하는 URL 그대로 사용해서는 Jupyter Notebook 에 접속할 수 없음   
웹 브라우져를 실행하고 URL 입력 창에 {your-jupyter-notebook-scheme}://**{AWS EC2 인스턴스 상세정보 중 IPv4 Public IP}**:{your-jupyter-notebook-port} 를 입력하여 접속

> ~~https://172.31.35.203:8888~~

```
https://54.081.311.162:8888
```

<details>
  <summary>[Optional] 알 수 없는 인증기관 발급 사설 인증서에 따른 Jupyter Notebook URL 접근 불가 해결 방법</summary> 

Google Chrome 웹 브라우져의 경우, 알 수 없는 인증기관에서 발급된 사설인증서를 이용한 사이트 접근을 우선적으로 방지하고 있어 경고 화면이 나오게 됨   
이 경우, 경고 화면에서 어떠한 동작도 하지 않고   
<kbd>t</kbd><kbd>h</kbd><kbd>i</kbd><kbd>s</kbd><kbd>i</kbd><kbd>s</kbd><kbd>u</kbd><kbd>n</kbd><kbd>s</kbd><kbd>a</kbd><kbd>f</kbd><kbd>e</kbd>    
를 키보드로 입력하면 해당 사이트 접근이 가능하도록 되어 있음   
본 지침서의 경우도 마찬가지로 사설인증서이기에 경고 화면이 나오면`thisisunsafe` 를 입력하여야 Jupyter Notebook Dashboard 화면으로 접근 가능

---
</details>

### Jupyter Notebook Dashboard

*Jupyter 공식 웹사이트의 예시 화면*    
예시 화면의 오른쪽 위 *`New v`* 버튼을 눌러 Drop Down 메뉴 중 *`Terminal`* 을 선택하면 새 브라우져 창 (또는 새 탭) 으로 터미널 화면이 나옴    
![Jupyter Notebook Dashboard](https://jupyter.readthedocs.io/en/latest/_images/tryjupyter_file.png)



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
