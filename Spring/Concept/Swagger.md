## **Swagger란?**
**📌 Swagger: REST API로 개발시 문서를 자동으로 만들어주는 프레임워크**
***

## **🧐 배경**
> 작은 규모의 단일 서버가 아니라면 보통의 경우.  
> **사용자 -> WEB, Mobile -> API -> DB**  
> 이런 구조에서는 API서버가 어떤 스펙으로 데이터를 주고받는지 문서가 꼭 필요.

**"But!" api 문서를 document로 관리하면 너무 귀찮은 일, 그래서 Swagger 등장**

> ### **API → Swagger Definition → Server/Client code**
> ### **API → Swagger Definition → API Document**

****

## **Swagger**
**🛠 Swagger 에서 공식적으로 지원하는 툴 리스트**

> 1. Swagger Core
>     * Swagger definition을 생성하는 library
> 
> 2. Swagger CodeGen
>    * Swagger definition을 이용하여 ```Server/Client code```를 생성하는 Command-line-tool
> 
> 3. Swagger UI
>     * REST API document web 제공
> 
> 4. Swagger Editor
>     * YAML/JSON 형태의 Swagger definition을 편집하는 editor

**Swagger 기능**
> 1. 작성된 API 기반으로 ```Swagger Definition Generation```
> 2. 작성된 API 기반으로 API Document ```Web Service -> Swagger-UI```
> 3. Swagger Definition을 기반으로 ```API Document Web Service -> Swagger-UI```
> 4. Swagger Definition 을 기반으로 ```Test Code Generation -> Swagger CodeGen```


