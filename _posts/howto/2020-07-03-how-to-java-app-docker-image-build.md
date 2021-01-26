---
title:  "How to build Docker Image of Java Application using Gradle"
excerpt: ""

categories:
  - howto
tags:
  - [howto, java, docker, image, build, gradle]

toc: true
toc_sticky: true
 
date: 2020-07-03
last_modified_at: 2020-07-03
---

# Intro

Java 어플리케이션을 도커 이미지화 할 필요성이 생겨 찾아보던 중, `gradle-docker-plugin` 라이브러리를 사용해 Docker 이미지를 빌드 할 수 있는 방법을 찾았다. 이를 테스트 해 본 내용을 단계별로 기록해 보려 한다. (kotlin을 사용한다.)

---

# How to

## Docker 설치

테스트할 머신에 docker를 설치한다.

```
sudo yum install docker
```

## Add Plugin

### Plugin 추가 — Kotlin

타겟이 되는 Application의 build.gradle.kts 파일에 “application”, “docker-java-application” 플러그인을 추가한다.

> gradle version 6.4.1 기준

```
// build.gradle.kts 파일 예시
plugins {
   ...
   java
   id("com.bmuschko.docker-java-application") version "6.4.0"
}
```

### Plugin 추가 — Groovy

groovy를 사용하는 경우 kotlin과 같이 타겟이 되는 Application의 build.gradle 파일에 “application”, “docker-java-application” 플러그인을 추가한다.

> gradle version 6.3 기준

```
// build.gradle.kts 파일 예시
plugins {
   ...
   id 'java'
   id 'com.bmuschko.docker-java-application' version '6.4.0'
}
```

## Add DockerExtension Config

### DockerExtension Config — Kotlin

docker extension의 config를 build.gradle.kts 파일에 추가한다. java 버전에 맞는 이미지를 선택하고, maintainer 정보와 오픈할 port를 추가한다.

```
docker {
   javaApplication {
      baseImage.set("openjdk:8-alpine")
      maintainer.set("Name Lee 'xxx@gmail.com'")
      ports.set(listOf(8761))
   }
}
```

### DockerExtension Config — Groovy

groovy의 경우 문법에 맞게 docker extension의 config를 build.gradle 파일에 추가한다. java 버전에 맞는 이미지를 선택하고, maintainer 정보와 오픈할 port를 추가한다.

```
docker {
    javaApplication {
        baseImage = 'openjdk:8-alpine'
        maintainer = 'Name Lee "xxx@gmail.com"'
        ports = [8761]
    }
}
```

## Build Docker Image

gralde (wrapper)를 이용해 docker image를 빌드한다. 아래의 명령을 사용한다.

```
./gradlew clean jar dockerBuildImage
```

## Check Created Docker Image

docker images 명령을 통해 생성 된 docker image를 확인 할 수 있다.

```
docker images
```

## 예시 Git Project build.gradle 파일

- https://github.com/taesunny/ddangn-market-service-registry/blob/90616c3403e429e81a1b83314b6f56be5341a78b/build.gradle.kts#L9

---

# Source

- https://docs.oracle.com/en/solutions/monitor-applications-on-kubernetes/create-docker-image-java-application.html
- https://plugins.gradle.org/plugin/com.bmuschko.docker-java-application
- https://bmuschko.github.io/gradle-docker-plugin/