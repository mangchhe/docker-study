## Docker 기본 명령어

### 이미지 다운로드

``` bash
docker pull [저장소명:태그]
```

### 이미지 빌드

``` bash
docker build --tag [이미지명:태그] [Dockerfile PATH]
```

### 이미지 공유

``` bash
docker push [이미지명:태그]
```

### 이미지 조회

``` bash
docker image ls
docker images
```

### 이미지 삭제

- 이미지 id 대신 `$(docker images -q)`를 주게 될 경우 모든 이미지 삭제

``` bash
docker rmi [이미지 id]
```

***

### 컨테이너 (재)시작

``` bash
docker start [컨테이너 id | name]
docker restart [컨테이너 id | name]
```

### 컨테이너 생성 & 실행

- 이미지가 없을 경우 자동으로 pull 받고 생성후 실행

``` bash
docker run [옵션] [이미지명]
```

### 컨테이너 정지

``` bash
docker stop [컨테이너 id | name]
```

### 컨테이너 조회

- -a : 정지된 컨테이너까지 조회

``` bash
docker container ls
docker ps
```

### 컨테이너 삭제

- 컨테이너 id 대신 `$(docker ps -a -q)`를 주게 될 경우 모든 컨테이너 삭제

``` bash
docker rm [컨테이너 id | name]
```

### 컨테이너 옵션

|option|description|
|:---:|:---:|
|-d|detached mode, 백그라운드 모드|
|-p|호스트와 컨테이너의 포트를 연결(포워딩)|
|-v|호스트와 컨테이너 디렉토리를 연결(마운트)|
|-e|컨테이너 내에서 사용할 환경변수 설정|
|--name|컨테이너 이름 설정|
|-rm|프로세스 종료시 컨테이너 자동 제거|
|-it|interactive & tty 줄임말, 터미널 입력을 위한 옵션|

예)

``` bash
docker run -d -p 13306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
```

***

### 네트워크 종류

- bridget
  - 하나의 호스트 컴퓨터 내에서 여러 컨테이너들이 서로 소통할 수 있도록 함
- host
  - 호스트의 네트워크 환경을 그대로 사용함
- none
  - 네트워크를 사용하지 않음

### 네트워크 조회

``` bash
docker network ls
```

### 네트워크 생성

- --gateway <IP> : 동일 LAN에 존재하지 않는 다른 네트워크와 통신할 IP 설정
- --subnet <IP> : 동일 LAN 네트워크 IP 범위 설정

``` bash
docker network create
docker --gateway 172.18.0.1 --subnet 172.18.0.0/16 <네트워크명>
```

### 네트워크 상세 정보

``` bash
docker network inspect
```

### 컨테이너 네트워크 연결

``` bash
# 이미 생성된 컨테이너에 네트워크 연결
docker network connect <네트워크명> <컨테이너명>
# 컨테이너 생성과 동시에 네트워크 연결
docker run --network <네트워크명>
```

***

### Dockerfile

#### FROM

- 어떤 이미지를 기반으로 새로운 이미지를 생성할 것인지를 나타낸다.

#### RUN

- bash 쉘에서 입력하는 것과 동일하며 이미지가 만들어질 때 한번 실행되는 명령어이다.

#### ADD | COPY

- 호스트의 파일 시스템으로부터 파일을 가져온 것이다.
- 하는 기능은 동일하고 ADD가 COPY보다 더 추가적인 기능을 제공한다. URL을 source로 사용할 수 있고 압축형태인 경웅에 압축을 풀어준다. 단 remote파일의 경우에는 압축을 풀어주지 않는다.

#### ENTRYPOINT | CMD

- 컨테이너가 시작되었을 때 스크립트 혹은 명령을 실행한다.
- ENTRYPOINT가 명령을 수행되었을 때에는 **반드시 ENTRYPOINT에서 지정한 명령을 수행**하고 CMD 같은 경우는 컨테이너 실행 시에 인자 값을 주게 되면 Dockerfile에 **지정된 CMD 값 대신하여 인자값이 변경되어 실행**된다.

ex)

``` text
FROM openjdk:17-ea-11-jdk-slim
VOLUME /tmp
COPY target/user-service-1.0.jar UserService.jar
ENTRYPOINT ["java", "-jar", "UserService.jar"]
```
