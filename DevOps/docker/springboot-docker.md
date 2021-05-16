## Springboot 프로젝트를 도커로 실행시키기

### 🐳 Dockerfile 만들기
여기서 주의사항 "무조건 SpringBoot_app root 에 ``Dockerfile`` 만들기"

```
$ vim Dockerfile
```

Dockerfile 내용

```dockerfile
# docker start with a base image containing java runtime
FROM openjdk:11

# Add Author information
LABEL maintainer="s20062@gsm.hs.kr"

# Add a volume to /tmp
VOLUME /tmp

# Make port 8080 available to the world outside this container
EXPOSE 8080

# The application's jar file
ARG JAR_FILE=target/the-0.0.1-SNAPSHOT.jar

# Add the application's jar to the container
ADD ${JAR_FILE} the_moment_server.jar

# Run the jar file
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/the_moment_server.jar"]

```

해석하자면..  
> 1. openjdk:11 을 사용할것이다.
> 2. maintainer 이메일은 "s20062@gsm.hs.kr" 이다.
> 3. 8080 포트로 바인딩 할 것이다.
> 4. JAR 파일 위치는 여기다.
> 5. 이 JAR 파일을 컨테이너로 만들어주세요.

### 📦 사용방법
* 빌드하기
```
$ docker build -t dockerhub ID/project_name:tag .
```
* 실행시키기
```
$ docker run -name ConatinerName -d -p 5000:8080 dockerhub ID/project_name:tag
```
* 배포확인
```
$ (sudo) docker ps 
```