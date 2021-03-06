---
title:  "[Java] Java SE, Java EE(== Jakarta EE), JRE, JDK Concept"
excerpt: ""

categories:
  - programming
tags:
  - [java]

toc: true
toc_sticky: true
 
date: 2021-03-06
last_modified_at: 2021-03-06
---

## Java SE

Java Standard Edition의 약자로, Java 프로그램 개발을 위한 용도로 사용된다. 입출력을 위한 API, UI를 위한 API(Swing, AWT-Abstract Window Toolkit), JavaBeans 및 Java 프로그램을 위한 기본 API(math, zip, logging, regex 등)들을 포함한다.

> 2021년도 3월 현재, 2020년 9월 릴리즈된 Java SE 15 버전이 최신

## JRE

Java Runtime Environment의 약자로, Java 프로그램을 실행하기 위한 환경이다. Java Class Libraries, Java Class Loader, JVM을 포함한다.

## JDK

Java Development Kit의 약자로, JRE에 추가로 컴파일러, 디버깅 툴, 개발 툴 등의 Java 프로그램 개발을 위한 툴들을 포함한다.

### 예시

- jar: 클래스 라이브러리들과 리소스들을 하나의 JAR 파일로 생성, 관리
- javac: 자바 컴파일러, 자바 소스 파일을 바이트코드로 변환
- javadoc: 소스 코드 주석으로부터 자동으로 문서를 생성

## Java EE == Jakarta EE

Java Enterprise Edition의 약자로, Java를 사용한 서버 개발을 위한 플랫폼이다. Java SE의 기능들을 포함하며, 엔터프라이즈 환경을 위한 도구로 EJB, JSP, Servlet, JNDI, JDBC 같은 기능들이 포함된다.

과거 J2EE가 Java EE로 이름이 변경 되었으며, Java EE가 이클립스 재단에서 관리 됨에 따라서 'Jakarta EE'로 이름이 변경되었다.

### EJB

Enterprise JavaBeans의 약자로 엔터프라이즈 시스템을 구현하기 위한 서버측 컴포넌트 모델이다. EJB는 Java EE의 사양(API) 중 하나로, Java EE의 기능 중 하나인 JSP가 주로 화면 로직을 담당하고, EJB가 비즈니스 로직을 처리한다.

## Source

- https://ko.wikipedia.org/wiki/자바_플랫폼,_엔터프라이즈_에디션
- https://ko.wikipedia.org/wiki/자바_플랫폼,_스탠더드_에디션
- https://ko.wikipedia.org/wiki/엔터프라이즈_자바빈즈
- https://namu.wiki/w/Java
- https://www.itworld.co.kr/t/62076/가상화/110768
- https://ko.wikipedia.org/wiki/자바_개발_키트