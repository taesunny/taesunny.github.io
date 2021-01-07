---
title:  "[Java] Annotations"
excerpt: "About Java Annotations"

categories:
  - programming
tags:
  - [java, study, programming]

toc: true
toc_sticky: true
 
date: 2020-12-15
last_modified_at: 2020-12-15
---

# 정의
- 사전적 의미: 주석
- metadata의 형태
- program에 대한 데이터를 제공

# 용도
- Information for the compiler: compiler가 error나 warning을 발견할 수 있도록 사용됨
- Compile-time and deployment-time processing: software tool들이 annotation을 이용해 code, XML files 등을 생성 가능
- Runtime processing: Runtime에서도 사용될 수 있음

# 형태
- `@`{Annotation name}을 변수, 함수에 붙여 사용
    - class instance 생성문 (new class()), type cast, implements절, exception throws 선언문에 사용 가능
    ```
    @Override
    void myMethod() {...}
    ```

- annotation에 elements를 추가 가능
    - 이름을 명시 안할 수도 있음
  
    ```
    @Author(
      name = "Benjamin Franklin",
      date = "3/27/2003"
    )
    class MyClass() { ... }

    @SuppressWarnings("unchecked")
    void myMethod() { ... }
    ```
- 여러 annotaion 설정 가능
- default값 설정 가능

# 선언 방법
- 선언 예시
    ```
    @interface ClassPreamble {
      String author();
      String date();
      int currentRevision() default 1;
      String lastModified() default "N/A";
      String lastModifiedBy() default "N/A";
      // Note use of array
      String[] reviewers();
    }
    ```

- 사용 예시
    ```
    @ClassPreamble (
      author = "John Doe",
      date = "3/17/2002",
      currentRevision = 6,
      lastModified = "4/12/2004",
      lastModifiedBy = "Jane Doe",
      // Note array notation
      reviewers = {"Alice", "Bob", "Cindy"}
    )
    public class Generation3List extends Generation2List {

    // class code goes here

    }
    ```

# Predefined Annotation Types
- @Deprecated
- @Override
- @SuppressWarnings
- @NonNull String str;
- ...

# Meta Annotation
## @Target
- 적용 위치 설정 (default: 전체)
- TYPE, ANNOTATION_TYPE, METHOD, PACKAGE...


## @Retention
- SOURCE, CLASS, RUNTIME
- 영향을 미치는 시점을 설정


# 출처
- https://docs.oracle.com/javase/tutorial/java/annotations/index.html
- https://sjh836.tistory.com/8