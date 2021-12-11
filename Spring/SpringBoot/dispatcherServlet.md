# Dispatcher Servlet이 대체 뭐하는 녀석인다.
> 나는 예전에 Spring MVC 동작 원리를 설명한적이 있다.  
> 하지만 Dispatcher Servlet이 기술면접에서도 자주 등장하는 녀석이라 깔끔하게 짚고 넘어가려고 한다.

우선 springboot 를 공부하자니 AOP, PSA, Application Context 등 너무 방대한 용어들 사이에 DispatcherServlet 도 그 중 하나이다.

간략하게 정리하자면 DispatcherServlet 은 "개발자에게 서블릿 컨텍스트 구현의 책임이 아닌 스프링 컨텍스트 구현의 책임을 주는 것"이다.

더 자세하게 알아보자..