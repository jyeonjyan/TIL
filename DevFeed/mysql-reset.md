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

6. mysql 시작하기
```
$ mysql.server start
```

7. MySQL 설정 방법
```
$ mysql_secure_installation
```

... 세부세팅
```
"Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No"
> no
> 1234

"Remove anonymous users? (Press y|Y for Yes. any other key for No)"
> yes

"Disallow root login remotely? (Press y|Y for Yes, any other key for No)"
> yes

"Remove test database and access to it? (Press y|Y for Yes, any other key for No)"\
> yes

"Reload privilege tables now? (Press y|Y for Yes, any other key for No)"
> yes
```

8. mysql user 접속
```
$ "mysql -uroot -p"
```