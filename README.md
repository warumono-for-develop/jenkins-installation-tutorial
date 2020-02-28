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
* [Getting Started](#getting-started)
  * [References](#references)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
    * [Step 1 Plugin 설치](#step-1)
    * [Step 2 Docker 설치](#step-2)
    * [Step 3 Docker 상태 확인](#step-3)
* [Usage](#usage)
  * [Step 1 Pull image](#step-1)
  * [Step 2 List images in local Docker](#step-2)
  * [Step 3 Run container](#step-3)
  * [Step 4 List containers in Docker](#step-4)
  * [Step 5 Stop container](#step-5)
  * [Step 6 Remove container](#step-6)
  * [Step 7 Remove image](#step-7)
* [Attach](#attach)
  * [Docker command](#docker-command)
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
* 원결 배포 가능

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

- [x] [AWS](https://aws.amazon.com/ko/) EC2 Instance

  \[Free tier\] Ubuntu Server 18.04 LTS (HVM), SSD Volume Type

#### Optional

- [ ] [Jupyter Notebook](https://jupyter.org/)

  [Jupyter Notebook Installation Tutorial](https://github.com/warumono-for-develop/jupyter-notebook-installation-tutorial)



<!-- GETTING STARTED -->



## Getting Started

Docker 설치는 터미널을 이용하여 명령어를 입력하는 작업이 많으므로 진행 순서와 오탈자에 주의

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

docker run -d -p {your-host-inbound-port}:{your-jenkins-inbound-port} -v {your-host-jenkins-diectory-full-path}:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins

```sh
your-terminal> docker run -d -p 8080:8080 -v /home/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins
```

#### Step 3

##### Jenkins 설정

###### Step 1

###### 임시 비밀번호 찾기

cat {your-host-jenkins-diectory-full-path}/secrets/initialAdminPassword

또는, `docker logs jenkins` 명령어로 해당 비밀번호 해시 값 확인 가능

```sh
your-terminal> cat /var/jenkins/secrets/initialAdminPassword
{auto-password-hash-value}
```

**{auto-password-hash-value}** 값 메모하여 [Jenkins 최초 화면 진입](#jenkins-접속)시 사용

###### Step 2

###### Jenkins 접속

정상적으로 Jenkins 가 구동되었다면, 웹 브라우져를 실행하고 URL 입력 창에 `http://{your-aws-ec2-private-ip}:{your-host-inbound-port}` 를 입력

Jenkins 최초 화면에서 비밀번호 입력에 관한 내용과 입력 창이 보임

**[{auto-password-hash-value}](#임시-비밀번호-찾기)** 값 복사하여 입력 창에 붙여넣기

###### Step 3

###### Customize Jenkins

`Customize Jenkins` 화면에서 `Install suggested plugins` 를 선택

###### Step 4

###### Getting Started

`Getting Started` 화면에서 플러그인 목록이 나열되어 있고 자동으로 해당 플러그인들을 다운로드 및 설치 함

*다소 시간이 걸리므로 차분히 설치 완료 될때까지 기다림*

###### Step 5

###### Create First Admin User

`Create First Admin User` 화면에서 사용자 계정 정보를 입력하여 저장(Save and Finish)

###### Step 6

###### Instance Configuration

`Instance Configuration` 화면에서 `Jenkins URL: http://localhost:8080/` 기본 설정 값으로 사용

*jenkins 이미지의 경우 Instance Configuration 화면이 없음*

###### Step 7

###### Jenkins is ready!

`Jenkins is ready!` 화면에서 `Start using Jenkins` 버튼 클릭

정상적으로 설정이 완료되었다면, `Jenkins 대시보드` 화면이 나타남

###### Step 8

###### Connect into Jenkins

Jenkins 는 임의의 어플리케이션을 갖고 있어야 하기에 이 어플리케이션 파일 및 리소스를 Docker image 를 이용하기에 Jenkins 내부에 Docker 가 설치되어 있어야 함

단, Jenkins 내부에 Docker 를 설치하지 않고 외부의 Docker 와 연동하는 방법도 존재하지만 본 설명서에는 Jenkins 내부에 Docker 를 설치하여 구동하는 것을 설명

Jenkins 가 구동되어 있는 상태에서 진행하여야 하며, Docker 를 설치 과정은 Jenkins 내부로 접속하는 것부터 시작

*docker* **exec -it *{your-jenkins-container-id}* /bin/bash**

```sh
your-terminal> docker ps -a

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                   PORTS                       NAMES
cb03e2270a2e        jenkins             "/bin/tini -- /usr/l…"   1 hours ago         Up 45 hours              0.0.0.0:8080->8080/tcp, 50000/tcp   funny_golick

your-terminal> docker exec -it cb03e2270a2e /bin/bash

your-jenkins>
```

###### Step 9

###### Install Docker in Jenkins

Docker 공식 사이트에서 제공하는 Shell Script 를 다운로드 하여 해당 Shell Script 를 실행하므로써 **Docker 다운로드 및 설치를 한번에 작업 가능**

*작업 진행 경로는 아무곳에서나 진행하여도 무관하나, 사용자 기본 접근 경로 ( `~` )에서 진행하였음*

*작업 완료 후, `exit` 을 입력하여 Jenkins 로 부터 나옴*

```sh
your-jenkins> cd ~

your-jenkins> curl -fsSL https://get.docker.com -o get-docker.sh

your-jenkins> sh get-docker.sh

your-jenkins> docker --version
Docker version 19.03.6, build 369ce74a3c
```



<!-- USAGE EXAMPLES -->



## Usage

정상적으로 Docker 설치 및 설정이 완료되었다면 실행하여 Docker 공식 사이트에서 제공하는 샘플(?) image `hello-world` 를 다운로드하여 구동하는 것으로 Docker 기본 사용방법 연습

Docker 명령어는 공식 사이트 또는 인터넷 등으로 미리 숙지하고 사용하는 것을 권장

본 설명글에는 *[자주 사용하는 명령어](#docker-command)* 를 간략하게 설명하였으니 참고하여 숙지하고 연습하기 권장

#### Step 1

##### Create new job


`Jenkins 대시보드` 화면에서 `create new job` 선택

Enter an item name 사용자가 원하는 이름 입력

Freestyle project 선택

OK 클릭


General 탭의 Build 섹션에서 Add build step 을 선택하여 Execute shell 선택

Command 입력 창에 Git Hub 에 등록되어 Docker Hub 에서 업로드 된 이미지를 다운로드 하고, 해당 이미지의 컨테이너를 실행하는 명령어를 입력
docker pull {your-docker-image-name}
docker run {your-docker-image-name}


Jenkins 대시보드로 이동하여 

Case 1

생성된 job 이 존재하지 않는 경우 아래 화면에서 `create new jobs` 를 선택
또는, 왼쪽 메뉴에서 New Item 선택

    Welcome to Jenkins!
    Please create new jobs to get started.

Enter an item name 사용자가 원하는 이름 입력

Freestyle project 선택

OK 클릭



생성된 job 이 존재하는 경우 job 목록에서 해당 job 의 Name 을 선택

해당 Project {your-jon-name} 으로 이동

Project {your-jon-name} 화면에서 왼쪽 메뉴에서 Build Now 를 선택하면 왼쪽 메뉴 바로 아래에 Build History 에 실행 한 횟수에 따라 #X 와 실행 날짜가 생성 됨
<--- 해당 빌드(#X)를 클릭하여 상세 화면으로 이동하여 화면 왼쪽 메뉴에서 Console Output 를 클릭하면 로그를 정보를 확인할 수 있음

정상적으로 빌드 완료 후, 브라우져에서 어플리케이션을 테스트할 수 있는 URL(http://your-ec2-ip:your-local-port/xxx) 로 테스트

#### Step 2

##### Build now




Docker Hub 에 등록되어 공개되어 있는 image 를 검색하는 명령어

docker **search** `{your-docker-image-name-search-keyword}`

Docker Hub 로 부터 해당 image 를 로컬 저장소로 다운로드 하는 명령어

docker **pull** `{your-docker-image-name}`

```sh
your-terminal> docker pull hello-world
```

#### Step 2

##### List images in local Docker

docker images

```sh
your-terminal> docker images

REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
hello-world                    latest              fce289e99eb9        14 months ago       1.84kB
```

#### Step 3

##### Run container

docker **run** `{your-docker-image-name}`

```sh
your-terminal> docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

#### Step 4

##### List containers in Docker

docker **ps -a**

```sh
your-terminal> docker ps -a

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS                         NAMES
d3d5ae2842b1        hello-world         "/hello"                 3 minutes ago       Exited (0) 3 minutes ago                         sad_haibt
```

#### Step 5

##### Stop container

docker **stop** `{your-docker-container-id}`

```sh
your-terminal> docker stop hello-world
```

#### Step 6

##### Remove container

docker **rm** `{your-docker-container-id}`

```sh
your-terminal> docker rm hello-world
```

#### Step 7

##### Remove image

임의의 이미지를 삭제하려는 경우 해당 이미지가 컨테이너에 의존되어 있고 컨테이너가 중지되었든 구동되고 있든 상관없이 **컨테이너가 존재하면 이미지 삭제 불가**

docker **rmi** `{your-docker-container-id}`

```sh
your-terminal> docker rmi hello-world
```



<!-- ATTACH -->



## Attach

### Docker command

[Use the Docker command line](https://docs.docker.com/engine/reference/commandline/cli/)

모든 명령어의 뒤에 `--help` 를 입력하면 해당 명령어의 사용법이 표기 됨

```sh
your-terminal> docker xxxx --help
```

<!--
<details>
  <summary>
    <i>Clickable Collapse Link</i>
    <a href="https://your-reference-url"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Another Reference Link</a>
  </summary>
  <p>
    your contents
  </p>
</details>
-->

<details>
  <summary>
    <i>Click to view the document</i>
    <a href="https://docs.docker.com/engine/reference/commandline/cli/"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Use the Docker command line</a>
  </summary>

> ## About image

<blockquote>

> ### 보유 이미지 목록 조회

<blockquote>

*docker* **images**

```sh
your-terminal> docker images
```

</blockquote>

> ### 이미지 이름 조회

<blockquote>

*docker* **search {your-docker-image-name-search-keyword}**

```sh
your-terminal> docker search hello
```

</blockquote>

> ### 이미지 다운로드

<blockquote>

*docker* **pull {your-docker-image-name}:{your-docker-image-version}**

```sh
your-terminal> docker pull hello-world:latest

your-terminal> docker pull hello-world:0.1.9
```

</blockquote>

> ### 이미지 삭제

<blockquote>

*docker* **rmi {your-docker-image-id}**

```sh
your-terminal> docker rmi 1f1b68f35fa5
```

</blockquote>

> ### 이미지 전체 삭제

<blockquote>

docker rmi \`docker images\`

```sh
your-terminal> docker rmi \`docker images\`
```

</blockquote>

</blockquote>

> ## About container

<blockquote>

> ### 보유 컨테이너 목록 조회

<blockquote>

docker ps \[-a\]

\[-a\]

|옵션|설명|형식|예시|
|---|---|---|---|
|-a|all. 중지되었거나 구동중인 모든 컨테이너 포함|N/A|-a|

```sh
your-terminal> docker ps

your-terminal> docker ps -a
```

</blockquote>

> ### 컨테이너 실행

<blockquote>
  
docker run [options] image[:TAG|@DIGEST] [COMMAND] [ARG...]

```sh
your-terminal> docker run --rm -d -p 9090:8080 hello-api --name api
```

\[options\]

|옵션|설명|형식|예시|
|---|---|---|---|
|-d|detached mode. 백그라운드 모드|N/A|-d|
|-p|port. 호스트와 컨테이너의 포트를 연결 (포워딩)|호스트 포트:컨테이너 포트| -p 9090:8080 |
|-v|volume. 호스트와 컨테이너의 디렉토리를 연결 (마운트)|호스트 디렉토리 경로:컨테이너 디렉토리 경로|-v /your/dir/path:/var/www/http|
|-e|enviroment. 컨테이너 내에서 사용할 환경변수 설정|...|...|
|--name|컨테이너 이름 설정|컨테이너 이름|a-container thecontainer|
|--it|컨테이너의 표준 입력과 로컬 컴퓨터의 키보드 입력을 연결|N/A|-it|
|--rm|remove. 프로세스 종료시 컨테이너 자동 제거|컨테이너 ID|--rm 1f1b68f35fa5|
|--link|컨테이너 연결|컨테이너 이름:별칭|a-container:mycontainer|

</blockquote>

> ### 컨테이너 시작

<blockquote>

docker start {your-docker-container-id-or-name}

```sh
your-terminal> docker start 1f1b68f35fa5

your-terminal> docker start hello-world
```

</blockquote>

> ### 컨테이너 재시작

<blockquote>

docker restart {your-docker-container-id-or-name}

```sh
your-terminal> docker restart 1f1b68f35fa5

your-terminal> docker restart hello-world
```

</blockquote>

> ### 컨테이너 접속

<blockquote>

docker attach {your-docker-container-id-or-name}

```sh
your-terminal> docker attach 1f1b68f35fa5

your-terminal> docker attach hello-world
```

</blockquote>

> ### 컨테이너 중지

<blockquote>

docker stop {your-docker-container-id-or-name}

```sh
your-terminal> docker stop 1f1b68f35fa5

your-terminal> docker stop hello-world
```

`exit` 을 입력하거나 `control + D` 를 누르면 **컨테이너를 중지시킬 수 있음**

`control + P` ---> `control + Q` 를 순서대로 누르면 **컨테이너를 중지시키지 않을 수 있음**

</blockquote>

> ## sudo 입력없이 명령어 사용할 수 있는 팁

<blockquote>

Case 1
현재 접속중인 사용자에게 sudo 권한 부여

your-terminal> sudo usermod -aG docker $USER

Case 2
임의의 사용자에게 sudo 권한 부여
your-terminal> sudo usermod -aG docker {your-user}

</blockquote>

</blockquote>

</details>



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

