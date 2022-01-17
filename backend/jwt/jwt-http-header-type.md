# BEST HTTP Authorization header type for JWT

## 개요
JWT 예제마다, token 기반 인증/인가 예제마다. 인증 토큰에 가장 적합한 HTTP 헤더 유형이 무엇인지 궁금해서 알아본 내용을 공유한다.  
이미 나와 같은 생각을 많은 분들이 하셔서 [링크](https://bit.ly/3tAfjFe) 걸어둔다. (해당 내용을 발췌해서 만든다.)

## 본론
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
