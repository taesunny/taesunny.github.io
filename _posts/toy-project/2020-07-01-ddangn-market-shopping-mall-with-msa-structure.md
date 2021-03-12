---
title:  "[땅근마켓 쇼핑몰][당근마켓 클론코딩/토이프로젝트] - 쇼핑몰 MSA 구조로 개발해보기 (Clone Coding/Toy Project)"
excerpt: ""

categories:
  - toy-project
tags:
  - [toy-project, clone-coding, toy-project, msa]

toc: true
toc_sticky: true
 
date: 2020-07-01
last_modified_at: 2020-07-01
---

# Intro

평소 클라우드(Kubernetes, Container, IaaS, PaaS, SaaS)에 대해서만 연구 하며, 실제 클라우드 위에서 MSA를 어떻게 구축하고, 서비스를 제공하는지에 대해 관심 있던 와중에 Spring도 공부하고, MSA 구조로 서비스도 구축해 보고 싶어서 시작하게 되었다.

---

# Project

## 제공 기능

1. Web UI를 이용해 상품 조회/등록, 채팅기능
2. Android Application을 통해 상품을 조회하고, 푸시알림을 수신하는 기능
3. Web UI를 이용해 공지사항/광고 푸시알림을 등록 하고, Android Application으로 푸시가 전송되는 기능
4. 구축한 Auth 서버를 이용한 회원 가입/로그인 기능

## 전체 구조 (Architecture)

{:refdef: style="text-align: center;"}
![Project Structure](/assets/img/toy-project/2021-01-27-23-05-39.png)
{: refdef}


## Modules


### Product API Server

Spring Boot & JAVA를 이용하여 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market)

{:refdef: style="text-align: center;"}
![Project API](/assets/img/toy-project/2021-01-27-23-06-53.png)
{: refdef}


### Notification Server

Spring Boot & Kotlin, Firebase FCM을 이용하여 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-app-server)


### Chatting Server

상품별 채팅 기능을 제공, Spring Boot & Kotlin, Embeded Redis, 웹소켓을 이용하여 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-chatting-server)

{:refdef: style="text-align: center;"}
![chatting feature](/assets/img/toy-project/2021-01-27-23-10-22.png)
{: refdef}


### Web UI

JavaScript, React를 이용해서 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-frontend)

- 상품 리스트, 디테일 정보 확인, 상품 등록, 댓글 확인, 등록 등의 상품 관련 기능들이 가능, 회원 가입가능
- 로그인한 사용자의 Role을 이용하여, 가입 후 로그인 한 사용자만 상품 등록이 가능.
- 가입 후 ‘Admin’ Role 획득 시 푸시알림 등록이 가능

{:refdef: style="text-align: center;"}
![ui home](/assets/img/toy-project/2021-01-27-23-12-36.png)
{: refdef}

{:refdef: style="text-align: center;"}
![ui login](/assets/img/toy-project/2021-01-27-23-13-12.png)
{: refdef}

{:refdef: style="text-align: center;"}
![ui product list](/assets/img/toy-project/2021-01-27-23-13-54.png)
{: refdef}


### Android Application

Flutter를 이용하여 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-app)

App을 설치한 사용자들은, 관리자가 Web UI에서 보내는 Notification들을 수신할 수 있다.

[[참고 Post - Flutter Android App의 Firebase 연동 & FCM Cloud Messaging Test](https://taesunny.github.io/programming/how-to-use-firebase-and-fcm-cloud-messaging-in-flutter-android-app/)]

{:refdef: style="text-align: center;"}
![mobile app](/assets/img/toy-project/2021-01-27-23-15-19.png)
{: refdef}


### API Gateway

Spring Boot, Netflix Zuul, Kotlin를 이용해 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-api-gateway)


### Service Registry (for Service Discovery Pattern)

Netflix Eureka를 이용하여 구현 완료

> [github repo](https://github.com/taesunny/ddangn-market-service-registry)


{:refdef: style="text-align: center;"}
![mobile app](/assets/img/toy-project/2021-01-27-23-17-07.png)
{: refdef}


### Auth Server

Keycloak 오픈소스를 이용하여 구현 완료

> [Install Keycloak using docker & Basic Settings 글](https://medium.com/@taesun/%EB%8B%B9%EA%B7%BC%EB%A7%88%EC%BC%93-%ED%81%B4%EB%A1%A0%EC%BD%94%EB%94%A9-%ED%86%A0%EC%9D%B4%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EB%95%85%EA%B7%BC%EB%A7%88%EC%BC%93-msa-%EA%B5%AC%EC%B6%95%ED%95%B4%EB%B3%B4%EA%B8%B0-dangn-market-clone-coding-toy-project-ddangn-market-80a2a7660fd1)


### DB

Amazon RDS 이용


### 상품 사진 Storage

Amazon S3를 이용


## 사용 기술 요약

- 언어: Java, Kotlin, Javascript, CSS, HTML
- 프레임워크: Spring Boot, Flutter, React
- 이용 서비스: Amazon RDS, Amazon S3, Amazon Lightsail, Amazon Route 53
- 기타: Gradle, NPM, Docker, Redis


## TODO List

[ ] 다중 API 서버 동작 시 Product, Comment 자원 관리 고려

[ ] Circuit breaker

[ ] configuration server

