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
1. 단일 호스트에 여러 개의 격리된 환경
* Compose는 프로젝트 이름을 사용하여 서로 환경을 분리 합니다. 이 프로젝트 이름을 다른 컨텍스트에서 사용할 수 있습니다.  
* dev host 에서, 프로젝트의 브렌치의 각각 feature에 대해 안정적인 복사본을 실행할 때와 같이 각각의 단일 환경의 여러 복사본을 만듭니다.
* CI server에서, 서로 간섭하지 않도록 하기 위해 고유한 빌드 넘버로 프로젝트 이름을 셋팅할 수 있습니다.
* 공유된 host나 dev host를 사용하여 같은 서비스 이름을 사용한 다른 프로젝트에 간섭이 되는 것을 막습니다.
* 기본적인 프로젝트 이름은 프로젝트 디렉토리의 basename 입니다. 프로젝트 이름을 -p 옵션 또는 COMPOSE_PROJECT_NAME 환경변수를 사용하여 프로젝트 이름을 커스텀할 수 있습니다.

2. 컨테이너를 만들 때 볼륨 데이터를 보존함
* 서비스에 의해 사용되어 지는 모든 볼륨을 막을 수 있습니다. ``docker-compose up`` 명령어를 실행했을 때, ``docker-compose``를 실행하면 이전 실행에서 컨테이너를 찾았을 때, 이 전 컨테이너의 볼륨을 새 컨테이너로 복사를 합니다.
* 이 프로세스를 통해 볼륨에서 만든 데이터가 손실되지 않도록 합니다.
* ``docker-compose`` 명령어를 윈도우에서 실행한다면 환경변수 설정이 필요할 수 있습니다.

3. 변경된 컨테이너만 다시 작성함
* Compose가 컨테이너를 생성하기 위해서 설정을 캐싱해놓습니다. 
* 변경되지 않은 서비스를 재시작 하였을 때 Compose 는 기존 컨테이너를 다시 사용합니다.
* 컨테이너를 다시 사용한다는 것은 환경을 매우 빠르게 변경 할 수 있다는 것을 의미합니다.

4. 환경 간 구성 변수 및 이동
* Compose는 Compose file 에서 변수를 지원합니다. 이러한 변수를 사용하여 다른 환경 또는 다른 사용자에 대한 Compose 를 상요자 정의 할 수 있습니다.
* extends 필드를 사용하거나 여러 작성 파일을 작성하여 작성된 파일들을 extends 할 수 있습니다.

5. 개발환경
* 소프트웨어를 개발할 때, 단일의 환경에서 애플리케이션을 실행하고 상호 작용하는 능력은 매우 중요 합니다. Compose 명령어는 환경을 만들고, 이것이 상호작용하는데 사용되어 집니다.
* Compose file은 문서를 작성하는 방법과 모든 서비스의 종송적인 환경(db, queue, caches, web service API)를 제공합니다.
* Compose는 단일 시스템 Compse 파일과 몇 가지 명령어로 다양한 개발 시작 가이드를 줄일 수 있습니다.(개발 환경 셋팅 가이드가 간편해 질 수 있다.)

6. 자동화된 테스트 환경
* CD(Continuous Deployment) OR CI(Continuous Integration)프로세스의 중요한 부분은 자동화된 테스트 입니다.
* 자동화된 end-to-end 테스트에서는 테스트를 실행할 환경이 필요 합니다. Compose는 테스트를 위한 독립적인 환셩을 생성하고 destroy 하는 방법을 제공합니다.
* Compose file 에서 모든 환경을 정의함으로써, 이러한 환셩을 생성하고 destroy 할 수 있습니다.