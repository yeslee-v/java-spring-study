---
sticker: lucide//equal-not
tags:
  - jdk
---
# 문제
- gradle로 프로젝트 셋팅하고 ./gradle build 하니까
```
BUG! exception in phase 'semantic analysis' in source unit '_BuildScript_' Unsupported class file major version 68

> Unsupported class file major version 68
```
- java는 openjdk 24 버전 쓰고 있고 gradle은 8.13 버전 사용하고 있어

# 원인
- `Unsupported class file major version 68`: **Gradle이 JDK 24을 제대로 지원하지 않아서 생기는 문제**. class file major version 68은 JDK 24 의미.
- Gradle 내부의 스크립트 해석기(Groovy 기반)나 의존성 중 일부가 아직 Java 24에서 컴파일된 클래스 파일을 제대로 처리하지 못하는 거야. Gradle 8.13도 Java 24을 **공식 지원하지 않아.**

# 해결
- JDK 버전 낮추기: Java 17 또는 Java 21 (LTS 버전)으로 변경

# 과정
- intellij에서 Project structure에 들어가서 아래 방법으로 수동으로 바꿨는데 java --version하면 여전히 24로 나오네
```
sdk: ms-17 Microsoft openJDK 17.0.15 - aarch64

language level: 17-sealed types, always-strict floating-point sementics
```

→ IntelliJ와 터미널은 서로 다른 실행 환경을 사용하고 있음: 터미널에서 jdk 변경해주어야함

1. temurin 설치 후, 17버전 다운로드
	```
	brew install temurin
	brew install --cask temurin
    ```
2. ~/.zshrc에서 다음 환경 변수 추가 후 적용(source .zshrc)
	```
	export JAVA_HOME=/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home
	
	export PATH=$JAVA_HOME/bin:$PATH
	```
3. `java –version`으로 제대로 변경됐는지 확인, `./gradle build` 재실행

# 관련 링크
- https://docs.gradle.org/current/userguide/compatibility.html#java_runtime