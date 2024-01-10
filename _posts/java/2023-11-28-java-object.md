---
title: "Object 제공 메서드: toString() / equals() & hashCode()"
excerpt: "Object는 왜 이 메서드들을 지원하지?"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
## Object 클래스

Object는 기본적으로 모든 클래스들이 상속하는 클래스. 이 클래스는 오버라이딩하도록 제공되는 메서드들을 포함하고 있다.

예시를 통해 왜 Object가 해당 메서드들을 지원하는지 살펴보자.

## 예시

```java
public class example {
	public static void main(String[] args) {
    	Car car = new Car();
        System.out.println(car); // println(Object x)
	}
}
```
Object를 받아서 출력하는 메서드를 가지고 있다. Object로 참조할 수 있는 모든 매개변수는 모두 이 메서드를 사용할 수 있다.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216649&authkey=%21AOaEM8WVtyEvfms&width=390&height=259" alt="">
  <figcaption>println 메서드</figcaption>
</figure> 

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216650&authkey=%21AL1lWGtZDEDQAMY&width=469&height=165" alt="">
  <figcaption>valueOf 메서드</figcaption>
</figure> 
`println()` 메서드를 살펴보면, `toString()` 메서드가 실행되는 것을 알 수 있다. 다만, Car 클래스에 toString()메서드를 오버라이드하여 작성하지 않으면 쓸모가 없다.

## 오버라이드 메서드의 필요성

- `toString()`: 클래스의 속성 값을 출력하는 메서드입니다. 어떤 속성을 출력해주어야 하는지 결정해야 한다.
- `equals()`: 같은 값인지를 확인하는 메서드입니다. 같은지 판단하는 기준이 필요하다.
- `hashCode()`: 
    해시를 사용할 때에는 다음의 두 가지 기능이 필요합니다: (1) 해시 코드 생성, (2) 두 값이 같은지 판단한다.
    
    `HashSet`, `HashMap` 이라는 자료구조 객체를 사용할 때 
`equals()`와 `hashCode()` 메서드를 오버라이드하여 사용하게 됩니다.
