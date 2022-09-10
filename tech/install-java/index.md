---
layout: post
author: "googie"
title:  "MacOS Java JDK 설치하기"
subtitle: "자바 입문하기"
type: "java"
tech: true
text: true
post-header: true
header-img: "img/header.jpg"
order: 2
comments: true
draft: false
---

Java를 사용하여 개발을 하기 위해선 [JDK(라이브러리)](https://www.oracle.com/java/technologies/downloads/#java8-mac)와 [JRE(환경)](https://www.oracle.com/java/technologies/downloads/#jre8-macos)를 설치해야 한다.


![설치](img/install_java.png)
![설치](img/install_jre.png)

- **x64 DMG(jdk-8u341-macosx-x64.dmg, jre-8u341-macosx-x64.dmg)** 를 다운로드 합니다.

![설치](img/install_java2.png)

- 로그인이 필요하다면, 만들어주고 설치하면 됩니다.

![설치](img/install_java3.png)

- 다운로드가 완료되면, 폴더를 열어 설치를 이어나갑니다.

- 설치를 확인하려면, 아래 명령어를 입력하면 됩니다.
```
java -version
javac -version
```

- 설치된 위치를 확인하려면, 아래 명령어를 입력하면 됩니다.
```
which java
which javac
```

