# Gradle 6 to 7 what's different?

자세한 내용은 [여기서](https://docs.gradle.org/current/userguide/upgrading_version_6.html) 확인할 수 있다.

## 개요

How to solve..
```
Could not find method testCompile() for arguments
```

## IntelliJ에서 gradle version을 확인하는 방법은 굉장히 간단하다.

```
gradle/wrapper/gradle-wrapper.properties
```

해당 디렉토리의 파일에 보면 `distributionUrl=https\://services.gradle.org/distributions/gradle-6.9-bin.zip` 이렇게 gradle 버전을 확인할 수 있다.
