# 엎질러진 github 명령 취소하기

## git add 취소하기
먼저 모든 파일을 스테이징한다.
```
$ git add .

// 파일들의 상태를 확인한다.
$ git status
...
modified: ***.md
```
git reset HEAD [file] 명령어를 통해 git add를 취소한다.
```
$ git reset HEAD ***.md
```

## git commit 취소하기
우선 commit 목록을 확인한다. 
```
$ git log
```
방법 1  
> commit을 취소하고 해당 파일들은 staged 상태로 워킹 디렉터리에 보존
```
$ git reset --soft HEAD^
```
방법 2  
> commit을 취소하고 해당 파일들은 unstaged 상태로 디렉터리에 보존
```
$ git reset --soft HEAD^
$ git reset HEAD^
// 마지막 커밋 2개를 취소 
$ git reset HEAD~2
```
방법 3  
> commit을 취소하고 해당 파일들은 unstaged 상태로 디렉터리에서 삭제
```
$ git reset --hard HEAD^
```

## git commit message 변경하기
git commit -amend를 통해 git commit message를 변경할 수 있다.
```
$ git commit --amend
```

## git reset 명령어의 옵션
1. -soft: index 보존, 디렉터리 보존, 모두 보존.
2. -mixed: index 취소, 디렉터리 보존, 기본 옵션.
3. -hard: index 취소, 디렉터리 삭제, 모두 삭제.

## git push 취소하기
1. 가장 최근의 commit을 취소한다.
```
$ git reset HEAD^
```
2. 원하는 시점으로 워킹 디렉터리를 되돌린다.
```
$ git reflog
$ git reset HEAD@{number} or $ git reser [commit id]
```
3. 이 상태에서 다시 commit
```
$ git commit -m " "
```
4. git 강제 push
```
$ git push origin [branch name] -f
```
