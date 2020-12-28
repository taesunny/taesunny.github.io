---
title:  "[Java] Reflection 개념"
excerpt: "About Java Annotations"

categories:
  - analysis
tags:
  - [java, study, reflection]

toc: true
toc_sticky: true
 
date: 2020-12-28
last_modified_at: 2020-12-28
---

# Reflection

Java 코드 상에서 로드된 class, field, method, constructor의 정보를 Class Type 정보 없이 동적으로 확인하고, 수정하는 기능이다.

> 사전적으로 Reflection는 거울에 비친 상, 반사, 반영을 의미

## Reflection Features

아래의 테스트 프로그램처럼 class 객체를 받아와 메소드, 필드 등을 조회, 수정할 수 있다.

### Get Features

- Class.forName("class name") : 클래스 객체를 Get
- {class}.getConstructor() : 생성자 Get
- {class}.getDeclaredMethod() : public/private Method Get
- {class}.getMethod() : public method Get
- {class}.getDeclaredField() : Field Get

### Usage

- {constructor}.newInstance() : Object를 Get
- {method}.invoke() : method를 실행
- {field}.set() : field의 값을 설정
- private 메소드, 필드의 경우 setAccessible(true)를 설정하여 접근이 가능

### Code Example

```java
import java.lang.reflect.*;

class Human {
    private String name;
    private int age;

    public Human(String name, int age) {
        this.name = name;
        this.age = age;
    }

    private String getName() {
        return name;
    }

    private void setName(String name) {
        this.name = name;
    }

    private int getAge() {
        return age;
    }

    private void setAge(int age) {
        this.age = age;
    }
}

public class Test {
    public static void main(String args[]) {
        try {
            Class humanClass = Class.forName("Human");
            Constructor humanConstuctor = humanClass.getConstructor(new Class[] { String.class, int.class });
            Object obj = humanConstuctor.newInstance("No name", 0);

            Field nameField = humanClass.getDeclaredField("name");
            nameField.setAccessible(true);
            Field ageField = humanClass.getDeclaredField("age");
            ageField.setAccessible(true);

            Method getAgeMethod = humanClass.getDeclaredMethod("getAge");
            getAgeMethod.setAccessible(true);
            Method setAgeMethod = humanClass.getDeclaredMethod("setAge", new Class[] { int.class });
            setAgeMethod.setAccessible(true);

            System.out.println("Name is " + (String) nameField.get(obj));
            System.out.println("Age is " + (int) getAgeMethod.invoke(obj));

            // set new name, age
            nameField.set(obj, "sunny");
            setAgeMethod.invoke(obj, 30);

            System.out.println("Happy new year!!!");
            System.out.println("Name is " + (String) nameField.get(obj));
            System.out.println("Age is " + (int) getAgeMethod.invoke(obj));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

```bash
// compile
javac Test.java

// run
java Test
```

# Source
- [https://docs.oracle.com/javase/8/docs/technotes/guides/reflection/index.html](https://docs.oracle.com/javase/8/docs/technotes/guides/reflection/index.html)