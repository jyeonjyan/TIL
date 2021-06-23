## h2 DB 설치 및 사용 방법
우선 h2 db 공식 사이트로 접속
> https://www.h2database.com/html/main.html

안정화 된 버전 다운로드

<img src="../img/h2DB_down.png" width="700px">

내가 좋아하는 디렉터리에 옮긴 후 실행
```
$ cd h2/
$ cd bin/
$ chmod 755 ./h2.sh
$ ./h2.sh
```

최초 실행할때
```
jdbc:h2:~/querydsl
```

디렉토리에 ~/ 디렉토리 `db` 확장자 생성 확인 후  
다음 실행에서는 아래 url 입력 (tcp 모드)
```
jdbc:h2:tcp://localhost/~/querydsl 
```