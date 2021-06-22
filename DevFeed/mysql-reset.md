## 🍺 homebrew 로 설치된 mysql 삭제 후 재설치

1. mysql 서비스 중지

```sh
$ brew services stop mysql
```

2. mysql 삭제

```sh
$ brew uninstall mysql
```

3. mysql 관련 파일을 모두 삭제한다.

```sh
$ rm -rf /usr/local/var/mysql
```

4. mysql 설정 파일 삭제한다.

```sh
$ rm /usr/local/etc/my.cnf
```

5. mysql 재설치

```
$ brew update 
$ brew install mysql
```