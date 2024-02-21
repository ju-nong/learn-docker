# learn-docker
TB 서버에 도커를 띄우는 작업이 있어서 공부하게 되었다

목표  - nginx와 react로 이루어진 컨테이너 띄우기
<br/>

### Docker 정의
대강 아는데 누군가에게 알려줄 정도로 이해하진 못해서, 실습을 한 뒤 적을 예정이다

Docker - 프로그램을 환경과 격리하여 관리하는 도구 <br/> 
Image - 컨테이너를 만드는데 사용되는 템플릿 <br/>
Container - 실행 가능한 인스턴스(OS, Server, Build Code 등) <br/>
Docker Compose - 여러 Container를 결합해서 실행해주는 도구 <br/>
<br/>

### Docker 시작하기
OS - `Window 10 Pro`

1. Docker Desktop 설치
2. Windows 기능 켜기/끄기 -> Hyper-V 체크 해제
3. cmd에서 `docker run -d -p 80:80 docker/getting-started` 입력 -> 웹 브라우저에서 localhost 접속 확인
<br/>

### Docker Compose 설치
Docker Desktop 설치하면 자동으로 설치 된다는 썰이 있다, 나중에 또 설치하게 된다면 설치하기 전에 `docker-compose.exe version` 명령어를 쳐보자

1. PowerShell 관리자 권한 실행
2. `Start-BitsTransfer -Source "https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-windows-x86_64.exe" -Destination $Env:ProgramFiles\Docker\docker-compose.exe` 명령어 실행
3. `docker-compose.exe version` 명령어 실행으로 버전 나오는지 확인
<br/>

### 명령어
`docker run -d -p 80:80 docker/getting-started` - Docker hub에 있는 docker/getting-started는 이미지를 갖고 와서 컨테이너를 실행 시킴<br/>
`docker ps` - 실행 중인 컨테이너를 봄<br/>
`docker ps -a` - 중지된 컨테이너도 봄<br/>
`docker images` - pull 받은 이미지들을 봄<br/>
`docker pull openjdk` - Docker hub에 있는 openjdk 이미지를 pull 받음<br/>
`docker pull ubuntu:18.04` - 이미지를 18.04 버전으로 pull 받음<br/>
`docker rmi openjdk` - 이미지를 삭제함(이미지명 말고 IMAGE ID로도 삭제 가능함)<br/>
`docker stop [CONTAINER ID]` - 컨테이너를 중지시킴(ps로 컨테이너 아이디 확인 가능)<br/>
`docker rm [CONTAINER ID]` - 중지된 컨테이너 삭제<br/>
`docker run -d -p 8080:80 httpd` - httpd 컨테이너를 데몬으로 실행하고 외부에서 8080포트로 들어오면 내부 80포트로 포트포워딩 해줌
<br/>

### tomcat 컨테이너 실행
1. `docker pull tomcat` 톰캣 이미지를 받아옴
2. `docker run -d tomcat` 톰캣 컨테이너를 실행시킴(-d는 백그라운드 즉 데몬으로 실행 시키는 옵션)

tomcat 컨테이너를 8080포트로 실행 시켜도 localhost:8080로 접근해보면 안 뜬다.<br/>
이유는 웹 브라우저는 호스트 운영체제에 8080포트를 접근하기 때문이다.<br/>
그래서 호스트 운영체제에서 8080포트로 띄워진 tomcat 컨테이너로 포트포워딩을 해줘야 된다.<br/>

### 컨테이너 포트포워딩
1. `docker run -d -p 8080:80 httpd` 외부에서 8080포트로 들어오면 내부에 80포트인 httpd 연결
2. `docker run -d -p 8081:80 nginx` 외부에서 8081포트로 들어오면 내부에 80포트인 nginx 연결
<br/>

### ubuntu 컨테이너 실행 및 접근
`docker run --name myubuntu ubuntu` myubuntu라는 이름으로 ubuntu를 실행 시켜준다.<br/>
하지만 OS에 해당하는 ubuntu는 tomcat이나 nginx 같은 데몬 프로세스가 없기 때문에, 일반적인 실행 명령어를 사용할 경우 컨테이너가 중지된다<br/><br/>

실행 명령어에 인터렉션 한다는 i와 터미널 모드로 실행하겠다는 t를 붙여주면 된다.<br/>
`docker run -dit --name myubuntu ubuntu`<br/>
실행된 ubuntu에 접근하는 방법은 `docker attach [CONTAINER ID]`
