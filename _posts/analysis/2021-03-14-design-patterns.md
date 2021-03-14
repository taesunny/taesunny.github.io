---
title:  "Design Patterns with Java Examples"
excerpt: ""

categories:
  - analysis
tags:
  - [design-pattern, java]

toc: true
toc_sticky: true
 
date: 2021-03-14
last_modified_at: 2021-03-14
---

> 현재 각 패턴과 Example들을 업데이트 중입니다.

---

디자인 패턴은 프로그래머가 어플리케이션이나 시스템을 디자인할 때 공통된 문제들을 해결하는데에 쓰이는 형식화 된 패턴이다.

> [[Example Github Project Link](https://github.com/taesunny/design-pattern-study)]

---

# 분류

Purpose(목적)과 Scope(범위)에 따라 디자인 패턴들을 분류 할 수 있다.

{:refdef: style="text-align: center;"}
![Design Patterns Classification](/assets/img/analysis/2021-03-14-22-41-13.png)
{: refdef}

## 목적에 따른 분류

### Creational Class Pattern

- 객체를 생성하는 책임의 일부를 서브클래스/객체로 넘김

### Structural Pattern

- 상속을 통해서 클래스/객체를 조합

### Behavioral Class Pattern

- 상속을 이용해서 알고리즘과 제어 흐름을 기술 for class
- 하나의 작업을 수행하기 위해 객체 집합이 어떻게 협력 하는지를 기술 for object

---

# Design Patterns

## Factory Method Pattern

팩토리 매서드 패턴은 객체지향 디자인 패턴이다. 객체 생성 인터페이스를 미리 정의하고, 인스턴스를 만들 클래스의 결정을 서브클래스에서 내린다. 이를 통해 클래스간의 결합도를 낮출 수 있다.

- Example

아래의 예제와 같이 FileManager를 구현한 NFSFileManager, LocalFileManager의 객체를, 실제 FileManager를 사용하는 곳에서 'FileManagerFactory'라는 클래스를 통해 생성한 후 사용한다. FileManagerFactory 클래스에서는 getFileManager(String)의 인자에 따라 목적에 맞는 객체를 선택해 생성한다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/factorymethod)

{:refdef: style="text-align: center;"}
![Factory Method Pattern Example UML](/assets/img/analysis/2021-03-14-22-42-43.png)
{: refdef}

## Builder Pattern

복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에서 서로 다른 표현 결과를 만들 수 있게하는 패턴이다. 객체 생성 시 인자들이 많을 경우 구조적으로 간단하게 객체를 생성 할 수 있다.

- Example

아래의 예제와 같이 대상 객체를 build하는 Builder 클래스를 만들고, Builder 클래스를 통해 객체를 생성한다.

필수 설정 인자들을 Builder 객체의 객성자를 통해 받고, Optional 한 값들을 setter 메소드를 통해 설정하는 방식이 대표적이다. setter들은 this를 리턴하여 체이닝이 가능하게 함으로써, 깔끔하게 객체를 생성하는데 도움이 된다. optional한 값들을 모두 설정한 뒤 최종적으로 build() 메소드를 호출해 객체를 생성한다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/builder)

{:refdef: style="text-align: center;"}
![Builder Pattern Example UML](/assets/img/analysis/2021-03-14-22-43-42.png)
{: refdef}

## Singleton Pattern

어떤 클래스의 인스턴스는 오직 하나임을 보장하도록 구현하는 패턴이다. 생성된 객체를 전역적으로 접근 가능하다.

- Example

아래의 예시와 같이 해당 객체를 생성하는 것이 제한된 방식을 사용한다. 생성자를 private 으로 설정하여, getter 메소드로만 객체를 사용가능하다. 객체가 null 일 경우 단 한번 객체가 생성되고, 이후 해당 객체를 get 하여 사용한다.

이러한 방식은 멀티쓰레드 프로그램에서는 동기화 문제가 발생할 수 있다. 이를 방지하기 위해 getter 매소드에 synchronized 키워드를 추가하여 Thread-safe하게 만들어 주거나 static 변수 생성 시점에 미리 객체를 생성하는 방법을 사용할 수 있다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/singleton)

```java
// 구현 예제
public class MyOnlyOne {
    private static MyOnlyOne moo = null;

    private MyOnlyOne() {
    }

    public static MyOnlyOne getMyOnlyOne() {
        if (moo == null) {
            moo = new MyOnlyOne();
        }

        return moo;
    }
}
```

## Strategy Pattern

전략패턴은 특정한 동작들의 알고리즘들을 정의하고 캡슐화해서, 쉽게 교체하여 사용할 수 있도록 하는 패턴이다.

- Example

아래의 예시와 같이 GivingBirth와 Moving이라는 동작들을 Strategy라는 클래스로 캡슐화하여, Dog, Bird와 같은 Animal들에 각 동작들을 쉽게 교체하여 사용할 수 있도록하는 구조를 가지고 있다.

Dog, Bird 클래스 생성 후 EggStrategy, BabyStrategy, RunningStrategy, FlyingStrategy와 같은 Strategy 구현 객체들를 목적에 맞게 GivingBirth, Moving Strategy 변수들에 설정한 후 각 동작들을 사용한다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/strategy)

{:refdef: style="text-align: center;"}
![Strategy Pattern Example UML](/assets/img/analysis/2021-03-14-22-44-46.png)
{: refdef}

## Decorator Pattern

주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙이는 패턴이다. 기능 확장이 필요할 때 서브 클래싱 대신 유연하게 사용이 가능하다.

- Example

아래의 예제와 같이 'Decorator' 클래스들을 이용해 Printer에 기능을 추가(Decorating)하여 사용할 수 있다. Decorator 들을 목적에 맞게 다양하게 조합하여 Decorate된(기능이 추가된) Printer 객체를 생성 한뒤 사용할 수 있다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/decorator)

{:refdef: style="text-align: center;"}
![Decorator Pattern Example UML](/assets/img/analysis/2021-03-14-22-45-30.png)
{: refdef}

```java
// 사용 예제
Printer p = new PhotoPrinter();
System.out.println("--- Default Printer: " + p.print("my-photo"));

Printer pCuteFiltered = new CuteDecorator(new PhotoPrinter());
System.out.println("--- Default Printer with Cute Filter: " + pCuteFiltered.print("my-photo"));

Printer pBlackAndWhiteFiltered = new BlackAndWhiteDecorator(new PhotoPrinter());
System.out
        .println("--- Default Printer with BlackAndWhite Filter: " + pBlackAndWhiteFiltered.print("my-photo"));

Printer pCuteAndBlackAndWhiteFiltered = new CuteDecorator(new BlackAndWhiteDecorator(new PhotoPrinter()));
System.out.println("--- Default Printer with Cute Filter and BlackAndWhite Filter: "
        + pCuteAndBlackAndWhiteFiltered.print("my-photo"));
```

## Command Pattern

커맨드 패턴은 요청을 객체의 형태로 캡슐화하여, 요청을 조합해 사용하거나 요청을 기록하여 실행된 연산의 취소 작업을 실행할 수 있다.

- Example

아래의 예제에서는 Garbage Collecting 작업을 구현해 보았다. (GC 작업이 취소가 된다는 가정하에)

GCController의 gc() 메소드 호출 시, 커맨드들을 실행하고 실행된 커맨드 들을 'commandHistory Stack'에 저장해 둔다. 이후 revertAll() 메소드에서 실행된 커맨드들의 undo() 메소드들을 실행하여 작업 취소를 진행한다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/command)

{:refdef: style="text-align: center;"}
![Command Pattern Example UML](/assets/img/analysis/2021-03-14-22-46-15.png)
{: refdef}

## Iterator Pattern

Iterator(반복자) 패턴은 내부 표현을 노출하지 않고 어떤 객체 집합에 속한 원소들을 순차적으로 접근할 수 있는 방법을 제공하는 패턴이다.

- Example

아래의 예시에서는 Food와 같은 원소들을 Fridge라는 집합을 통해 관리하고 Iterating하는 기능을 제공한다. Fridge는 집합이라는 뜻의 Aggregate interface를 구현하여 iterator를 제공한다. iterator() 메소드를 이용해 iterator를 생성한 뒤, hasNext(), next() 메소드들을 사용해 원소들을 순차적으로 접근해 사용한다.

> java.util에 Iterator interface가 정의되어 있다.

[[예제 코드 링크]](https://github.com/taesunny/design-pattern-study/tree/main/src/iterator)

{:refdef: style="text-align: center;"}
![Iterator Pattern Example UML](/assets/img/analysis/2021-03-14-22-46-56.png)
{: refdef}

```java
// 사용 예제
Fridge fridge = new Fridge(5);
fridge.addFood(new Food("meat"));
fridge.addFood(new Food("juice"));
fridge.addFood(new Food("cola"));

System.out.println("- Current Foods Number: " + fridge.getLength());

Iterator<Food> it = fridge.iterator();

while (it.hasNext()) {
    Food food = it.next();
    System.out.println("Current Food: " + food.getName());
}
```

## Adapter Pattern for Class/Object

- 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환
- 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들이 함께 작동하도록 함

## Bridge Pattern

- 구현부에서 추상계층을 분리, 각자 독립적으로 변형할 수 있도록 함

## Composite Pattern (복합체)

- 객체들의 관계를 트리 구조로 구성하여 부분과 전체 계층을 표현
- 사용자가 단일 객체와 복합 객체 모두 동일하게 다루도록 함

## Facade

- 뜻) 정면, 표면
- 서브시스템에 있는 인터페이스 집합에 대해서 하나의 통합된 인터페이스를 제공
- 서브시스템을 좀더 사용하기 편하게 만드는 상위 수준의 인터페이스를 정의한다.

## Flyweight

- 크기가 작은 객체가 여러 개 있을 때, 공유를 통해 이들을 효율적으로 지원하는 패턴

## Proxy Pattern

- 어떤 다른 객체로 접근하는 것을 통제하기 위해서 surrogate(대리자) 또는 placeholder(빠진 것을 대신 하는 것, 자리채움자)를 제공

## Interpreter Pattern

## Template Method Pattern

## Chain of Responsibility Pattern

## Mediator Pattern

## Memento Pattern

## Observer Pattern

## State Pattern

## Visitor Pattern

## Abstract Factory Pattern

- 구체적인 클래스를 지정하지 않고 관련성을 갖는 객체들의 집합을 생성 or 서로 독립적인 객체들의 집합을 생성할 수 있는 인터페이스 제공

## Prototype Pattern

- 생성할 객체의 종류를 명세화 할 때, 원형이 되는 예시를 이용, 그 원형을 복사하여 새로운 객체를 생성한다.

---

# Source

- 도서 - GoF의 디자인 패턴
- 도서 - Effective Java
- [https://ko.wikipedia.org/wiki/소프트웨어_디자인_패턴](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EB%94%94%EC%9E%90%EC%9D%B8_%ED%8C%A8%ED%84%B4)
- [https://ko.wikipedia.org/wiki/빌더_패턴](https://ko.wikipedia.org/wiki/%EB%B9%8C%EB%8D%94_%ED%8C%A8%ED%84%B4)
- [https://ko.wikipedia.org/wiki/팩토리_메서드_패턴](https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4)
- [https://ko.wikipedia.org/wiki/전략_패턴](https://ko.wikipedia.org/wiki/%EC%A0%84%EB%9E%B5_%ED%8C%A8%ED%84%B4)