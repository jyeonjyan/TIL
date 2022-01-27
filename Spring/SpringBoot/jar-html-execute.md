# Gradle jar에 html 파일들을 생성하는 방법
> Spring REST Docs을 사용하면 운영에서 AsciiDoc를 html로 실행해야 편하다.

[이호진님 Spring REST Docs 적용](https://techblog.woowahan.com/2597/)이라는 글에는 html executor 설정이 아래와 같이 명시 돼 있다.

```Groovy
bootJar {
    dependsOn asciidoctor // (3)
    from ("$/html5") { // (4)
        into 'static/docs'
    }
}
```

하지만 이렇게 하면 `gradle-doesnt-copy-html-files-into-executed-jar` 라는 에러를 반환한다.

#### best practice
```Groovy
bootJar {
    dependsOn asciidoctor 
    from ("${asciidoctor.outputDir}/html5") { 
        into 'static/docs'
    }
}
```

문제가 해결되는 것을 확인할 수 있다.