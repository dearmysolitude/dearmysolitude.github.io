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

## 참고 자료

[팩토리 메서드 패턴 - 위키백과](https://ko.wikipedia.org/wiki/%ED%8C%A9%ED%86%A0%EB%A6%AC_%EB%A9%94%EC%84%9C%EB%93%9C_%ED%8C%A8%ED%84%B4)

[리플렉션 개념 및 사용볍 - 로그의 개발일지](https://m.blog.naver.com/hj_kim97/223110095000)