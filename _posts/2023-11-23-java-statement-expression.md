---
title: "Expression vs. Statement / java"
excerpt: "Expression과 statement의 차이: 토막글"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
# 표현식 Expression

값이거나 값을 반환하는 코드 조각. 변수, 연산자, 리터럴, 메서드 호출로 구성된다.

```java
int score;
score = 90;
```

에서, score = 90은 int를 반환하는 표현식이다.

또 다른 예로,

```java
// #1
Double a = 2.2, b = 3.4, result;
result = a + b - 3.4;

// # 2
number1 == number2;

// # 3
"Number1 is bigger than Number2";
```

#1. a+b-3.4은 표현식이고

#2. 은 boolean을 반환하는 표현식,

#3. 은 문자열 표현식이다.

 
# 문장 Statement

작업을 수행하도록 설계된 표현식과 문장의 집합(complete unit of execution).

표현식에 세미콜론(;)을 추가함으로써 statement가 될 수 있다.

```java
// expression
number = 10
// statement
number = 10;
```

자바에서 변수의 선언은 declaration statement를 사용하여 선언한다.

 
# 출처

Java Expressions, Statements and Blocks (programiz.com)
