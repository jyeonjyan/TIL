# BEST HTTP Authorization header type for JWT

## 개요
JWT 예제마다, token 기반 인증/인가 예제마다. 인증 토큰에 가장 적합한 HTTP 헤더 유형이 무엇인지 궁금해서 알아본 내용을 공유한다.  
이미 나와 같은 생각을 많은 분들이 하셔서 [링크](https://bit.ly/3tAfjFe) 걸어둔다. (해당 내용을 발췌해서 만든다.)

## JWT HTTP header 
클라이언트가 엑세스 토큰을 보내는데 가장 적합한 HTTP header는 인증 체계가 있는 `Authorization` header 라고 한다.  
이 체계는 [RFC-6750](https://www.rfc-editor.org/rfc/rfc6750)에 의해 설명됩니다.

예시:
```
GET /resource HTTP/1.1
Host: server.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIXVCJ9TJV...r7E20RMHrHDcEfxjoYZgeFONFh7HgQ
```

(예: 본문 요청 또는 쿼리 문자열)을 사용하여 JWT를 자유롭게 수락할 수 있지만, 헤더가 더 적절하고 HTTP 1.1 컨텍스트에서 인증 프레임워크를 설명하는 RFC7235 `Authenticate`를 준수합니다.  

### JWT에서도 Bearer 스키마를 사용한 인증 헤더 방식을 권장합니다.

<img src="../../img/authentication-jwt.png" width="650px">

## JWT secret key

jwt secret key는 HS512 알고리즘을 사용할 것이기 때문에 512bit, 즉 64byte 이상의 secret key를 사용해야 한다.  
[Base64 String](https://generate.plus/en/base64) <- 해당 링크는 Base64 문자열을 byte length에 따라 생성해주는 사이트이다.

예시:
```
jwt.secret: 'BQby6J0BWgJHTF0yE5OO8/+CMSqFINbh7LbiLNno8yVCt787DdsfTDOAjfMrHL46SKjwB8oj6XMAGdwGcs+ZgA=='
```

## JWT가 Base 64 512bit encoding 을 사용하는 이유. 
> 이런 질문은 던지는건 굉장히 단순하지만, jwt를 사용하는 개발자는 꼭 짚고 넘어가야 하는 개념이라고 생각한다. [stackoverflow](https://stackoverflow.com/questions/58341833/why-base64-is-used-in-jwts)

ASCII 7bits encoding을 사용하여 라이트하게 데이터를 전송할 수 있는데 왜 Base64 512 bit를 굳이 사용할까?  

### 대표적인 문제는 아래와 같이 설명한다.

* ASCII는 7 bits 인토딩인데 나머지 1bit를 처리하는 방식이 시스템 별로 상이하다. 
* 일부 제어 문자의 경우 시스템 별로 다른 코드값을 가진다.

Base64는 ASCII 중 제어 문자와 일부 특수문자를 제외한 64개의 안전한 출력 문자만 사용한다. (아래 그림을 참고)

<img src="../../img/base64.png" width="650px">