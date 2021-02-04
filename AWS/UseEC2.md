# 간단하게 EC2 서버에 프로젝트 배포하기
## Amazon Web Service EC2

### 인스턴스 생성
```
https://aws.amazon.com/ko/education/awseducate/
```
**EC2 서버 만들기**
```
Amazon Linux 2 AMI (HVM), SSD Volume Type 사용하기
```
### 인스턴스 연결
<img src="./meterial/createInstance.png">

프라이빗 키 파일을 찾습니다. 이 인스턴스를 시작하는 데 사용되는 키는 ***.pem입니다.  
필요한 경우 이 명령을 실행하여 키를 공개적으로 볼 수 없도록 합니다.
```bash
$ chmod 400 ***.pem
```
ssh connect
```bash
$ ssh -i "***.pem" ec2-user@ec2-34-202-149-32.compute-1.amazonaws.com
```

### AWS EC2 서버 환경 설정
**git install**
```bash
$ sudo yum install git
```

**java8 install**
```bash
$ sudo yum install -y java-1.8.0-openjdk-devel.x86_64
```