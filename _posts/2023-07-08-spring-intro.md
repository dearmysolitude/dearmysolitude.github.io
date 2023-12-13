---
title: "Spring Framework: Intro"
excerpt: "Spring Framework를 시작하기 위한 기본적인 이해. + 자바 문법: var 자료형, try-with-resources 문"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
---
- [Spring Boot 3 & Spring Framework 6 마스터 - Udemy](https://www.udemy.com/course/spring-boot-and-spring-framework-korean/learn/lecture/36692976#content)
- [강의 Github Repository](https://github.com/in28minutes/master-spring-and-spring-boot/tree/main/01-spring)

## Spring

Spring은 각종 개발에 대한 core feature를 제공한다. Spring boot은 spring을 통해 쉽게 접근할 수 있다.

## Spring Container

- Spring Bean과 그 life cycle을 관리한다(클래스 인스턴스를 생성하고, 연결하고, 생성부터 파괴까지).
- Spring Container(IoC Container)에는 bean factory와 application context가 있음.
- Dependency Injection을 사용하여 애플리케이션을 구성하는 컴포넌트를 관리한다.
- IoC Container를 포괄하는 더 큰 개념

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216713&authkey=%21AMnz2r_HBzykDMU&width=292&height=333" alt="">
  <figcaption>IoC Container</figcaption>
</figure>

**IoC Container**
IoC(Inversion of Control) = DI(dependency injection)
- 애플리케이션의 객체를 인스턴스화하고, 구성하며, 이들 객체 간의 의존성을 조립하는 역할을 한다.
- XML, Java주석, Java 코드로 제공된 구성 메타데이터에서 빈의 정보를 얻는다.

> **Inversion of Control**
>
> 소프트웨어 엔지니어링에서 사용되는 디자인 패턴으로, 프로그램 흐름 제어를 사용자 정의 코드에서 제네릭 프레임워크로 이전하는 것을 의미한다. 이로 인해 흐름 제어가 반전되어 inversion of control이라고 한다.
>
> 절차적 프로그래밍에서는 프로그램의 사용자 정의 코드가 일반 작업을 처리하기 위해 라이브러리를 호출하지만, IoC에서는 프레임워크가 사용자 정의 코드를 호출한다. 
>
> **Dependency Injection**
>
> 객체가 필요로하는 다른 객체(의존성)을 제공하는 소프트웨어 엔지니어링 기법. 사용할 수 있는 객체(서비스)이다.
> Inversion of Control의 구체화된 기법이다.
>
> [What is inversion of control - educative](https://www.educative.io/answers/what-is-inversion-of-control)

### **Bean Factory**

Basic Spring Container

### **Application Context**

Advanced Spring Container with enterprise-specific features
	- Easy to use in web applications
	- Easy internalization
	- Easy integration with Spring AOP

- Bean Factory의 하위 인터페이스로, Bean Factory 의 모든 기능을 포함하며, 추가로 엔터프라이즈 특정 기능을 제공한다.
- 추가 기능은 위에서 언급한 Spring의 AOP와의 간편한 통합, 메세지 리소스 처리(국제화를 위한), 이벤트 발행, 웹 애플리케이션에 대한 애플리케이션 계층 특정 컨텍스트(예: WebApplicationContext) 등이 포함된다.

발췌

[Spring Application Context - Baeldung](https://www.baeldung.com/spring-application-context)

[Difference Between Inversion of Control and Dependency Injection - geeksforgeeks](https://www.geeksforgeeks.org/spring-difference-between-inversion-of-control-and-dependency-injection/)

### 뭘 쓸까?

Most enterprises applications use **Appliciation Context**
	- web applications, web services 에 추천된다.
	- REST API나 microservices

## Coupling

**Tightly coupled**

매개변수 등으로 객체간 연결이 강하게 된 코드

**Loosely coupled**

Spring은 객체간 결합이 loose하게 하여 여러 부분에 반복적으로 사용하는 객체를 중복없이 사용하는 코드의 작성을 돕는다.

## Java와 Spring에서 객체

### Java Bean

Classes adhering to 3 constraints:

1. Have public default (no argument) constructors
2. Allow access to their properties using getter and
setter methods
3. Implement java.io.Serializable

>`java.io.Serializable`
> 자바에서 객체 직렬화를 위해 사용된다. 객체 직렬화란 데이터를 바이트(byte) 형태로 변환하는 기술이며, 이를 통해 객체 단위로 데이터를 파일에 저장하거나 네트워크를 통해 전송할 수 있다.
> 마커 인터페이스로, 어떤 멤버 변수나 메서드도 가지고 있지 않다. 이 인터페이스를 구현한 클래스의 객체만 직렬화될 수 있다.

### POJO(Plain Old Java Object)

No constraints

Any Java Object(모든 자바 객체) is a POJO!

### Spring Bean

Any Java object that is managed by Spring

Spring uses IOC Container (Bean Factory or Application Context) to manage these objects

Spring에서, 응용 프로그램의 기반을 형성하는 객체들이 Spring IoC 컨테이너에 의해 관리되는데 이러한 객체들을 bean이라고 한다. [bean은 Spring IoC 컨테이너에 의해 인스턴스화, 조립 및 관리되는 객체이다](https://www.baeldung.com/spring-bean).
 

## 실습

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216714&authkey=%21ALbq_R2u_lT0Q44&width=173&height=457" alt="">
  <figcaption>IoC Container</figcaption>
</figure>

- Design: Game Runner가 다양한 게임들을 동작시키는 것.
- 순서: Tightly coupled → Loos coupled→ Spring 1 → Spring 2: 강의의 Github Repository를 확인해보자.
- Spring Bean을 활용한 Loose coupled된 gaming console.
- `GameRunner`위에서 돌아가는 `GamingConsole` 인터페이스를 통해`Mario/SuperContra/PacMan`게임을 구현하였다.

### Main

**Spring 컨텍스트 생성, Bean 객체 검색**

```java
// App03 GamingSpirnBeans.java
package com.jy.learnspringframework;

import com.jy.learnspringframework.game.MarioGame;
import com.jy.learnspringframework.game.PacManGame;
import com.jy.learnspringframework.game.SuperContraGame;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.jy.learnspringframework.game.GameRunner;
import com.jy.learnspringframework.game.GamingConsole;

public class App03GamingSpringBeans {

	public static void main(String[] args) {
            try(var context = 
                new AnnotationConfigApplicationContext(GamingConfiguration.class)) {
                // annotation config 를 가져와 바인딩
                    context.getBean(GamingConsole.class).up();
                
                    context.getBean(GameRunner.class).run();
            }
	}
}
```

`App03GamingSpringBeans`클래스는 Spring 어플리케이션 컨텍스트를 생성하고, 해당 컨텍스트에서 bean객체를 검색하는 역할을 한다. `AnnotationConfigApplicationContext` 클래스의 인스턴스를 생성할 때, GamingConfiguration.class를 전달하여 해당 클래스 구성정보를 통해 애플리케이션 컨텍스트를 생성한다. 

이해가 되지 않아 조금 살펴보았다.

- `AnnotationConfigApplicationContext`: 이 클래스는 Spring의 Java 기반 구성을 사용하여 애플리케이션 컨텍스트를 생성하는 데 사용됩니다.
- `try-with-resources` 문을 사용하여 `context` 변수에 애플리케이션 컨텍스트 인스턴스를 할당. 이렇게 하면 `try` 블록이 종료될 때 자동으로 `context` 변수가 닫히게 됩니다.
    - `try-with-resources`문: Java 7에서 도입된 자원 관리를 위한 구문이다.
    - try 블록 내의 모든 자원을 자동으로 닫아준다. 이 자원은 프로그램이 끝난 후에 반드시 닫혀야 하는 객체임을 의미한다.
    - 이러한 자원은 `java.lang.AutoClosable`인터페이스를 구현한 객체이거나 이 인터페이스를 구현한 모든 객체를 포함한 `java.lang.Closable`인터페이스를 구현한 객체일 수 있다.
    - `finally`구문을 사용하여 명시적으로 닫는 것보다 코드가 간결해지고, 자원 누수를 방지할 수 있다. 이 경우에는 예외로 인한 중단에도 자원이 닫히게 된다.
- 그 다음으로, `context.getBean(BusinessCalculationService.class)`** 메소드를 호출하여 `GamingConsole`, `GameRunner` 타입의 bean 객체를 검색하고, 이 객체의 `up()`, `run()` 메소드를 호출하여 결과를 출력한다.
- `var` 자료형
    - 컴파일러가 프로그래머의 의도를 추측하여 타입을 추론하는데 사용된다.
    - [이를 타입 추론이라고 한다.](https://digiconfactory.tistory.com/entry/%EC%9E%90%EB%B0%94-10-var-%EC%9D%98-%EC%82%AC%EC%9A%A9-%EC%9E%90%EB%A3%8C%ED%98%95-%EC%97%86%EC%9D%B4-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
    - 변수를 선언할 때 자료형을 명시하지 않아도 된다. 컴파일러가 프로그램 작성자의 의도를 추측하여 자료헝을 결정한다.
    - 자바스크립트나 파이썬과 같은 동적 타입 언어와는 다르게 한번 초기화 되면 변경할 수 없다.
    - 지역 변수에서만 사용할 수 있으며 전역 변수에서는 사용할 수 없다: 사용 범위를 제한하여 실수를 줄여준다.
    - 제네릭 프로그래밍에서 실수를 줄여준다: `HashMap<Integer, String> myHash = new HashMap<Integer, String>();`을 `var simpleHash = new HashMap<Integer, String>();`으로 줄일 수 있다.
    - 자바의 타입 시스템에 유연성을 추가하면서도, 강력한 타입 안정성을 유지하는데 도움을 준다.
    - 하지만 과도하게 사용하게 되면 가독성을 떨어뜨릴 수 있으며, 예상치 못한 타입 오류가 발생할 수 있다.

### Spring 구성 클래스, `@Configuration`: `GameRunner`, `GameingConsole`

```java
// GamingConfiguration.java

package com.jy.learnspringframework;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.jy.learnspringframework.game.GameRunner;
import com.jy.learnspringframework.game.GamingConsole;
import com.jy.learnspringframework.game.PacManGame;

@Configuration
public class GamingConfiguration {
	@Bean
	public GamingConsole game() {
		var game = new PacManGame();
		return game;
	}
	@Bean
	public GameRunner gameRunner(GamingConsole game) {
		var gameRunner = new GameRunner(game);
		return gameRunner;
	}
}
```

- `@Configuration`: 이 클래스가 Spring 구성, 즉 설정 클래스임을 나타낸다.
- `@Bean`: 이 객체를 Bean으로 선언하여 Spring에서 객체를 관리하도록 한다.
- `GameRunner`: `GamingConsole`, 아래 나올 인터페이스를 통해 loose bind를 구현한 것이다.

### Game: 인터페이스, `GamingConsole`

```java
package com.jy.learnspringframework.game;

public interface GamingConsole {
	void up();
	void down();
	void left();
	void right();
}
```
공통적인 기능이 추상화 되어있는 상태로, 모든 게임은 위 네 함수를 구현하고 있다.

### Game: PacManGame

`GamingConsole`의 구현체

```java
package com.jy.learnspringframework.game;

import org.springframework.stereotype.Component;

public class PacManGame implements GamingConsole {
	public void up() {
		System.out.println("DoubleJump");
	}
	
	public void down() {
		System.out.println("below zero");
	}
	
	public void left() {
		System.out.println("left");
	}
	
	public void right() {
		System.out.println("right");
	}
}
```

### Game: 실행, `GameRunner`

```java
package com.jy.learnspringframework.game;

public class GameRunner {
	 GamingConsole game;
	
	public GameRunner(GamingConsole game) {
		this.game = game;
	}

	public void run() {
		System.out.println("Running game: " + game);
		//System.out.println()보다 로깅 프레임 워크를 사용하는 것이 좋음.
		
		game.up();
		game.down();
		game.left();
		game.right();
		
	}
}
```

Loose bind를 위해 `GamingConsole`을 통해 동작을 실행하는 `GameRunner`

## `@Bean`을 통한 Spring 객체 표식

예시로 살펴보는 `@Bean` 어노테이션의 역할

- `@Bean`은 메서드 수준에서 사용되며, XML의 `<bean/>`요소와 직접적으로 연관이 있다.
- `@Component`나 `@Configuration`으로 주석이 달린 클래스에서 사용할 수 있다.
- 이 어노테이션을 붙이면 Spring이 객체를 관리하게 된다.
    - Application Context에 등록한다.
    - 빈의 이름은 기본적으로 메서드 이름과 동일
- Spring 객체의 생성 방법에는 다음과 같이 3 가지 방법이 있다.

1. 직접 입력: 객체를 직접 생성하여 초기화한다. 이후 객체는 Spring IoC에서 관리한다.

```java
    @Bean
	public Person person() {
		var person = new Person("James", 23, new Address("Main ST.", "korea"));
		return person;
	}
```

2. 메소드 호출

```java
    @Bean
	public Person person2MethodCall() {
		var person = new Person(name(), age(), address());
		return person;
	}
```

3. 파라미터 전달

```java
    @Bean
	public Person person3Parameters(String name, int age, Address address3) { 
	// anme, age, address3
		var person = new Person(name, age, address3);
		return person;
	}
```