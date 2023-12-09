---
title: "상속(Inheritance) / Java"
excerpt: "Java에서 사용하는 상속에 대해서 알아보자"

toc: true
toc_sticky: true

categories:
  - Programming
tags:
  - Java
---
# 상속이란?
![inheritance](/assets/images/java-inheritance.png)
출처: https://coding-factory.tistory.com/865

- ~ Is a ... / Kind of ~ 관계 ( ~는 ...다/ ...는 ~ 종류이다.)
- 상속 = 일반화 기능 + 확장 기능
- 상속은 가장 강한 결합도를 가진다. 반드시 써야할 때만 사용하고 가급적 사용하지 않는 것이 좋다. 결합도를 낮추고 응집도를 높여야 활용성이 높아지므로.
- 아무것도 상속하지 않으면 자동으로 java.lang.Obeject를 상속 받는다.

```java
[접근제한자] [ abstract |   final ] class 클래스명 extends 부모 클래스명 {
	...
}
```
 
## Question: 참조 변수가 자식 클래스의 객체를 참조하고 있는 경우 어떤 일이 발생할까?

```java
Car car = new Bus();
```

와 같은 경우는 어떤일이 벌어질까?

객체는 Bus가 생성되지만 그 객체를 가르키는 car변수는 Car로 생성된 것이다. 이때 Bus에서 선언되고 Car에서 정의되지 않은 메서드를 사용할 경우에는 컴파일 오류가 발생한다.

상속관계 클래스 객체의 특정 기능만 사용할 경우에 자주 사용하며, 코드 읽고 이해하는 것을 돕는다.