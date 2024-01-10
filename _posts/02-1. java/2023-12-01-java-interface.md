---
title: "인터페이스(Interface)"
excerpt: "자바의 인터페이스"

toc: true
toc_sticky: true

categories:
  - Java
tags:
--- 
기능들의 목록을 가지고 있는 것: 무슨 기능을 만들어야 할 지 메서드에 대한 선언만을 가지고 있다. 즉, 규칙을 잡거나, 플로우 로직을 잡는데 사용한다.

객체의 사용 방법을 정의한 타입이다. 객체의 교환성을 높여주기 때문에
- Upper CamelCase로 작성된다.
- 확장자가 .java파일로 작성한다.
- 모든 필드는 public static final이어야 하며, 모든 메서드는 public abstract이어야 한다. (Java 7까지는) final, abstract를 생략하면 자동으로 붙는다.
- Java 8 부터는 default 메서드(구현된 메서드)와 static 메서드도 선언이 가능하다.

 
## 인터페이스의 default 메서드
JDK 8에 추가된 메서드

default 메서드가 인터페이스에 추가된 이유에 대해 생각해보자.

 다음과 같은 상황이 발생한다면

- A라는 사용자가 메서드가 3개가 선언된 인터페이스를 작성한 후 외부에 공개하였다(라이브러리).
- 여러 사용자들이 해당 인터페이스를 사용하여 구현하였다.
- A라는 사용자는 1개의 메서드가 더 필요하다고 생각하여 메서드를 추가하였다.
- 여러 사용자들은 라이브러리가 업데이트 된 줄 알고 업데이트 하였다. 무슨 일이 벌어질까?

→ 부모 클래스에 추상 메서드가 하나 추가되었으므로 업데이트를 하게되면 여러 사용자들의 구현체는 컴파일 에러가 발생한다(호환성을 해침): default 메서드의 도입. 상속받듯이 사용할 수 있기 때문에 컴파일 에러가 발생하지 않는다. 원한다면 오버라이드도 할 수 있음.

 
> 번외: 자바 프로그램에서 호환성 문제
>
>- 바이너리 호환성: 에러 없이 기존 바이너리가 실행될 수 있는 것을 바이너리 호환성이라고 한다.
>- 소스 호환성: 코드를 고쳐도 기존 프로그램을 성공적으로 컴파일 할수 있다면 소스 호환성이 있다고 한다.
>- 동작 호환성: 같은 입력값이 주어져도 동작하는 것을 동작 호환성이라고 한다.

 
## Default 메서드의 사용
default 메서드를 사용하는 방식은 두 가지가 있다.

### 1. 선택형 메서드 Optional method

```java
public interface Iterator<E> {
	...
    
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

    ...
}
```
잘 사용하지 않는 remove()메서드를 default 메서드로 변경(Java 8)됨으로써 사용하지 않는 메서드를 구현할 필요가 없어졌다.

### 2. 동작 다중 상속 Multiple inheritance of behavior

Java 8에서는 인터페이스가 구현을 포함할 수 있으므로 여러 인터페이스에서 동작을 상속받을 수 있다. 중복되지 앟ㄴ는 최소한의 인터페이스를 유지한다면 코드에서 동작을 쉽게 재사용하고 조합할 수 있다.

#### 기능이 중복되지 않는 최소 인터페이스에서

```java
public interface SportCar {
    void setOutput(int output);
    int getOutput();
    default void setBoosterOutput() {
        setOutput(getOutput() * 2);
    }
}
```
setBoosterOutput 메서드는 구현 클래스에서 구현하지 않아도 된다.

#### 인터페이스 조합하기

인터페이스를 조합하여 다양한 클래스를 구현할 수 있다.
```java
public interface VacuumCleaner { // 진공 청소기
    default void vacuumCleaning() {
        // 진공 청소 구현
    }
}

public interface BeddingCleaner { // 침구 청소기
    default void beddingCleaning() {
        // 침구 청소 구현
    }
}

public interface WaterMop { // 물 걸레
    default void mopping() {
        // 물 걸레 청소 구현
    }
}

public class SamsungCleaner implements VacuumCleaner, BeddingCleaner, WaterMop {
   
}
```

이런 식으로 구현하면 디폴트 메서드를 수정시 상속받는 클래스들의 리팩터링도 수월하다.

 
## 인터페이스의 다중 상속으로 인한 특수한 상황들

상속에 대한 내용은 다른 글에서 살펴보고 여기에서는 인터페이스와 관련된 이슈를 살펴보자.

상속받은 두 디폴트 메서드의 시그니처가 같다면 override하여 어떤 시그니처를 사용(super)할 것인지, 혹은 구현을 해야 한다.

```java
public class C implements B, A {

    @Override
    public void run() {
        B.super.run();
    }
}
```

- 같은 시그니처를 갖는 인터페이스들이 상속관계라면, A(인터페이스)-상속->B(인터페이스)-상속->C: 서브 인터페이스 B가 우선권을 갖는다.
- 클래스에서 정의한 메서드와 인터페이스의 디폴트 메서드 시그니처가 같은 경우: 클래스나 슈퍼클래스에서 정의한 메서드가 디폴트 메서드보다 우선권을 가짐.

## Static Interface Method

일반적인 클래스에 선언하는 static 메서드와 같이 동작한다. 호출도 메서드명 앞에 인터페이스 이름을 넣어 호출한다.

객체 생성 없이 한 공간에서 연관된 메서드들의 결합도를 상승시킨다. 추상클래스와 유사하지만 추상클래스는 constructor, statem, behavior가 있다.

 
## 마무리

객체지향적 관점으로 이상적인 인터페이스는 복합 기능을 지양하고 특정 타입의 API를 정의하는데 사용하여야 한다. 하지만 추가된 기능들의 의의는 기존 코드와의 호환성과 유지보수의 용이성을 위한 유용한 기능을 제공한다는 것에 의미가 있다.

 
## 참고자료

https://devbksheen.tistory.com/entry/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9Cdefault-method%EB%9E%80

https://soft-dino.tistory.com/25
