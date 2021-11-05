# springboot app 요청 client ip 확인하기.

## request.getRemoteAddr();
기본적으로 HttpServletRequest 의 `.getRemoteAddr()` 메소드로 클라이언트 IP를 가져올 수 있다.  
하지만 보안관련해서 방화벽이나 클라우드를 운영하는 경우 클라이언트의 원 IP주소를 가져올 수 없다.(프록시들에 의해 타켓이 달라짐)  

이유는, 클라이언트가 요청을 하면 WebServer에서 프록시나 로드 밸런서를 통해 WAS에 요청하기 때문에 프록시나 밸런서의 IP주소만을 담게 되기 때문이다. 그래서 원 IP를 그대로 못가져오는 이슈가 발생한다.

#### 이슈가 발생할 수도 있는 IP getter
```java
public IdolData getList(@RequestParam String name, HttpRequestServlet request) {
    request.getRemoteAddr(); // IP 주소?
}
```

아까 말했듯 이렇게 해서 IP를 받아온다면, 로컬에서는 IPv6 로 IP 주소를 반환합니다.  
하지만, 서버에서는 127.0.0.1을 받아오는 이슈가 있었습니다.  

우리는 대부분 proxy 환경, docker등에서 어플리케이션을 구동시키기에 발생하는 이슈 입니다.  
이럴 때 사용하는것이 `X-Forwared-For` 입니다.

## X-Forwared-For 말고도..
```
X-Forwarded-For
Proxy-Client-IP
WL-Proxy-Client-IP
HTTPXFORWARDED_FOR
HTTPXFORWARDED
HTTPXCLUSTERCLIENTIP
HTTPCLIENTIP
HTTPFORWARDEDFOR
HTTP_FORWARDED
HTTP_VIA
REMOTE_ADDR
```
이러한 header 들도 봐둬야 할 필요가 있습니다.

## solution
```java
public class HttpReqRespUtils {

    private static final String[] IP_HEADER_CANDIDATES = {
        "X-Forwarded-For",
        "Proxy-Client-IP",
        "WL-Proxy-Client-IP",
        "HTTP_X_FORWARDED_FOR",
        "HTTP_X_FORWARDED",
        "HTTP_X_CLUSTER_CLIENT_IP",
        "HTTP_CLIENT_IP",
        "HTTP_FORWARDED_FOR",
        "HTTP_FORWARDED",
        "HTTP_VIA",
        "REMOTE_ADDR"
    };

    public static String getClientIpAddressIfServletRequestExist() {

        if (Objects.isNull(RequestContextHolder.getRequestAttributes())) {
            return "0.0.0.0";
        }

        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        for (String header: IP_HEADER_CANDIDATES) {
            String ipFromHeader = request.getHeader(header);
            if (Objects.nonNull(ipFromHeader) && ipFromHeader.length() != 0 && !"unknown".equalsIgnoreCase(ipFromHeader)) {
                String ip = ipFromHeader.split(",")[0];
                return ip;
            }
        }
        return request.getRemoteAddr();
    }
}
```

## 결론
무조건 controller에서 받아온 Request Header의 remote address를 사용해서는 안된다.  
내 서버 상황에 맞는 (proxy 유무)를 확인하여 필요한 header의 값을 가져오자.