# Error vs Exception
## 👩🏻‍🏫 오류와 예외의 개념
### Error vs Exception
오류: 시스템에 비정상적인 상황이 생겼을 때 발생. 심각한 수준의 오류. 예측 처리 불가.  
예외: 개발자가 구현한 로직에서 발생. 예측 처리 가능. 예회를 구분하고 명확한 처리 방법을 적용.

### Exception class
<img src="https://www.nextree.co.kr/content/images/2021/01/Exception-Class.png">

Exception은 수 많은 자식클래스를 갖고 있다.  
**```RuntimeException```이 중요하다, ```CheckedException``` 과 ```UncheckedException```을 구분하는 기준이 되기 때문에.**

### CheckedException과 Unchecked(Runtime)Exception
<img src="https://www.nextree.co.kr/content/images/2021/01/exception-table.png">

CheckedException 과 UncheckedException의 가장 명확한 기준은 "꼭 처리를 해야 하느냐" 이다.  
Checked Exception이 발생할 가능성이 있는 method는 ```try/catch```로 감싸거나 ```throw```로 던져야 한다.  
반면에 UncheckedException은 명시적인 예외처리를 하지 않아도 된다. 개발자의 부주의에서 발생하는 경우가 대부분이고 미리 예측하지 못했던 상황에서 발생하는 예외가 아니기에.