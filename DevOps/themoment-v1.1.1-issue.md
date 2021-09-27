## 학교가 불편한 순간 - 서버 v1.1.1 트러블슈팅 여정기 - redis

#### docker redis unable to connect
[🥕 여기를 참고했습니다](https://github.com/luin/ioredis/issues/763#issuecomment-451041838)  
하지만.. 왜 아직도 connect 가 안되는거야? 똑같은 error log 가 발생하네? 해서 docker-compose 자체에도 cache 가 있는지 구글링을 하게 됩니다.  

여기서!!! 원하는 답을 얻게됩니다.!!
[🥕 여기를 참고했습니다](https://medium.com/sjk5766/docker-compose-cache-%EC%97%86%EC%9D%B4-%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-4ac3561815be)

바로 아래와 같은 command 를 사용하여 다시 실행해보기로 하는데요..!!
```sh
$ docker-compose build --no-cache
```

결과는 success!! 그래서 바로 `docker-compose-env.sh` 라는 저희가 만든 자동 실행 쉘스크립트 맨 처음에 넣어주게 됩니다!!

