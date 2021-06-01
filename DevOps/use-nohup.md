## nohup 명령어 사용하기

### 우선 ``nohup``이란?
>  nohup이란 리눅스, 유닉스에서 쉘스크립트파일 (*.sh)을 데몬형태로 실행시키는,  
>  프로그램 터미널 세션이 끊겨도 실행을 멈추지 않고 동작하도록 하게 하는 명령어.


### 사용하기 

1. ``ubuntu``에 nohup 명령어 설치.

```
$ sudo apt-get install nohup 
// 여기서 nohub 이 아니라 nohup 이라는 점 헷갈리지 않기.
```

2. 내가 `nohup` 으로 사용하고자 하는 `.sh` 파일의 권한을 755 로 준다.

```
$ chmod 755 ./{/sh file name}
```

3. `nohup` 으로 프로젝트 파일 run

```
$ sudo nohup ./{/sh file name}
```

4. 여기서 생소한 문구를 발견한다면..

```
nohup: ignoring input and appending output to 'nohup.out'

// 당황하지 말고 
$ ls
nohup.out

$ cat nohup.out
// nohup으로 실행한 화면의 출력이 여기에 기록된다.
```