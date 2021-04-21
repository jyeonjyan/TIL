## 스프링부트 도커에 올리기

index
* Dockerfile
* Docker Build
* Docker Run

## Dockerfile
도커가 이해할 수 있는 Dockerfile이라는 것응ㄹ 만들어 줘야 한다  
어떤 자바를 사용 할 것인지, 어떤 어플리케이션을 사용 할 것인지, 어떤 명령어로 이 도커 컨테이너를 실행시켜야 되는지  
<br>

<ins>일단 SpringBoot root 디렉토리에 ``Dockerfile``을 만들자.</ins>

```Dockerfile
# Start with a base image containing Java runtime
FROM java:8

# if you want to use java 11,
FROM centos
RUN yum install -y java-11

# Add Author info
LABEL maintainer="authinfo@gmail.com"

# Add a volume to /tmp
VOLUME /tmp

# Make port 8080 available to the world outside this container
EXPOSE 8080

# The application's jar file [jar file이 있는 위치]
ARG JAR_FILE=build/libs/MySpringApp-0.0.1-SNAPSHOT.jar

# Add the application's jar to the container [어떤 이름으로 하고 싶은지 지정]
ADD ${JAR_FILE} to-do-springboot.jar

# Run the jar file
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/to-do-springboot.jar"]
```

<ins>Docker build</ins>

이제 작성한 도커파일로 도커 이미지를 만들어야 한다.  
도커 이미지를 만드는 명령어는 간단하다 ``Dockerfile``이 존재하는 디렉토리 아래에서 실행시켜야 한다는 점을 명심해라.  

```shell
$ cd */
$ docker build -t {my jar name} .
```

``Successfully tagged...``로 끝나면 완료 된 것이다.

<ins>Docker Run </ins>

* Docker images 가 제대로 만들어 졌는지 확인

```shell
$ docker images
```

* Docker image 를 이용해 도커 컨테이너를 실행 해 보자.

```shell
$ docker run -p 5000:8080 {my jar name}
```

  * 이렇게 하면 개발당시에는 8080을 사용하고 도커에 올리고 나서는 5000포트를 이용 할 수 있다는 장점이 있다.

