---
title: "자바 코드의 동작을 JVM 단에서 살펴보자"
excerpt: "자바에서 각종 데이터는 어떻게 JVM가 관리하지?"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - JVM
  - 생성자
---
## 자바 코드의 동작

다음과 같은 간단한 코드를 생각해보자.

```java
public class Car {
	//필드 선언
	String model;
	int speed;

	//생성자 선언
	Car(String model) {
		this.model = model; //매개변수를 필드에 대입(this 생략 불가)
	}
	
	//메소드 선언
	void setSpeed(int speed) {
		this.speed = speed; //매개변수를 필드에 대입(this 생략 불가)
	}

	void run() {
		this.setSpeed(100);
		System.out.println(this.model + "가 달립니다.(시속:" + this.speed + "km/h)");
	}
}
```

```java
public class CarExample {
	public static void main(String[] args) {
		Car myCar = new Car("포르쉐");
		Car yourCar = new Car("벤츠");

		myCar.run();
		yourCar.run();
	}
}
```

### 1. 실행 전 클래스 정보의 메모리 로드
- 클래스 정보가 메모리에 로드된다: 메서드 영역, 클래스 메타 정보와 메서드가 저장된다.

### 2. 메인 메서드 실행 

- 메인 스레드가 스택에 올라온다. 이 때, 이 스택에는 args, myCar, yourCar 를 포함하고 있다. 셋 다 참조 변수로, 이 영역에 4 바이트씩 차지하고 있다.
- 생성자가 호출되며 `new` 예약어로 객체가 힙에 생성되고, `myCar` 참조 변수는 이제 이 객체를 가르킨다. 컴파일러가 생성자 메서드에 추가한 객체 스스로가 전달되어 this 예약어로 처리할 수 있게 된다.

### 3. 생성자 실행

- 생성자 `Car()` 가 호출되어 이 객체를 초기화하는 로직이 스택에 스레드로 발생한다.
- "포르쉐"는 Java에서 String을 관리하는 정적 변수로 메서드 영역에 생성되고, 이를 참조하는 참조 변수가 `Car()`로 전달된다. 이 부분에 대해서는 Java의 String 클래스에 대한 설명을 살펴보자: [Java의 String 클래스: String Constant Pool]({% post_url 2023-11-30-java-string-class %})
- Car() 생성자는 지역 변수로 model을 가지고 있고, 스택의 스레드에서 이 값은 전달받은 String인 model을 참조한다.
- 힙에 생성된 이 객체의 model 필드는 지역 변수인 model을 참조한다. 이 스레드는 종료, 스택에서 지워진다.
- `yourCar`도 마찬가지: 이 과정이 반복된다.

### 4. myCar 객체의 run()메서드가 실행된다.

-  myCar의 `run()`메서드가 실행되면서 해당 스레드가 스택 영역에 적재된다.
- `run()` 메서드는 지역 변수는 없지만 `setSpeed` 메서드에 전달할 때 100이라는 primitive 자료형을 저장한다
-  `this.setSpeed()` 가 실행되면서(해당 스레드가 스택에 추가된다.) 지역 변수로 생성된 100이 전달된다: Java에서 참조형이 아닌 경우는 call by value로 전달되므로 값이 복사된다.
- setSpeed()가 호출되어 힙에 저장 있는 myCar객체의 speed 필드가 해당 값으로 저장된다: call by value 이므로 값이 복사되어 힙 객체의 해당 메모리 주소에 직접 저장된다: int니 4 byte.
- 이후 스레드는 스택에서 제거된다.
- System클래스의 출력 메서드가 실행된다.
- run()메서드가 종료되면서 이 스레드도 스택에서 사라진다.
- 이후 동일한 과정이 yourCar 객체에 대해서도 동일하게 반복된다.

### 끝
- main메서드가 종료되면서 메인 스레드가 스택에서 완전히 비워진다.

## 생성자 오버로딩

다양한 방식으로 객체를 생성하고 싶은 경우. 여러 개의 생성자를 만들 수 있다. 물론 매개 변수는 다르게 받아야 한다.

- 오버로딩된 메서드를 다룰 때 컴파일러는 내부적으로 매개변수 타입을 메서드 이름에 명시하여 구분한다. 따라서 매개변수의 종류와 수가 다른 메서드만 오버로드 할 수 있는 것이다.
- 생성자 오버로딩이 많아질 경우 생성자 간의 중복된 코드가 발생할 수 있음: this() 예약어를 사용하면 원하는 생성자를 불러올 수 있다: 내 클래스의 다른 생성자를 불러올 수 있다.

```java
Car(String model) {
		this(model, "은색", 250);
}

Car(String model, String color) {
		this(model, color, 250);
}

Car(String moedel, String color, int maxSpeed) {
		this.model = model;
		this.color = color;
		this.maxSpeed = maxSpeed;
}
```

이 코드에서 맨 마지막 메서드가 중심 내용을 구현하고 있다. 위의 두 메서드는 이 메서드를 감싸는 래핑 메서드로 **단일 책임 원칙**을 적용하고자 이런 방식으로 구현한 것이다. 이렇게 구현하면 코드의 유지 보수가 용이하고 코드의 유연성이 증가한다.

**래핑 함수를 왜 사용하는지 보여주는 좋은 예 중 하나이다.**

참고: Java - **Wrapper 클래스 / Boxing & Unboxing**