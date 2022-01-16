# Node 진영 개발자가 아닌 사람이 Gatsby를 써야할 때 겪을 수 있는 에러 해결책
> Gatsby로 기술블로그 쓰려고 node을 조금 만졌는데.. 중간에 다양한 에러가 발생해서 나중을 위해 기록한다.

## 노드 버전을 업데이트 하자!

<img src="../../img/node-update-need.png" width="700px">

> 나도 [이 이슈](https://github.com/zoomkoding/zoomkoding-gatsby-blog/issues/16)와 같은 문제를 겪었다. 🥲

`$ node -v`를 통해 노드 버전을 확인하고 latest 버전이랑 너무 멀면 아래 명령어로 업데이트 하자. [참고](https://stackoverflow.com/a/47909570)

#### Ubuntu Linux / Mac

```sh
sudo npm install n -g
sudo n stable
```

## Gatsby template 모듈을 모두 설치하자.

<img src="../../img/webpack-error-module-not-found.png" width="800px">

처음에는 이렇게 `emotions/react` 관련 not found 에러만 뜨길레 해당 부분만 설치했는데. 나중에는 다른 것(react, styled)도 뜸.  
[stackoverflow](https://stackoverflow.com/a/70479813)에서 어느 분이 all stop solution을 공유해주셨는데 그냥 이거 쓰면 될 것 같다.

#### all stop solution
```sh
npm install @mui/material @emotion/react @emotion/styled
```

## Gatsby blog 내용이 변경될 때 마다 자동 재시작 하기
> 잦은 변경이 발생하는 환경에서는 로컬 CPU의 부하를 줄 수 있기 때문에 꼭 필요할 때 사용하자.  

1. nodemon, kill-port 설치
```shell
npm install nodemon kill-port --save-dev
```

(*옵션) `--watch`, `--exec` 커맨드를 통해 지켜보고 있을 파일을 선택할 수 있다.
```shell
nodemon --exec "your command here" --watch "filename 1" --watch "filename n" --watch "directory name"
```

(*옵션) 실행하려는 명령에 대해 kill-port를 사용해서 기존 프로세스의 port 점유를 막는다.
```shell
kill-port --port 8000
```

2. 해당 옵션들을 적절히 활용하여 커맨드를 만들고 `package.json/script`에 추가합니다.
```shell
"scripts": {
    "dev-auto-restart": "nodemon --exec 'kill-port --port 8000 && gatsby develop' --watch gatsby-node.js --watch package.json --watch gatsby-config.js"
  }
```

3. 추가한 script/명령어로 실행합니다.

```shell
npm run dev-auto-restart
```