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

**이제 이걸 그림으로 그려봅시다**

1. 클래스 정보가 메모리에 로드된다: 클래스의 진입점(메인 메서드)에서 코드가 시작된다.
2. 메인 스레드가 스택에 올라온다. 이 때, 이 스택에는 args, myCar, yourCar 를 포함하고 있다. 셋 다 참조 변수로, 이 영역에 4 바이트 씩 차지하고 있다.
3. 생성자가 호출되며 `new` 예약어로 객체가 힙에 생성되고, `myCar` 참조 변수는 이제 이 객체를 가르킨다. 컴파일러가 생성자 메서드에 추가한 객체 스스로가 전달되어 this 예약어로 처리할 수 있게 된다.
5. 생성자 `Car()` 가 호출되어 스택에 해당 스레드가 발생한다.
6. "포르쉐"는 Java에서 String을 관리하는 정적 변수로 메서드 영역에 생성되고, 이를 참조하는 참조 변수가 `Car()`로 전달된다.
7. Car() 생성자는 지역 변수로 model을 가지고 있고, 스택의 스레드에서 이 값은 전달받은 String인 model을 참조한다.
7. 힙에 생성된 이 객체의 model 필드는 지역 변수인 model을 참조한다. 이 스레드는 종료, 스택에서 지워진다.
8. `yourCar`도 마찬가지
9. myCar의 `run()`메서드가 실행되면서 해당 스레드가 스택 영역에 발생한다.
10. `run()` 메서드는 지역 변수가 없으므로 아무런 변수를 가지고 있지 않다.
11. `this.setSpeed()` 가 실행되면서(해당 스레드가 스택에 추가된다.) 지역 변수로 생성된 100이 전달된다. << 이 부분이 조금 애매하네
12. setSpeed가 호출되어 힙에 있는 myCar객체의 speed 필드가 해당 값으로 변경된다.
13. setSpeed는 지역 변수로 speed를 가지고 있고, 이를 외부로부터 전달받은 값으로 대입된다. 이 메서드를 불러온 객체의 필드 값을 해당 값으로 대입한다.
14. 이후 스레드는 스택에서 제거된다.
15. System클래스의 출력 메서드가 실행된다.
16. run()메서드가 종료되면서 이 스레드도 스택에서 사라진다.
17. yourCar도 동일한 과정을 반복한다.
18. main메서드가 종료되면서 스택이 완전히 비워진다.

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