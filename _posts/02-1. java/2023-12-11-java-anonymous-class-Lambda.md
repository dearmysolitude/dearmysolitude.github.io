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
메서드를 하나만 가지고 있는 인터페이스

우선 다음 코드를 살펴보자.

```java
public class MyRunnableMain {
    public static void main(String[] args) {
        MyRunnable r = new MyRunnable() {
            @Override
            public void run() {
                System.out.println("MyRunnable run!");
            }
        };

        r.run();
    }

}

public interface MyRunnable {
        public void run();
}
```

인터페이스는 객체를 가질 수 없기 때문에 객체를 인터페이스로 생성해주면 이를 구현하도록 IDE가 템플릿을 위와 같이 제공한다.

다음 케이스를 살펴보자.

```java
public class RunnableExecute {
    public void execute(MyRunnable myRunnable) {
        myRunnable.run();
    }
}
```

```java
public class MyRunnableMain2 {
    public static void main(String[] args) {

        MyRunnable myRunnable = new MyRunnable() {
            @Override
            public void run() {
                System.out.println("hello");
            }
        }

        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(myRunnable);
    }
}
```

RunnableExecute의 execute메서드는 MyRunnable 인터페이스 객체를 전달받아 실행한다.

이 메인 클래스는 다음과 같이 간략하게 쓸 수 있다.

```java
public class MyRunnableMain2 {
    public static void main(String[] args) {

        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(new MyRunnable() {
            @Override
            public void run() {
                System.out.println("hello");
            }
        });
    }
}
```

재사용할 일이 없다면 굳이 클래스를 만들어줄 필요가 없으니 이렇게 익명 클래스를 사용하고 최대한으로 줄이는 것이다.

여기에서 람다식을 이용해 더 간단하게 만들 수 있다.

```java
public interface MyRunnableMain3 {
    public static void main(String[] args) {

        RunnableExecute runnableExecute = new RunnableExecute();
        runnableExecute.execute(() -> { //메서드 이름까지 지워버림: 이름없는 객체를 만들어 전달
                System.out.println("hello");
            }
        );
    }
}
```

- 이름 없는 객체를 간략화 시켜서 사용한 것이다.
- 메서드를 단 하나만 가지고 있어야 한다.
- 람다 인터페이스를 사용한 람다 표현식은 JDK 8에서 추가되었다.
- JDK 8에서 추가된 이러한 문법을 사용할 때 보통 모던 자바(Modern Java) 라고 한다.
- Stream API와 같이 쓰면 편리한 코드를 작성할 수 있다.

## 참고
- 익명 클래스에서 this는 익명 클래스의 인스턴스 자신을 가르키지만,
- 람다 인터페이스에서는 바깥 인스턴스를 가르킨다.