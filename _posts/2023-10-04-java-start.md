---
title: "자바 기초 / Java"
excerpt: "자바에 대한 기본적인 지식들"

toc: true
toc_sticky: true

categories:
  - Programming
tags:
  - Java
---
# JAVA

- 객체 지향 언어.
- 플랫폼에 독립적이다.
- 느리지만, 버전 업 되면서 다른 언어들의 장점을 흡수하고 있다.(모던 자바)
    - 람다: 함수형 프로그래밍
    - Stream API: 람다 표현식과 메서드 참조 등의 기능과 결합하여 매우 복잡하고 어려운 데이터 처리 작업을 쉽게 조회하고 필터링하고 변환하고 처리할 수 있도록 한다.
    - 병렬 프로그래밍: 여러 개의 CPU 코어에서 작업을 배분해서 동시에 작업을 수행한다.

 
## 클래스
- class 이름은 파일 이름과 동일해야 한다. 첫 글자는 무조건 대문자.
    ```java
    public class Hello{
    ...
    }
    ```
 
## 메서드
- 클래스는 필드(Field)와 메서드(Method)를 가질 수 있다.
- 프로그램 실행이 시작되는 시작점: main 메서드
    ```java
	public static void main(String[] args) {
	...
    }
    ```

## 예시
```java
System.out.println("Hello");
```
- 대문자로 시작하는 System은 클래스이다.
- out은 소문자로 시작하고 인자를 받지 않으므로System의 필드.
- println은 소문자로 시작하고 소괄호로 인자를 받으므로 메서드.

- 검색: java system api -> https://docs.oracle.com/javase/8/docs/api/java/lang/System.html 

>System class에 대해 찾아 볼 수 있다: Field Summary / Method Summery


## 컴파일

- Hello.java (Java 코드)   --컴파일-->   Hello.class (바이트 코드) --JVM(운영체제에 따라 상이, jdk안에 있음)-->  Hello.class실행: 한 줄씩(인터프리터 방식)
- 리눅스에서 다음과 같은 명령어를 사용하면 cat Hello.java -> Hello.java의 내용이 출력
- cat Hello.class -> 바이트코드가 출력된다.

인텔/맥 실리콘의 기계어 명령어 집합이 다르기 때문에 호환의 문제가 있다. Java는 소스코드와 기계어의 중간인 byte코드를 넣어 이를 해결하였다(C언어의 경우 컴파일 하면 기계어로 된 코드가 만들어진다).