---
title: "데이터형 / java"
excerpt: "자바의 데이터형: 원시타입과 참조타입, 클래스 정보의 등록"

toc: true
toc_sticky: true

categories:
  - Programming
tags:
  - Java
---
# 키워드(keyword)
Java 언어에서 정한 예약어

# 변수명
- 하나 이상의 글자
- 첫번째 글자는 문자, '$', '_'
- 그 이후 글자는 문자, '$', '_', 숫자
- 길이 제한 없음
- 키워드는 변수명으로 사용할 수 없음

 <br> 

# 기본형 타입(Primitive Type)

기본형 타입은 모두 정해진 메모리 크기를 가지고, 주어진 메모리 안에 그 값을 저장한다.

## int

- 정수 하나를 저장하기 위해 메모리에 4 byte가 필요.
- int i = 1;
    - 메모리 4 byte를 i로 칭하고 정수를 저장한다는 뜻이다. 정수 리터럴(literal) 1을 i 라는 메모리 공간에 저장한 것이다. Literal이란 변수에 입력되는 값 자체.
    - 이 4byte메모리에 숫자 1이 2진수로 저장된다.
- short: 2 byte
- long: 8 byte

## float, double
실수

## char: 문자 1개

- 2바이트 자료형(0000~0FFF, 16 진수, 양수만 표현한다.)으로, **유니코드** 문자의 일련 번호를 저장한다.
- C언어의 경우 ASCII 문자를 사용하기 때문에 1 바이트인 것과 대조적이다.
- "A" = 65
- "a" = 97

## boolean

- true/false: 각각 boolean자료형을 나타내는 예약어이다.

# 참조 타입(Reference Type):
Class, Interface, ...

- 대문자로 시작.
- 값을 가지지 않고, 값을 참조한다.
- *기본형 타입을 제외한 모든 타입.* 참조형 타입은 값이 저장되어 있는 곳의 주소값을 저장하는 공간으로, 스택 메모리에 저장된다. 참고로, 생성되는 객체는 힙 메모리에 생성된다. 자바에서는 C와 달리 메모리 주소를 노출시키지 않아(포인터 연산자가 없음) 어떤 방식으로 데이터만 전달 되는지만 알고 있으면 될 것이다.
- 이렇게 참조형으로 데이터를 주고받다보니, 가르키는 곳에 빈 객체가 있다는 것을 나타내는 Null 개념이 생기게 되었다.

## Null

무언가가 존재하지 않는 상태를 나타내기위해 만들어졌다. 참조형 자료를 초기화할때 사용하는 것이지, 기본형 자료에는 사용할 수 없다. 대소문자를 구분하며, 소문자로 작성했을 때에만 정상 작동한다.

primitive 자료형이 기본 값을 가지고 있는 것처럼(int는 0, boolean 은 false), 참조형 자료의 기본 값이다.

## 참조 데이터의 원본? 클래스 정보의 등록
![metaspace](/assets/images/java-metaspace.png)
Java 8부터 적용된 메타스페이스(PERM → Metaspace)

- Java Heap은 JVM이 관리하는 메모리, native memory는 운영체제에서 관리하는 메모리이다.
- 이 메모리에는 처음 프로그램이 실행될 때 클래스 정보들이 올라가게 된다.

## 소스 코드/클래스 파일은 정적이다.

HDD나 SSD의 경우 RAM보다 속도가 느리기때문에 매번 보조기억장치에서 이를 읽어들이는 것은 성능을 저하시킨다. 따라서 클래스 정보는 처음 사용될 때 메모리에 그 정보를 올리고 필요할 때마다 가져오는 것이다.

→ 이미 클래스에 대한 정보를 PERM 혹은 Metaspace에 올려두고, 객체가 생성될 때마다 사용하는 것이다.

![class-to-object](/assets/images/java-class-to-object.png)
Java에서 메타스페이스를 사용하여 클래스를 관리하고 여기에서 객체가 생성되는 방법

## Static 정보의 생성

Java 7까지는 non-heap영역, 8 이상부터는 heap에 저장한다.

클래스가 로딩될 때 한 번 메모리에 올라가 초기화된다.

 
## 객체의 생성

- new 연산자를 사용할 때마다 이미 메모리에 올라가 있는 클래스 정보를 이용해 Java heap에 인스턴스를 생성한다.
- 이후 참조 변수를  사용해 계속해서 이를 참조하는 식으로 데이터를 다루게된다: 참조 변수는 Java의 stack메모리에 생성된다.
- 인스턴스가 더이상 참조되지 않을때, GC가 메모리를 관리한다.
 
 <br>

# 참고 자료
[위키피디아, 유니코드](https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C_0000~0FFF)

[Java에서의 null 예약어와 reference에 대한 9가지 사실](https://javarevisited.blogspot.com/2014/12/9-things-about-null-in-java.html#%2EVIhq14n-F90%2Elinkedin)

[번역본: 고민덩어리님 번역글](https://m.blog.naver.com/lestat85/220217676199)

[추가: Garbage Collection Algorithm and JVM Memory Management](https://www.programmersought.com/article/4905216600/)