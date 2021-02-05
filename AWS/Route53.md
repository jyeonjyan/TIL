# Amazon Routes 53
## AWS Community Day - Route53 - 서태호

### **1. Route 53 이란?**
AWS에서 제공하는 DNS(Domain Name System)이다.  
Route 53 =  일반 DNS 기능(네임서버) + 모니터링기능 + L4기능(SLB) + GSLB 기능을 지원한다.  

### **2. 일반 DNS에 대한 이해**
**DNS 동작과정은 네트워크 통신을 하기위해 IP를 찾아가는 과정이다.**
<img src="https://www.internetsociety.org/wp-content/uploads/2019/02/dnsprivacy2-450x335.gif">

### **3. 일반 DNS와 차이점**

**-일반 DNS**  
도메인 등록 시 네임서버를 지정한다.  
```mattzip.com``` 도메인 등록시, 등록 대행기관에 네임서버를 등록한다.
<img src="https://i.ytimg.com/vi/hfj0IGgKAgU/maxresdefault.jpg">

**-Route 53**  
Route 53 사이트에서 네임서버를 할당받은 후,  
도메인등록 대행기관 사이트에 접속해 정보를 등록한다.  
<img src="https://www.ictshore.com/wp-content/uploads/2019/08/cld0002-04-route_53_registered_domains.png">

> Route 53 에는 레코더 ALIAS(별칭)이 있다.  
> 도메인 자체에 ALIAS를 줄 수 있다.  

**ex) ```mattzip.com```을 치면 ```www.mattzip.com``` 과 같이 응답하게 해주는것이 가능하다.**

> Route 53은 모니터링 기능이 있다.  
> Route 53은 L4 기능이 있다.  

<img src="https://t3.daumcdn.net/thumb/R720x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/uSr/image/0XeFNHDfU4OMw8DjzYIWOJNWAhw.png">

> Route 53의 GSLB 기능  

<img src="https://t4.daumcdn.net/thumb/R720x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/uSr/image/h-pf_e2FfD4NT7Bz1Du1lnKAIRs.png">

이외에도,  
Route 53 Failover  
ex) If A route is dead, B route Backup  
<br>

Route 53 Act-Act(트래픽 조절)

등 등 등 등