## 노션사이트 내 도메인 주소로 redirect 하기.
> [도움을 주셔서 감사합니다 🙇🏻‍♂️](https://fruitionsite.com/)

1. CloudFlare 에 가입해서 DNS를 등록해준다.
2. CloudFlare의 네임서버를 도메인 관리 서비스에 등록해준다.
3. 연결을 원하는 도메인을 CNAME 으로 생성하고 대상을 notion.so 로 설정해준다.
4. 위에 안내한 사이트에 따라 script를 worker 로 만들어 등록해주고
5. 생성한 도메인에 그 worker을 등록해준다.