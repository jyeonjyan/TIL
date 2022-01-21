# Sentry를 통해 로깅 시스템 구축하기

자세한 initialize는 [Sentry 공식 페이지](https://sentry.io/welcome/)를 참고하자.

## 환경설정 DSN 주소를 public 으로 설정해도 되나요?

Sentry에서 DSN은 Data Source Name 으로 Sentry에 새로운 프로젝트를 생성해서 모니터링 하고 싶을 때. 자신의 어플리케이션  이벤트를 Sentry 프로젝트에 전송 해주는 역할을 한다.

결론적으로 DSN은 내 애플리케이션에서 발생한 이벤트를 내 Sentry 프로젝트에 보내주는 역할을 하는 것이다.

이 DSN에 대해서 VCS에 public으로 commit 할것이냐 private으로 commit 할 것이냐에 대해 의견이 분분하다.

### 자세한 내용은 [-> Sentry 포럼 참고](https://forum.sentry.io/t/dsn-private-public/6297)  
여기서도 user 한분이 언급한다 VCS(git)에 올리게 되면 모든 사람이 DSN을 볼 수 있는데 이걸 비공개로 유지해야 하는지 어떻게 해야 하는지.  

하지만 Sentry 개발자는 DSN을 굳이 private 할 필요가 없다고 한다.