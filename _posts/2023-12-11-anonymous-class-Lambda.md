---
title: "익명 클래스와 람다"
excerpt: "익명 클래스와 람다 처음 보기"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
## 이름 없는 클래스(Anonymous Class)

- ``new 생성자() { ... }``
- 생성자의 뒤에 중괄호가 나오고 코드를 오버라이딩하여 구현한다.

추상 클래스인 Car는 객체 생성이 불가능하다. Car를 상속 받지만 상속 개체를 사용하고 싶지 않을 때에 익명 클래스를 사용하게 된다.

```java
public class CarExam{
    public static void main(String[] args) {
        Car c1 = new Car() { // 익명 클래스: 추상클래스의 메서드를 오버라이드 해야한다.
            @Override
            public void a() {
                System.out.println("이름 없는 객체의 a() 오버라이딩")
            }
        };

        c1.a(); // 이름 없는 객체의 a() 오버라이딩 이 출력됨.
    }
}
```

- Interface를 객체로 생성하는 경우 IntelliJ가 자동으로 익명 클래스 템플릿을 작성해 준다.

## 람다 인터페이스(Lambda)
```Java
() {
    System.out.println("Runnable!");
}
```
메서드를 하나만 가지고 있는 인터페이스

- 이름 없는 객체를 간략화 시켜서 사용한다.
- 람다 인터페이스를 사용한 람다 표현식은 JDK 8에서 추가되었다.
- JDK 8에서 추가된 이러한 문법을 사용할 때 보통 모던 자바(Modern Java) 라고 한다.
- Stream API와 같이 쓰면 편리한 코드를 작성할 수 있다.

## 참고
- 익명 클래스에서 this는 익명 클래스의 인스턴스 자신을 가르키지만,
- 람다 인터페이스에서는 바깥 인스턴스를 가르킨다.