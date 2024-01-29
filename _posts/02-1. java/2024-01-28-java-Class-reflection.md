---
title: "자바 Class 와 Reflection"
excerpt: "자바의 클래스 객체와 이를 이용한 자바 reflection에 대해 살펴보자."

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - JVM
  - 생성자
  - Reflection
---

## 각 클래스도 OS입장에서는 '클래스'의 특별한 케이스이다.
- OS 단에서 보면 특정 클래스도 클래스의 특별화된 객체이다. '클래스'로 일반화 한 것.
- `Car.class.getName();`  << 이 코드를 살펴보자
- 클래스의 이름 뿐 아니라, 메서드나 필드도 가져올 수 있다(일반화)

## `Class`

- 클래스에 대한 메타데이터를 담고 있다. 클래스 이름, 슈퍼클래스, 인터페이스, 필드, 메서드를 포함한다.
- 모든 클래스는 `class` 라는 이름의 정적 필드를 가지며, 이 필드는 해당 클래스의 `Class`객체를 참조한다.
- 이를 통해 해당 클래스에 대한 정보를 얻거나, 리플렉션(Reflection)을 사용하여 동적으로 코드를 실행할 수 있다.

### 자바 Reflection

### 왜 자바 리플렉션과 자바 클래스를 같이 보는거지?

리플렉션은 자바에서 제공하는 API로, 런타임에 클래스, 인터페이스, 필드 및 메서드 같은 클래스 멤버에 대한 정보를 가져오거나 새로운 인스턴스를 생성하거나, 메서드를 호출하거나 필드 값을 가져오거나 설정하는 등 작업을 할 수 있게 해 준다. 동적으로 코드를 실행하고, 클래스의 구조를 분석하거나 변경할 수 있다.

### 자바 리플렉션이란?

런타임 시점에 프로그램 내부 구조를 분석하고, 조작할 수 있는 기능. JVM은 클래스 정보를 클래스 로더를 통해 읽어와서 해당 정보를 JVM에 저장한다.

주요 기능

- 객체의 클래스 정보를 가지고 올 수 있다.
- 클래스의 필드 정보를 가져오거나 수정할 수 있다.
- 클래스의 메서드 정보를 가져오거나 실행할 수 있다.
- 클래스의 생성자 정보를 가져오거나 객체를 생성할 수 있다.
- 어노테이션 정보를 가져올 수 있다.

장점

- 런타임 시점에서 클래스 인스턴스를 생성하고
- 접근 제어자와 관계없이 필드와 메서드에 접근하여 작업을 수행할 수 있는 유연성을 가지고 있다.

단점

- 하지만, 일반적인 메서드보다 오버헤드가 크다.
- 동적 바인딩과 메타데이터 접근에 따른 비용이 발생한다. 
- 코드의 가독성과 유지보수성이 저하될 수 있다.


### 클래스 로더를 이용한 인스턴스 생성

```java
public class BeanFactoryMain() {
    public static void main(String[] args) {
        // a() 메서드를 가지고 있는 클래스가 있음.
        // 이 클래스의 이름이 무엇인지는 알 수 없음.
        // 나중에 알게 될 것임.
        // a() 메서드를 실행할 수 있도록 코드를 작성하시오. 

        // 1. 클래스 이름으로 들어갈 String
        String className = "com.example.Bus";
        // 2. className에 해당하는 클래스 정보를 CLASSPATH에서 읽어들여서 clazz에서 참조한다. 
        Class clazz = Class.forName(className);

        Method[] declaredMethods = clazz.getDeclaredMethod(); // 클래스에서 메서드들을 객체로 가져옴
        for(Method m :  declaredMethods) {
            System.out.println(m.getName()); 
        }
        
        // 3. 클래스 정보를 참조하여 객체를 생성한다: 이제, 메서드는 어떻게 사용할 수 있을까?
        Object o = clazz.newInstance();

        // 3-1. Bus 객체를 가져와 형변환하여 메서드 사용.
        Bus b = (Bus)o; 
        b.a(); 

        // 3-2. Bus가 Car의 부모 클래스일 때: a 메서드는 오버라이드된 Bus의 a()가 사용된다.
        Car b = (Car)o; 
        b.a(); 

        // 3-3. 메서드도 Java Reflection으로 런타임 시에 불러올 수 있다: Bus가 아니더라도, Car의 자식 클래스가 아니더라도 가능함
        Method m = clazz.getDeclaredMethod("a", null); // a 메서드에 대한 정보를 가지고있는 Method를 반환하라: "a"는 메서드 이름, null은 parameterTypes
        m.invoke(o, null); // Object o가 참조하는 객체의 m 메서드를 실행하라. null은 args
    }
}
```

- 프린트 되는 것은 clazz가 참조하는 Bus클래스의 메서드들의 이름
- 일련의 1, 2, 3, 4 과정은 ``Bus b = new Bus();``와 동일하다.
- 메서드를 불러올 때에는 3-1, 3-2, 3-3과 같은 방법으로 불러올 수 있다.

## 팩토리 메서드 패턴과 Java Reflection의 결합

[펙토리 메서드 패턴]({% post_url 2023-12-11-java-factory-method-pattern %})

→ 클래스 이름만 가지고 인스턴스를 만드는 동작을 구현할 수 있다

Spring 같은 경우도 Bean 생성 시 사용되는 패턴임. 물론 더 고차원적인 많은 가공을 사용하여 편의를 제공하는 것이다.


### 그러나

예기치 않은 부작용을 초래할 수 있다. 

- 특히, 접근 제어자를 무시하고 private 필드에 접근
- private 메서드를 호출

하는 등의 작업은 코드 안정성을 헤칠 수 있기 때문이다.

## 참고 자료

[자바 리플렉션 간단 설명 및 사용 방법 정리](https://blog.naver.com/PostView.nhn?blogId=gracefulife&logNo=220627537434)

[자바 리플레션 소개 및 사용법, 예제](https://hbase.tistory.com/350)

[리플렉션 개념 및 사용볍 - 로그의 개발일지](https://m.blog.naver.com/hj_kim97/223110095000)