<!-- SHIELDS -->



[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]



<!-- LOGO -->



<br />
<p align="center">
  <a href="https://github.com/warumono-for-develop/jenkins-installation-tutorial">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

  <h1 align="center">Jenkins Inatallation Tutorial</h1>

  <p align="center">
    AWS EC2 인스턴스에 Jenkins 프로그램 설치 관련 지침서
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



<!-- TABLE OF CONTENTS -->



## Table of Contents

* [About the Tutorial](#about-the-tutorial)
  * [Official Website](#official-website)
  * [Built With](#built-with)
* [Preview](#preview)
* [Getting Started](#getting-started)
  * [References](#references)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
    * [Step 1 Jenkins 설치](#jenkins-설치)
    * [Step 2 Jenkins 구동](#jenkins-구동)
    * [Step 3 Jenkins 설정](#jenkins-설정)
      * [Step 1 임시 비밀번호 찾기](#임시-비밀번호-찾기)
      * [Step 2 Jenkins 접속](#jenkins-접속)
      * [Step 3 Customize Jenkins](#customize-jenkins)
      * [Step 4 Plugins in Getting Started](#plugins-in-getting-started)
      * [Step 5 Create First Admin User](#create-first-admin-user)
      * [Step 6 Instance Configuration](#instance-configuration)
      * [Step 7 Jenkins is ready!](#jenkins-is-ready!)
      * [Step 8 Connect into Jenkins](#connect-into-jenkins)
      * [Step 9 Install Docker in Jenkins](#install-docker-in-jenkins)
    * [Step 4 Jenkins 원격 빌드 설정](#jenkins-원격-빌드-설정)
      * [Step 1 원격 빌드 활성화 및 Authentication Token 생성](#원격-빌드-활성화-및-authentication-token-생성)
      * [Step 2 CSRF 비활성화](#csrf-비활성화)
      * [Step 3 API Token 생성](#api-token 생성)
* [Usage](#usage)
  * [Step 1 Create new job](#create-new-job)
  * [Step 2 Build now](#build-now)
  * [Step 3 Test](#test)
    * [빌드 Test](#빌드-test)
    * [원격 빌드 Test](#원격-빌드-test)
* [Roadmap](#roadmap)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)
* [Acknowledgements](#acknowledgements)



<!-- ABOUT THE PROJECT -->



## About The Tutorial

어플리케이션 배포를 자동화하여 편리하며

원격으로 배포를 진행 가능

최종적으로, 어플리케이션 + Git + Docker + Jenkins 를 모두 연동하여 개발에 집중할 수 있도록 하는게 목적

Jenkins 를 사용하는 이유 :
* 배포 자동화에 따른 개발에 집중 가능
* 원격 배포 가능

*Jeninks 외 다른 프로그램 또는 그 외 방법들도 있겠지만, Jenkins 프로그램을 써보는 것도 추천*

### Official Website

공식 사이트 또는 관련 정보를 미리 숙지하고 사용할 것을 권장

* [Jenkins](https://jenkins.io/)

### Built with

#### Required

- [x] [Jenkins](https://jenkins.io/)

  Docker version 19.03.6

- [x] [Docker](https://www.docker.com/)

  Docker version 19.03.6
  
  [Docker Installation Tutorial](https://github.com/warumono-for-develop/docker-installation-tutorial)

- [x] [AWS](https://aws.amazon.com/ko/) EC2 Instance

  \[Free tier\] Ubuntu Server 18.04 LTS (HVM), SSD Volume Type

#### Optional

- [ ] [Jupyter Notebook](https://jupyter.org/)

  [Jupyter Notebook Installation Tutorial](https://github.com/warumono-for-develop/jupyter-notebook-installation-tutorial)



<!-- PREVIEW -->



## Preview

전반적으로 소소한 설정 작업이 많아, 전반적인 작업 순서를 간략하게 기술

|순서|작업장|작업도구|작업내용|작업결과|참조|비고|
|---|----|------|------|------|---|---|
|1|AWS|웹 브라우져|EC2 설치|EC2 정상 구동|||
|2|AWS EC2|터미널|Docker 설치|Docker 정상 설치|[Docker 설치](https://github.com/warumono-for-develop/docker-installation-tutorial#Installation)||
|3|AWS EC2|터미널|Docker 설정|Docker 정상 구동|[Docker 구동](https://github.com/warumono-for-develop/docker-installation-tutorial#usage)|AWS EC2 용 Docker|
|4|AWS EC2|터미널|Jenkins 설치|Jenkins 정상 설치|[Jenkins 설치](#jenkins-설치)||
|5|AWS EC2|터미널|Jenkins 설정|Jenkins 정상 구동|[Jenkins 설정](#jenkins-설정)||
|6|Jenkins in AWS EC2|터미널|Docker 설치|Docker 정상 설치|[Install Docker in Jenkins](#install-docker-in-jenkins)|Jenkins 용 또 다른 Docker|
|7|Jenkins in AWS EC2|터미널|Docker 설정|Docker 정상 구동|[Install Docker in Jenkins](#install-docker-in-jenkins)|Jenkins 용 또 다른 Docker|
|8|Jenkins in AWS EC2|터미널|Jenkins job 생성|job 정상 생성|[Create new job](#create-new-job)||
|9|Jenkins in AWS EC2|터미널|Jenkins job 빌드|Docker 이미지 다운로드 및 실행|[Build now](#build-now)||
|10|Jenkins in AWS EC2|터미널|Jenkins job 로그 확인|Docker 이미지 정상 구동 확인|[빌드 Test](#빌드-test)||
|11|Jenkins in AWS EC2|터미널|Jenkins 설정|원격 빌드 활성화|[Jenkins 원격 빌드 설정](#jenkins-원격-빌드-설정)||
|12|Jenkins in AWS EC2|터미널|Jenkins job 원격 빌드|원격 빌드 확인|[원격 빌드 Test](#원격-빌드-test)||
|13|Jenkins in AWS EC2|터미널|Jenkins job 원격 빌드 로그 확인|Docker 이미지 정상 구동 확인|[원격 빌드 Test](#원격-빌드-test)||



<!-- GETTING STARTED -->



## Getting Started

Jenkins 설치는 터미널을 이용하여 명령어를 입력하는 작업이 많으므로 진행 순서와 오탈자에 주의

*본 작업을 진행에 앞서 프로그램을 설치할 대상 서버는 반드시 백업 완료 후 진행할 것을 권장*

*사용자가 사용하는 프로그램 버전과 환경 등에 따라 설명글의 진행 절차와 결과가 다소 다르거나 생략 또는 추가 작업이 있을 수 있음*

*설명글에 사용되는 용어 및 단어 등은 되도록 공식적(?)으로 사용되는 것으로 표기하였으나 오탈자 및 잘못된 용어 또는 정보가 있을 수 있음*

### References

**_`your-teminal>`_** 사용자의 터미널 프로그램 프롬프트를 나타내며 입력하는 문자가 아님

**_`{your-xxx}`_** 사용자가 정의해야하는 항목 변수이며 사용자가 직접 임의의 이름이나 특정된 값 등으로 입력

**_`{auto-xxx}`_** 사용자의 요청에 따른 결과 항목 변수이며 프로그램이 임의의 이름이나 특정된 값 등으로 노출

### Prerequisites

#### AWS EC2 인스턴스 **생성** 및 **활성화**

기존 AWS EC2 인스턴스 또는 새로운 인스턴스를 생성하여 정상적으로 구동중인 서버

#### AWS EC2 인스턴스의 **보안 그룹 설정**

AWS EC2 인스턴스에 적용되어 있는 보안 그룹으로 이동하여 Inbound 에 Jenkins 에 접속할 URL 의 PORT 추가

#### AWS EC2 인스턴스 내부 IP 정보

AWS EC2 인스턴스의 상세 정보 중 **Private IPs**

*Private IPs 정보는 터미널의 명령어를 이용하여도 확인 가능*

```sh
your-terminal> ifconfig
```

### Installation

#### Step 1

##### Jenkins 설치

Docker 명령어로 jenkins 키워드로 이미지 검색을 해보면 많은 이미지들이 조회 됨

Jenkins 공식 사이트에서 제공하는 이미지의 이름은 `jenkins` 이나, 업데이트 및 관리가 지속적으로 되고 있지 않다는 소문(?)이 있으니

다른 이미지 이름 **`jenkins/jenkins`** 이미지를 다운받아 사용할 것을 권장

```sh
your-terminal> docker search jenkins
NAME                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
jenkins                                Official Jenkins Docker image                   4666                \[OK\]                
jenkins/jenkins                        The leading open source automation server       1920                                   
...

your-terminal> docker pull jenkins/jenkins
```

#### Step 2

##### Jenkins 구동

docker run -d -p **_`{your-host-inbound-port}`_**:**_`{your-jenkins-inbound-port}`_** -v **_`{your-host-jenkins-diectory-full-path}`_**:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins

```sh
your-terminal> docker run -d -p 8080:8080 -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins
```

#### Step 3

##### Jenkins 설정

> ### Step 1

<blockquote>

#### 임시 비밀번호 찾기

cat {your-host-jenkins-diectory-full-path}/secrets/initialAdminPassword

또는, `docker logs jenkins` 명령어로 해당 비밀번호 해시 값 확인 가능

```sh
your-terminal> cat /var/jenkins/secrets/initialAdminPassword
{auto-password-hash-value}
```

**{auto-password-hash-value}** 값 메모하여 [Jenkins 최초 화면 진입](#jenkins-접속) 시 사용

</blockquote>

> ### Step 2

<blockquote>

#### Jenkins 접속

정상적으로 Jenkins 가 구동되었다면, 웹 브라우져를 실행하고 URL 입력 창에 `http://{your-aws-ec2-private-ip}:{your-host-inbound-port}` 를 입력

Jenkins 최초 화면에서 비밀번호 입력에 관한 내용과 입력 창이 보임

**[{auto-password-hash-value}](#임시-비밀번호-찾기)** 값 복사하여 입력 창에 붙여넣기

</blockquote>

> #### Step 3

<blockquote>

#### Customize Jenkins

`Customize Jenkins` 화면에서 `Install suggested plugins` 를 선택

</blockquote>

> #### Step 4

<blockquote>

#### Plugins in Getting Started

`Getting Started` 화면에서 플러그인 목록이 나열되어 있고 자동으로 해당 플러그인들을 다운로드 및 설치 함

*다소 시간이 걸리므로 차분히 설치 완료 될때까지 기다림*

</blockquote>

> #### Step 5

<blockquote>

#### Create First Admin User

`Create First Admin User` 화면에서 사용자 계정 정보를 입력하여 저장 (`Save and Finish`)

</blockquote>

> #### Step 6

<blockquote>

#### Instance Configuration

`Instance Configuration` 화면에서 `Jenkins URL: http://localhost:8080/` 기본 설정 값으로 사용

*jenkins 이미지의 경우 Instance Configuration 화면이 없음*

</blockquote>

> #### Step 7

<blockquote>

#### Jenkins is ready!

`Jenkins is ready!` 화면에서 `Start using Jenkins` 버튼 클릭

정상적으로 설정이 완료되었다면, `Jenkins 대시보드` 화면이 나타남

</blockquote>

> #### Step 8

<blockquote>

#### Connect into Jenkins

Jenkins 는 임의의 어플리케이션을 갖고 있어야 하기에 이 어플리케이션 파일 및 리소스를 Docker image 를 이용하기에 Jenkins 내부에 Docker 가 설치되어 있어야 함

단, Jenkins 내부에 Docker 를 설치하지 않고 외부의 Docker 와 연동하는 방법도 존재하지만 본 설명서에는 Jenkins 내부에 Docker 를 설치하여 구동하는 것을 설명

**Jenkins 가 구동되어 있는 상태에서 진행**하여야 하며, Docker 를 설치 과정은 **Jenkins 내부로 접속**하는 것부터 시작

*docker* **exec -it *{your-jenkins-container-id}* /bin/bash**

```sh
your-terminal> docker ps -a

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS                       NAMES
cb03e2270a2e        jenkins             "/bin/tini -- /usr/l…"   1 hours ago         Up 45 hours              0.0.0.0:8080->8080/tcp, 50000/tcp   funny_golick

your-terminal> docker exec -it cb03e2270a2e /bin/bash

your-jenkins>
```

</blockquote>

> #### Step 9

<blockquote>

#### Install Docker in Jenkins

[Required](#required) [Docker Installation Tutorial](https://github.com/warumono-for-develop/docker-installation-tutorial) 과 혼돈하지 말아야 함

[기존에 설치되어 있는 Docker](#required) 는 **`로컬` 또는 `호스트 서버`에 설치되어 있는 Docker**

본 순서에서 진행하는 Docker 설치는 **`Jenkins 내부`에서 사용하게 되는 Docker**

Docker 공식 사이트에서 제공하는 Shell Script 를 다운로드 하여 해당 Shell Script 를 실행하므로써 **Docker 다운로드 및 설치, 설정을 한번에 작업 가능**

*작업 진행 경로는 아무곳에서나 진행하여도 무관하나, 사용자 기본 접근 경로 root 에서 진행하였음*

*작업 완료 후, `exit` 을 입력하여 Jenkins 로 부터 나옴*

```sh
your-jenkins> cd ~

your-jenkins> curl -fsSL https://get.docker.com -o get-docker.sh

your-jenkins> sh get-docker.sh

your-jenkins> docker --version
Docker version 19.03.6, build 369ce74a3c
```

</blockquote>

#### Step 4

##### Jenkins 원격 빌드 설정

curl 명령어 또는 웹 브라우져의 URL 입력 창에 원격 빌드 접근 URL 을 사용하여 빌드를 진행할 수 있음

> ### Step 1

<blockquote>

#### 원격 빌드 활성화 및 Authentication Token 생성

`Jenkins 대시보드` 화면에서 job 목록 중 원격 빌드 할 대상의 `Name` 을 클릭

`Project {auto-jenkins-job-name}` 의 화면에서 왼쪽 메뉴 중 `Configure` 선택

상세 화면에서 `Build Trigger 섹션` 목록 중 `Trigger builds remotely (e.g., from scripts)` 선택하여 **체크박스 활성화**

그에 따라, `Authentication Token` 의 입력 창이 나타나면 `비밀번호 처럼 사용하게 될 문자열 (암호화된 코드 등으로 가독성이 떨어지는 문자열 사용을 권장)` 입력

</blockquote>

> ### Step 2

<blockquote>

#### CSRF 비활성화

`Jenkins 대시보드` 화면에서 왼쪽 메뉴 중 `Manage Jenkins` 선택

`Manage Jenkins` 화면에서 하단 부분 설치되어 있는 Plugin 목록 중 `Configure Global Security` 선택

`Configure Global Security` 화면에서 중간 부분 `Prevent Cross Site Request Forgery exploits` 선택하여 **체크박스 비활성화**

</blockquote>

> ### Step 3

<blockquote>

#### API Token 생성

`Jenkins 대시보드` 화면에서 왼쪽 메뉴 중 `Manage Users` 선택

`Users` 화면에서 사용자 목록 중 임의의 사용자 오른쪽 `톱니바퀴 버튼` 클릭

사용자 상세 정보 화면에서 `API Token 섹션` 의 `Show API Token...` 버튼 클릭하여 **`API Token` 메모**

</blockquote>



<!-- USAGE EXAMPLES -->



## Usage

정상적으로 Docker 설치 및 설정이 완료되었다면 실행하여 Docker 공식 사이트에서 제공하는 샘플(?) image `hello-world` 를 다운로드하여 구동하는 것으로 Docker 기본 사용방법 연습

Docker 명령어는 공식 사이트 또는 인터넷 등으로 미리 숙지하고 사용하는 것을 권장

타 설명글에 *[자주 사용하는 명령어](https://github.com/warumono-for-develop/docker-installation-tutorial#docker-command)* 를 간략하게 설명하였으니 참고하여 숙지하고 연습하기를 권장

#### Step 1

##### Create new job

`Jenkins 대시보드` 화면에서 생성된 job 이 존재하지 않는 경우 아래 화면에서 `create new jobs` 를 선택

`Enter an item name` 사용자가 원하는 이름 입력 (일반적으로 구동하고자 하는 대상 어플리케이션의 이름을 토대로 입력)

`Freestyle project` 선택

`OK` 클릭

`General 탭`의 `Build 섹션`에서 `Add build step` Drop Down 을 펼쳐 `Execute shell` 선택

`Command` 입력 창이 나타나고, 이 입력 창에 Docker Hub 에서 기본적으로 제공하는 `hello-world` **이미지를 다운로드** 하고, 해당 이미지의 **컨테이너를 실행**하는 명령어를 입력

```sh
docker pull {your-docker-image-name}

docker run {your-docker-image-name}
```

#### Step 2

##### Build now

`Project {your-jon-name}` 화면 왼쪽 메뉴에서 `Build Now` 를 선택하여 빌드 진행

왼쪽 메뉴 아래에 `Build History` 에 실행 한 횟수에 따라 `#{auto-jenkins-build-index}` 와 실행 날짜가 생성되어 표시 됨

Progress bar 가 나타나 진행 상태를 보여주며 빌드 오류 시 `빨간색` 으로 표시되고, 정상적인 경우 `파란색` 으로 표시 됨

#### Step 3

> ### Test

<blockquote>

#### 빌드 Test

해당 빌드(`#X`)를 클릭하여 상세 화면으로 이동하여 화면 왼쪽 메뉴에서 `Console Output` 을 클릭

빌드 관련 로그 및 어플리케이션의 로그 등이 표시되어 나타남

[Docker **hello-wolrd** 실행 결과](https://github.com/warumono-for-develop/docker-installation-tutorial#run-container) 에서 실행한 결과 정보와 추가적인 빌드 로그 정보가 보여지며 로그 마지막 부분에 `Finished: SUCCESS` 가 보였다면 정상적으로 빌드 및 배포가 완료되었음을 의미함

</blockquote>

<blockquote>

#### 원격 빌드 Test

curl -X POST http://{your-jenkins-username}:{auto-jenkins-api-token}@{your-aws-ec2-private-ip:your-jenkins-port}/job/{your-jenkins-job-name}/build?token={your-jenkins-authentication-token}

|변수|설명|참조|비고|
|---|---|---|---|
|your-jenkins-username|Jenkins 사용자 아이디|[Create First Admin User](#create-first-admin-user)||
|auto-jenkins-api-token|Jenkins API Token|[원격 빌드 활성화 및 API Token 생성](#원격-빌드-활성화-및-api-token-생성)||
|your-aws-ec2-private-ip|Jenkins 호스트 서버 IP|[AWS EC2 인스턴스 내부 IP 정보](#aws-ec2-인스턴스-내부-ip-정보)||
|your-jenkins-port|Jenkins 호스트 서버 PORT|[Instance Configuration](#instance-configuration)||
|your-jenkins-job-name|빌드 대상 Jenkins job Name|[Create new job](#create-new-job)||
|your-jenkins-authentication-token|Jenkins Authentication Token|[원격 빌드 활성화 및 Authentication Token 생성](#원격-빌드-활성화-및-authentication-token-생성)||

명령어 규칙에 따라 작성된 예시

```sh
your-terminal> curl -X POST http://warumono:a3t32p94xe400rr29fb34abc41doofee@15.225.202.16:8080/job/hello-app/build?token=build_token
```

`Jenkins 대시보드` 화면에서 job 목록 중 빌드 대상이였던 job Name 을 클릭하여 상세 화면으로 이동

해당 빌드(`#X`)를 클릭하여 상세 화면으로 이동하여 화면 왼쪽 메뉴에서 `Console Output` 을 클릭

빌드 관련 로그 및 어플리케이션의 로그 등이 표시되어 나타남

[Docker **hello-wolrd** 실행 결과](https://github.com/warumono-for-develop/docker-installation-tutorial#run-container) 에서 실행한 결과 정보와 추가적인 빌드 로그 정보가 보여지며 로그 마지막 부분에 `Finished: SUCCESS` 가 보였다면 정상적으로 빌드 및 배포가 완료되었음을 의미함

</blockquote>

<!-- ROADMAP -->



## Roadmap

See the [open issues](https://github.com/warumono-for-develop/jenkins-installation-tutorial/issues) for a list of proposed features (and known issues).



<!-- CONTRIBUTING -->



## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project

2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)

3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)

4. Push to the Branch (`git push origin feature/AmazingFeature`)

5. Open a Pull Request



<!-- LICENSE -->



## License

Distributed under the MIT License. See [`LICENSE`][license-url] for more information.



<!-- CONTACT -->



## Contact

**warumono** - warumono.for.develop@gmail.com

Project link: [https://github.com/warumono-for-develop/jenkins-installation-tutorial](https://github.com/warumono-for-develop/jenkins-installation-tutorial)



<!-- ACKNOWLEDGEMENTS -->



## Acknowledgements

* [안경잡이개발자](https://ndb796.tistory.com/)



<!-- MARKDOWN LINKS & IMAGES -->



<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
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

