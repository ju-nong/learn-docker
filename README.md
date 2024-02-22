# learn-docker

TB 서버에 도커를 띄우는 작업이 있어서 공부하게 되었다

목표 - nginx와 react로 이루어진 컨테이너 띄우기
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
`docker start [CONTAINER ID]` - 중지된 컨테이너 실행<br/>
`docker rm [CONTAINER ID]` - 중지된 컨테이너 삭제<br/>
`docker run -d -p 8080:80 httpd` - httpd 컨테이너를 데몬으로 실행하고 외부에서 8080포트로 들어오면 내부 80포트로 포트포워딩 해줌<br/>
`docker logs [CONTAINER ID]` - 컨테이너 로그 출력<br/>
<br/>

### tomcat 컨테이너 실행

1. `docker pull tomcat` 톰캣 이미지를 받아옴
2. `docker run -d tomcat` 톰캣 컨테이너를 실행시킴(-d는 백그라운드 즉 데몬으로 실행 시키는 옵션)

tomcat 컨테이너를 8080포트로 실행 시켜도 localhost:8080로 접근해보면 안 뜬다.<br/>
이유는 웹 브라우저는 호스트 운영체제에 8080포트를 접근하기 때문이다.<br/>
그래서 호스트 운영체제에서 8080포트로 띄워진 tomcat 컨테이너로 포트포워딩을 해줘야 된다.<br/>
<br/>

### 컨테이너 포트포워딩

1. `docker run -d -p 8080:80 httpd` 외부에서 8080포트로 들어오면 내부에 80포트인 httpd 연결
2. `docker run -d -p 8081:80 nginx` 외부에서 8081포트로 들어오면 내부에 80포트인 nginx 연결
   <br/>

### 운영체제 컨테이너 실행 및 접근

`docker run --name myubuntu ubuntu` myubuntu라는 이름으로 ubuntu를 실행 시켜준다.<br/>
하지만 OS에 해당하는 ubuntu는 tomcat이나 nginx 같은 데몬 프로세스가 없기 때문에, 일반적인 실행 명령어를 사용할 경우 컨테이너가 중지된다<br/><br/>

실행 명령어에 인터렉션 한다는 i와 터미널 모드로 실행하겠다는 t를 붙여주면 된다.<br/>
`docker run -dit --name myubuntu ubuntu`<br/>
실행된 ubuntu에 접근하는 방법은 `docker attach [CONTAINER ID]`<br/>
<br/>

### 프로세스 컨테이너 실행 및 접근

`docker run -d -p 8080:80 httpd`로 실행 시켜준다.<br/>
`docker exec -it [CONTAINER ID] bash` bash 명령어로 접근한다.<br/>
<br/>

### Volume 옵션

호스트의 파일 또는 폴더를 컨테이너내의 파일 시스템과 공유할 수 있는 옵션이다.<br/>
명령어 원문 - `docker run -d -p 8080:80 -v [HOST DIR PATH]:[CONTAINER DIR PATH] httpd`<br/>
ex) `docker run -d -p 8080:80 -v C:\Users\gnew\Desktop\docker\webapp:/usr/local/apache2/htdocs httpd`<br/>
<br/>

### ubuntu 컨테이너에서 vi 설치 및 터미널 나가기

1. `docker run -dit ubuntu` - ubuntu 설치 및 실행
2. `docker attach [CONTAINER ID]` - ubuntu 접속
3. `apt update` - ubuntu 프로그램 업데이터
4. `apt install vi` - vi 설치
5. `Ctrl` + `p` + `q` - ubuntu 나오기
   <br/>

### 레포지토리 만들기 및 이미지 굽기

Git이랑 비슷하다

1. Docker hub에 접속해서 Create Repository
2. `docker commit [CONTAINER ID] [MY ID/REPOSITORY NAME]:1.0` - 실행 중인 컨테이너 아이디와 커밋할 내 아이디 및 레포지토리명을 입력하고 버전(TAG)을 적어준다
3. `docker images` - 이미지 생성 확인
4. `docker push [MY ID/REPOSITORY NAME]:1.0` - 푸시
5. `docker pull [MY ID/REPOSITORY NAME]:1.0` - 풀
   <br/>

### Docker File

도커 이미지를 작성할 때 스크립트 짜는 것을 Docker File이라고 부른다.

```dockerfile
# FROM [이미지명]:[TAG]
FROM openjdk:11-jdk-slim

# WORKDIR [도커 진입 경로]
WORKDIR /app

# COPY [복사할 호스트 경로] [붙여넣기할 도커 경로]
COPY ./build/aws-v3-0.0.3.jar ./application.jar

# ADD도 COPY랑 인자는 같은데 압축 파일 같은 경우 압축이 풀린다

# ENTRYPOINT [명령어 나열]
# 실행 명령어
ENTRYPOINT ["java", "-jar", "-Dspring.profiles.active=dev", "application.jar"]

# CMD [명령어 나열]
# 빌드할 때 CMD 명령어를 다시 작성할 경우 덮어쓰기됨
CMD ["--server.port=3000"]
```

`docker build -t [생성할 이미지명] [Dockerfile 경로]` - Dockerfile를 실행
