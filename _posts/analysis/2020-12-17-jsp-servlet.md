---
title:  "JSP & Servlet"
excerpt: "About JSP and Servlet"

categories:
  - analysis
tags:
  - [jsp, servlet, analysis]

toc: true
toc_sticky: true
 
date: 2020-12-17
last_modified_at: 2020-12-17
---
# JSP & Servlet

## Servlet

- 클라이언트의 요청을 동적으로 처리하고, 결과를 반환하는 어플리케이션(서버측 프로그램)
- html으로 요청에 응답
- 자바 소스코드 속에 HTML 코드가 존재, 하나의 클래스
- 자바를 사용해 웹을 개발하기 위한 표준
- Java Thread로 동작
- MVC 패턴에서 Controller로 이용
- web.xml 파일에 URL과 서블릿 클래스를 매핑해서 사용
- 메모리 로딩 후, 해당 객체를 계속 재사용 → 응답속도가 빠르다.
- javax.servlet.http.HttpServlet 클래스를 상속
- Servlet 수정 시, Java 코드를 컴파일한 후 동적인 페이지 처리
  - 다시 컴파일, 재배포 필요→JSP로 보완
- Java의 CGI(Common Gateway Interface) 프로그램(WebServer와 WAS가 소통하기 위해 CGI 규칙을 준수)
- WAS에 의해 Servlet 실행, 다시 웹서버에 전달
- JSP 없이 서블릿만 이용하면, html들을 out.println으로 일일이 출력해야함

### 동작

- HTTP Request 발생
- Servlet Container로 요청 도착
- HttpServletRequest, HttpServletResponse 객체 생성
- web.xml을 통해 Servlet 검색
- 해당 Servlet에서 service() 메소드 호출
  - doGet(), doPost(), doOptions(), doHead(), doTrace()... 호출
    - 동적 페이지 생성 후 hTtpServletResponse 객체에 응답
- 응답
- HttpServletRequest, HttpServletResponse 두 객체 소멸

---

## Servlet Container

- 서블릿을 관리해주는 컨테이너
  - 생명 주기 관리
- 클라이언트의 요청을 받고, 응답 지원
- 웹서버와 소켓으로 통신
  - 서블릿과 웹서버가 통신할 수 있도록
- 멀티 쓰레딩 관리
- JSP 지원
- ex) 톰켓
  - 웹서버와 통신
  - was
  - JSP, Servlet이 작동하는 환경 제공
  - 아파치 - 웹서버

### 역할

- 웹서버와 통신 지원
  - 서블릿과 웹서버가 쉽게 통신할 수 있도록 지원
  - 소켓 관리 등
- 서블릿 생명주기(Life Cycle) 관리
  - 서블릿 클래스 로딩, 인스턴스화, 초기화
  - 요청 시 적절한 서블릿 메소드 호출
  - 서블릿 GC
- 멀티쓰레드 지원, 관리
  - 요청이 올 때마다 새로운 자바 쓰레드 생성
  - HTTP 서비스 메소드 실행 후 쓰레드 종료
- 보안관리
  - XML에 보안 설정, 수정이 필요해서 재 컴파일이 필요 없음

### Servlet 생명 주기

1. 클라이언트 요청
2. HttpServletRequest, HttpServletResponse 객체 생성, 서블릿 검색
3. 메모리에 서블릿 있는지 확인
4. 없을 경우 init() 메소드를 호출하여 적재
   - init() 메소드는 처음 한번만 실행
   - 서블릿의 쓰레드에서 공통적으로 사용해야하는 것을 오버라이딩하여 구현
   - 실행 중에 서블릿이 변경되면 → 기존 서블릿 파괴, init()으로 다시 메모리에 적재

 5. init() 호출 되고, 클라이언트 요청에 따라 service() 메소드 호출

6. doGet(), doPost() 등으로 분기

7. 서버 종료 또는 서블릿 필요 없을 때, 수정되었을 때 → 컨테이너가 서블릿에 종료 요청 → destroy() 메소드 호출, 한번만 실행, 메모리에서 제거

- 종료시 처리해야하는 작업들을 destroy() 메소드를 오버라이딩해서 구현

### web.xml

- web application 설정 파일
- 배포 시, application 환경 설정
- 배포할 Servlet정보, Servlet에 매핑되는 URL 정보를 tomcat에 전달
- 보통 annotation을 사용
  - @WebServlet("/hello")
- 내용
  - servlet, JSP 정의, 매핑 URL
  - error page 설정
  - 보안
  - session 유효시간
  - servletcontext 초기 파라미터
  - 등
- web.xml을 설정 할 경우

```yaml
<servlet>
  	<servlet-name>MappingTest</servlet-name>
  	<servlet-class>hello</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>MappingTest</servlet-name>
  	<url-pattern>/hello</url-pattern>
  </servlet-mapping>
```

---

## JSP

- Java Server Page
- Java 코드가 들어 있는 HTML 코드, Server side 스크립트 언어
- Servlet을 보완하고 기술을 확장한 스크립트 방식 표준
- <% 소스코드 %> 또는 `<%= 소스코드 =%>`형태
- 자바코드 → 웹 서버에서 실행, 웹 서버에서 JSP 파일을 서블릿 클래스로 변환
- 컴파일이 필요 없다.
  - 서버의 디렉토리에 추가만 하면 됨
- 서블릿 규칙이 복잡 → JSP가 나옴
  - JSP는 WAS에 의해 서블릿 클래스로 변환되서 사용됨

### 동작 구조

1. 웹 서버가 서블릿에 대한 요청을 받음
2. 서블릿 컨테이너에 요청을 넘김
3. JSP를 이용해 비지니스 로직과 프레젠테이션 로직을 분리
4. ????

---

## Servlet, JSP 모델

### JSP만 이용하는 모델

- JSP가 요청을 받아 Java Bean(DTO, DAO)를 호출해서 동적인 페이지 생성
- 동작 과정
  - JSP로 작성된 프로그램 → 내부적으로 WAS에서 Servlet으로 변환
  - JSP 태그를 분해하고 추출 → 다시 순수한 HTML 웹 페이지로 변환
  - 클라이언트로 응답
- 특징
  - 개발 속도가 빠름
  - View(프레젠테이션 로직)와 Controller(비즈니스 로직)이 혼재
  - JSP 코드가 복잡, 유지 보수가 힘듬

### JSP Servlet을 모두 이용 (MVC Architecture)

- View - JSP
- Controller - Servlet
- Model - Java Beans
  - DTO, DAO를 이용해 db 접근

---

## 출처

- [https://mangkyu.tistory.com/14](https://mangkyu.tistory.com/14)
- [https://ko.wikipedia.org/wiki/자바_서블릿](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%84%9C%EB%B8%94%EB%A6%BF)
- [https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html](https://gmlwjd9405.github.io/2018/11/04/servlet-vs-jsp.html)
- [https://victorydntmd.tistory.com/148](https://victorydntmd.tistory.com/148)
- [https://galid1.tistory.com/487](https://galid1.tistory.com/487)