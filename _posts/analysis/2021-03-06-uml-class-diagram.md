---
title:  "UML - Class Diagram"
excerpt: ""

categories:
  - analysis
tags:
  - [uml, class-diagram]

toc: true
toc_sticky: true
 
date: 2021-03-06
last_modified_at: 2021-03-06
---

# UML

UML은 Unified Modeling Language의 약자로 범용 모델링 언어이다. 시스템의 디자인을 시각화하여 보여 주는 것을 목적으로 한다.

## Types

표현하려는 내용과 목적에 따라 크게 두가지 타입과 각 타입의 종류들로 나눌 수 있다.

### Structural Diagrams

시스템 구조의 정적인 측면을 표현하는데 사용된다.

- Class Diagram
- Composite Structure Diagram
- Component Diagram
- Deployment Diagram
- Object Diagram
- Package Diagram
- Profile Diagram

### Behavior Diagrams

시스템의 동적인 측면을 표현하는데 사용된다.

- Activity Diagram
- Interaction Diagram
    - Communication Diagram
    - Interaction Overview Diagram
    - Sequence Diagram
    - Timing Diagram
- State Machine Diagram
- Use Case Diagram

---

# Class Diagram

Class Diagram은 UML의 한 종류로 객체 지향 소프트웨어 시스템을 작성하는 것을 목적으로 한다. 시스템의 클래스들과 오브젝트들의 구조를 시각화 하며, 클래스의 메소드들과 속성들을 포함한다.

## Syntax

### Class Representation

{:refdef: style="text-align: center;"}
![class representation example](/assets/img/analysis/2021-03-06-14-43-59.png)
{: refdef}

Class를 Row가 세개인 사각형으로 표현한다.

- 첫번째 Row: Class의 이름을 적는다.
- 두번째 Row: Class의 Attribute들을 적어 준다. '{공개 범위} {이름}: {Type}'의 형식을 갖는다.
- 세번째 Row: Class의 Method들을 적어 준다. '{공개 범위} {이름}: ({Arguments}): {return type}'의 형식을 갖는다.

#### Visibility

Attribute과 Method의 공개 범위는 다음의 방식으로 표현된다.

- '-' : private
- '+' : public
- '#' : protected
- '~' : package

### Relationships Representation

{:refdef: style="text-align: center;"}
![Relationships Representation](/assets/img/analysis/2021-03-06-14-44-41.png)
{: refdef}

클래스들을 관계에 따른 각 모양으로 클래스들의 관계를 표현한다.

- Inheritance: super class와 sub class와의 상속 관계를 나타낸다.
- Realization/Implementation: 인터페이스를 구현하는 경우
- Dependency: 하나의 클래스가 다른 클래스의 의존성을 가지고 있는 경우. 지역변수로 사용되거나, 메소드의 인자로 사용되는 경우. (x 멤버 변수, 일시적 사용)
- Association: 클래스의 연관 관계를 나타낸다. 멤버 변수로 사용되는 경우
    - Aggregation: 하나의 클래스가 다른 클래스(전체)의 부분이 되는 경우. 전체 클래스가 부분 클래스를 생성하지는 않는다.
    - Composition: 하나의 클래스가 다른 클래스(전체)의 부분이 되는 경우. 전체 클래스가 부분 클래스를 직접 생성(new)한다.

#### Multiplicity

각 선에 추가로 숫자를 기입하여 '일대다', '다대다'와 같은 연관 관계 Multiplicity를 표현 할 수 있다.

- '1' : 1
- '\*' or '0..*' : 0 또는 그 이상
- '1..*' : 1 또는 그 이상
- '0.. 1' : 0 또는 1
- '{숫자1}.{숫자2}.{숫자3}' : 숫자1 또는 숫자2 또는 숫자3
- 비어 있는 경우 : 일대일 관계

## Example

{:refdef: style="text-align: center;"}
![UML Diagram Example](/assets/img/analysis/2021-03-06-14-46-12.png)
{: refdef}

---

# Source

- [https://ko.wikipedia.org/wiki/통합_모델링_언어](https://ko.wikipedia.org/wiki/%ED%86%B5%ED%95%A9_%EB%AA%A8%EB%8D%B8%EB%A7%81_%EC%96%B8%EC%96%B4)
- [https://www.geeksforgeeks.org/unified-modeling-language-uml-introduction/](https://www.geeksforgeeks.org/unified-modeling-language-uml-introduction/)
- [https://www.geeksforgeeks.org/unified-modeling-language-uml-class-diagrams/?ref=rp](https://www.geeksforgeeks.org/unified-modeling-language-uml-class-diagrams/?ref=rp)
- [https://medium.com/@smagid_allThings/uml-class-diagrams-tutorial-step-by-step-520fd83b300b](https://medium.com/@smagid_allThings/uml-class-diagrams-tutorial-step-by-step-520fd83b300b)
- [https://infinitejava.tistory.com/61](https://infinitejava.tistory.com/61)