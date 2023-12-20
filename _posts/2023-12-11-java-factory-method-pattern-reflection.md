---
title: "팩토리 메서드 패턴(Factory method pattern)과 Java Reflection"
excerpt: "팩토리 메서드 패턴과 Java Reflection, 그리고 그 연계 사용에 대한 내용"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Design Pattern
  - 디자인 패턴
  - Factory method Pattern
  - 팩토리 메서드 패턴
  - Reflection
  - 리플렉션
---
## 팩토리 메서드 패턴
객체지향 디자인 패턴이다. 가상 생성자 패턴(Virtual Constructor Pattern)이라고도 한다. 부모 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며, 자식 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이다.

복잡한 생산 과정을 완전히 숨기고, 완성된 인스턴스만 반환한다.

팩토리 메서드 패턴을 사용하면, 객체를 생성하는 코드와 그 객체를 사용하는 코드를 분리할 수 있다. 따라서 코드의 유연성이 향상되고, 재사용성이 높아진다.

아래는 팩토리 메서드 패턴의 예이다.

```java
public class BeanFactory{
    // 2. 자기 자신 인스턴스를 참조하는 static한 필드를 선언.
    private static BeanFactory instance = new BeanFactory();

    // 1.private 생성자: 외부에서 인스턴스 생성하지 못함.
    private BeanFactory() {}
    
    // 3. 2번에서 생성한 인스턴스를 반환하는 static한 메서드를 만든다.
    public static BeanFactory getInstance() {
        return instance;
    }
}

```

```java
public class BeanFactoryMain() {
    public static void main(String[] args) {
        BeanFactory bf1 = BeanFactory.getInstance(); // BeanFactory에게 객체 생성과정을 맡김
        BeanFactory bf1 = BeanFactory.getInstance();
        if(bf1 == bf2) {
            System.out.println("bf1 == bf2");
        }
    }
}
```

→ new 연산자를 사용하는게 아니라 BeanFactory 클래스에서 객체를 생성하도록 한 Factory method 패턴이다.

## Java Reflection

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

→ 클래스 이름만 가지고 인스턴스를 만드는 동작을 구현할 수 있다

Spring 같은 경우도 Bean 생성 시 사용되는 패턴임. 물론 더 고차원적인 많은 가공을 사용하여 편의를 제공하는 것이다.

## 참고 자료

[팩토리 메서드 패턴 - 위키백과](https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4)

[리플렉션 개념 및 사용볍 - 로그의 개발일지](https://m.blog.naver.com/hj_kim97/223110095000)