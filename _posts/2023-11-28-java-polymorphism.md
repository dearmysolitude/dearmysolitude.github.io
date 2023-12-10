---
title: "다형성(Polymorphism)"
excerpt: "다형성의 개념과 Java에서 다형성을 다루는 기법들"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - 다형성
  - Polymorphism
---
프로그램 언어 각 요소(상수, 변수, 식, 객체, 메서드 등)이 다양한 자료형(type)에 속하는 것이 허가되는 성질을 가리킨다(위키피디아, [다형성]). 또는 여러 형태를 받아들일 수 있는 성질, 상황에 따라 의미를 다르게 부여할 수 있는 특성 등으로 정의하기도 한다.

다형성을 활용하면 기능을 확장하거나, 객체를 변경해야 할 때 타입 변경 없이 객체 주입만으로 수정이 일어날 수 있다. 또한 상속을 사용하여 중복 코드까지 제거할 수 있어 객체 지향 설계와 가까워질 수 있다. 다형성 구현에는 다양한 방법이 있을 수 있지만, 오버로딩, 오버라이딩, 함수형 인터페이스가 대표적이다.

→ Tecoble, [다형성] 페이지에서

 
## 오버로딩 Overloading

- 다양한 매개변수를 다양하게 받아들이는 메서드를 여러 개 만들어서 사용자가 편리하게 사용하도록 하는 것
- 오버로딩은 여러 종류의 타입을 받아들여 결국 같은 기능을 하도록 하기 위한 작업이다.
- 하지만 메서드 오버로딩을 구현한 경우 요구사항이 변경되었을 때 모든 메서드에서 수정이 불가피하므로 필요한 경우에만 하는 것이 필요하다: 테코블 페이지 필자의 경우에는 생성자 오버로딩을 자주 사용하는 듯 하다.

예: System.out.println(); <br> 
 
## 오버라이딩 Overriding

- 부모 클래스가 가지고 있는 메서드를 리턴 타입, 매개 변수, 메서드 이름을 동일하게 가지고 있는 것
- 상위 클래스가 가지고 있는 메서드를 자식 클래스에서 재정의 하는 것.
- 메서드가 오버라이딩되면 무조건 자식의 메서드가 실행된다: 자식 객체를 생성하여 참조 타입을 부모 타입으로 하더라도 오버라이딩 되어있는 메서드는 항상 오버라이딩된 메서드가 실행된다(즉 자식의 메서드가 실행된다).
- 참조 타입이 부모 타입이라면 부모타입의 메서드만 사용 가능하다. 사용하기 위해서는 형변환이 필요. 오버라이딩된 메서드의 경우 무조건 자식의 메서드가 실행된다.

- 하지만 **필드의 경우 참조 type을 따라간다.** 그 이유는,
    - 클래스가 작성될 때에는 클래스가 상속을 하게될지 안하게 될지 알 수 없다.
    - 상속 클래스가 생성되었을 때 필드를 상속된 클래스에서 가져오게되면 예상치 못한 동작을 일으킬 수 있다.
    - 즉, 클래스의 메서드를 상속하여 메서드를 확장하여 사용하고자 상속 개념을 사용하지만, 필드의 경우에는 의도대로 클래스를 사용하도록 상속하지 않는 것임
    - 이는 객체 지향적 관점에서 정보 은닉 전략을 따르기 위한 설계이다: 객체의 속성은 그 객체의 메서드를 통해서만 변경된다.

정보 은닉(Information hiding)은 객체 지향의 중요한 기법이다. 중요한 필드는 은닉하고, 필드는 메서드를 통해서만 접근하여 사용하도록 하는 것이다.

추상클래스 뿐 아니라 인터페이스도 구현의 방법만 다를 뿐 동일하게 메서드 오버라이딩을 활용할 수 있다.

 
## 함수형 인터페이스 Functional Interface

- 람다식을 사용하기 위한 API로, 자바에서 제공하는 인터페이스에 구현할 메서드가 하나뿐인 인터페이스를 의미한다. enum과 함께 사용하면 다형성의 장점을 활용할 수 있다.
- 다음은 테코블 블로그에서 예시로 사용한 문자열 계산기 클래스
```java
public enum Operator {
    PLUS("+", (a, b) -> a + b),
    MINUS("-", (a, b) -> a - b),
    MULTIPLY("*", (a, b) -> a * b),
    DIVIDE("/", (a, b) -> a / b);

    private final String sign;
    private final BiFunction<Long, Long, Long> bi;

    Operator(String sign, BiFunction<Long, Long, Long> bi) {
        this.sign = sign;
        this.bi = bi;
    }

	  public static long calculate(long a, long b, String sign) {
    	  Operator operator = Arrays.stream(values())
            	.filter(v -> v.sign.equals(sign))
            	.findFirst()
            	.orElseThrow(IllegalArgumentException::new);

    	  return operator.bi.apply(a, b);
	  }
}
```

- 메인 클래스
```java
public static void main(String[] args) {
    String question = "4*7";
    String[] values = question.split("");

    long a = Long.parseLong(values[0]);
    long b = Long.parseLong(values[2]);

    long result = Operator.calculate(a, b, values[1]);
    System.out.println(result); //28
}
```
 
## 정리

- 다형성은 코드 중복을 줄이면서 변경과 확장에 유연한 객체 지향적인 코드를 작성하는데 유용하다.
- 다형성의 구현을 위해서는 공통 특성을 타입으로 추상화하고 그것을 상속(인터페이스라면)해야한다. <br> 

## 참고 자료

즐거운 자바: 다형성

[https://tecoble.techcourse.co.kr/post/2020-10-27-polymorphism/](https://tecoble.techcourse.co.kr/post/2020-10-27-polymorphism/)