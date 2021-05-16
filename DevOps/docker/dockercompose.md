## 🐳 dockerCompose 란

### Compose 란?
복수개의 컨테이너를 실행시키는 도커 애플리케이션이 정의를 하기 위한 툴이다.  
Compose는 모든 환경(workflow에서 production, staging, development, testing)및 CI(워크플로우)를 포함한다.

<br>

기본적으로 3스텝의 프로세스를 사용한다. 
1. 앱의 환경을 정의하여 어디에서나 재사용할 수 있는 Dockerfile을 정의합니다.  
2. ``docker-compose.yml`` 에서 앱을 구성할 수 있는 서비스를 정의합니다.
3. ``docker-compose up`` 명령어를 실행합니다. 그리고 Compose를 시작시키고 전체의 앱을 실행시킵니다.

Sample ``docker-compose.yml``

```yml
version: '3'
services:
  web:
    build: .
    ports:
    - "5000:5000"
    volumes:
    - .:/code
    - logvolume01:/var/log
    links:
    - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

### Compose 특징
* Compose는 프로젝트 이름을 사용하여 서로 환경을 분리 합니다. 이 프로젝트 이름을 다른 컨텍스트에서 사용할 수 있습니다.  
* dev host 에서, 프로젝트의 브렌치의 각각 feature에 대해 안정적인 복사본을 실행할 때와 같이 각각의 단일 환경의 여러 복사본을 만듭니다.
* CI server에서, 서로 간섭하지 않도록 하기 위해 고유한 빌드 넘버로 프로젝트 이름을 셋팅할 수 있습니다.