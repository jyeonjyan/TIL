# CI/CD
## CI (Continuous Integration)
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGXdIT%2FbtqI9GkH3wP%2F5Qx2zLKYRxsYWLSoS6KH3K%2Fimg.png">

### CI는 Continuous Integration 즉, 지속적인 통합이라는 의미입니다.  
지속적인 통합이란,  
애플리케이션의 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트 되어  
공유 레포지토리에 통합되는 것을 의미합니다. (가능하다면 하루에 여러번까지)

<br>

**-다수의 개발자가 형상관리 툴을 공유하여 사용하는 환경**  
Git, SVN 등을 사용하는 경우인데  
지속적으로 서비스해야 하는 app이나, 현재 개발중인 app은  
기능 추가 시마다 commit을 날려 repo에 버전 업데이트를 한다.  
다수의 개발자가 한 팀으로 작업한다면 그만큼, commit의 양이 많다.  
그럴 때마다, 기능별로 빌드/테스트/병합 까지 하려면 상당히 번거로운 일이다.  

<br>

**이런상황에서, 자동화된 빌드 & 테스트는 원천 소스코드의 충돌 등을 방어하는 혜택을 제공한다.**  

<br>

**- MSA(Micro Service Archietecture)환경**  
MSA는 작은 기능별로 서비스를 잘게 쪼개어 개발하는 형태를 의미한다.  
MSA 환경에서는 Agile 방법론이 적용되기 때문에, 기능 추가가 매우 빈번하게 일어난다.  
작은 Micro Service 의 긴밀한 동작 테스트도 중요하다.  
그런 상황에서의 CI 적용은 기능 충돌 방지 혜택을 제공한다.  


### CI의 핵심 목표는
> 버그를 신속하게 찾아 해결하고,  
> 소프트웨어 품질을 개선하고,  
> 새로운 업데이트 검증 및 릴리즈 시간을 단축시킨다.  

## CD (CD (Continuous Delivery & Continuous Deployment)
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeeSLmu%2FbtqI9pXqCN8%2FiIopSPh3KSK1SwhRjkWPf1%2Fimg.png">

### **지속적인 서비스 제공, 지속적인 배포**
CD는 개발자의 변경 사항이 Repo를 넘어, Production 환경까지 릴리즈 되는 것을 의미한다.  
소프트웨어가 언제든지 신뢰 가능한 수준의 버전을 유지할 수 있도록 support 하는 것이 CD라고 할 수 있다.  

이런 서비스의 개발팀과 비즈니스팀(영업, CS팀 등)간의 커뮤니케이션 부족 문제를 해결해 줌으로써, 배포에 이르기까지의 노력을 최소한으로 단축시켜 준다는 Benefit을 제공한다.

## DevOps 의 역할
**CI/CD 자동화는 DevOps 핵심 업무에 속한다.**  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3WY5f%2FbtqI5zz0OUH%2FN5KhjwQ3SP9nYplyZrXuVK%2Fimg.png">

이처럼, DevOps 엔지니어는  
CI/CD를 위한 파이프라인을 구성하고, 이를 자동화 단계까지 끌어 올린다.  
또한 중간중간 모니터링 지표를 구성하여, 개발자들의 개발 방향을 가이드한다.  
