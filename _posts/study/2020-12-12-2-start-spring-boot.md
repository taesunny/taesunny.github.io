---
title:  "[스프링 부트 개념과 활용 스터디] 2부 스프링 부트 시작하기"
excerpt: "2부 스프링 부트 시작하기 스터디"

categories:
  - study
tags:
  - [study, spring, springboot]

toc: true
toc_sticky: true

comments: true
 
date: 2020-12-09
last_modified_at: 2020-12-09
---
# 특징
- 제품 수준의 spring application을 쉽게 만들어 준다.
- spring과 third-party libraries(ex) tomcat)의 설정을 제공
- XML 설정, code generation을 할 필요가 없다.
- Java8 이상을 지원
- 3.1 버전 이상의 Servlet을 지원
- https://spring.io/projects/spring-boot

# Project 생성 방법
## Spring Initializr 사용
- https://start.spring.io/에 접속 후 원하는 설정, dependencies 추가 후 프로젝트 zip 파일 다운로드
    ![spring initializr](/assets/img/study/2020-12-20-20-04-18.png)

- Zip 파일 압축 해제 후 IDE에서 project open
    - build.gradle 예시
    ![build.gradle example](/assets/img/study/2020-12-20-20-05-25.png)

    - main class 에시
    ![main class example](/assets/img/study/2020-12-20-20-06-10.png)


## gradle, maven 설정을 통한 설정
- maven 설정
    - https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#getting-started-maven-installation
- gradle 설정
    - https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#getting-started-gradle-installation

# Spring Boot 프로젝트 구조
![spring boot project structure](/assets/img/study/2020-12-20-20-06-48.png)

- /src/main/java에 java 파일들 존재
- /src/main/resources에 기타 파일들 존재
    - /src/main/java의 java파일들에서, 해당 디렉터리를 루트로 파일들에 참조 가능
- /src/test/java에 test를 위한 java 파일들 존재
- /src/test/java/resources에 test를 위한 리소스들을 저장

## Spring Boot 추천 main application 위치
- 최상위 패키기(default package) 위치에 main application class를 생성하는 것을 추천
    - @SpringBootApplication annotation이 붙은 main method가 존재하는 class
    - /src/java에 바로 두지 않는다.
- 이유
    - 해당 클래스 이하를 component scan을 하기 때문
    - /src/java에 두면, 모든 패키지들을 scan 하겠다는 뜻이기 때문
- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using-boot-structuring-your-code

# 출처
- '인프런, 백기선님의 스프링 부트 개념과 활용' 강의를 들으며 스터디한 내용을 정리한 글입니다.