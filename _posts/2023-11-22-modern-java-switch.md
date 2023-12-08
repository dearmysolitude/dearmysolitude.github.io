---
title: "Switch / java"
excerpt: "모던 자바의 Switch"

toc: true
toc_sticky: true

categories:
  - Programming
tags:
  - Java
  - Modern Java
---
# 들어가기 전에

JDK 14에 들어서면서 Switch Expression이라는 표현이 추가되었다.

Switch Statement는 전통적인 switch 구문으로, 여러가지 경우를 검사하고 지시를 수행하는 동작을 수행한다. 값을 반환하지 않고 주로 프로그램의 흐름을 제어한다.

반면 14에서 추가된 switch expression은 각 case 값을 반환하며, 이 값을 변수에 할당할 수 있다: Expression과 statement의 차이점은 다른 문서에서 살펴보자.

<br>

# JDK 14에서의 변경점들
## 1. Arrow Labels

원래 java의 switch는 case문의 종료를 break를 통해서 진행했어야 했다:

```java
switch (day) {
    case MONDAY:
    case FRIDAY:
    case SUNDAY:
        System.out.println(6);
        break;
    case TUESDAY:
        System.out.println(7);
        break;
    case THURSDAY:
    case SATURDAY:
        System.out.println(8);
        break;
    case WEDNESDAY:
        System.out.println(9);
        break;
}
```

하지만 다음과 같이 -> 를 사용하여 간단하게 표현할 수 있게 되었다.

```java
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}
```
 
## 2. Switch Expression

JDK 14에서는 switch문의 결과를 메서드의 인자로 전달할 수 있다. 따라서 다음과 같은 표현식이 가능하다.

```java
static void howMany(int k) {
    System.out.println(
        switch (k) {
            case  1 -> "one";
            case  2 -> "two";
            default -> "many";
        }
    );
}
```

또한 switch expression은 poly expression이다: 타겟 타입이 알려져있는 경우, 이러한 타입들로 생성된다. 즉 타입이 알려져 있다면, switch expression의 타입은 해당 타겟이 된다. 아니라면, 각 case arm들의 결과 타입을 혼합하여 결과를 산출한 타입을 적용한다.

## 3. Yielding a value

각 switch case에 대해 대부분 하나의  expression만 사용할 것이다. 모든 블럭이 필요할 경우에는, yield statement를 적용할 수 있다.

```java
int j = switch (day) {
    case MONDAY  -> 0;
    case TUESDAY -> 1;
    default      -> {
        int k = day.toString().length();
        int result = f(k);
        yield result;
    }
};
```

리턴과 같은 역할을 하지만, 함수를 종료하지 않고 switch expression에서 해당 case arm의 반환값을 설정한다.

Break statement는 switch statement에서, yield statement는 switch expression에서 사용한다.

 
## 4. Exhuastiveness

Switch expression은 switch statement와 달리 exhuastive 해야한다: 모든 switch case 의 반환값은 해당 switch label에 맞아야 한다. 따라서 default 절이 필요하다는 것을 의미한다. 다만, enum switch의 경우 컴파일러가 알려진 상수값을 compile시에 추가하여 컴파일 타임과 런타임에 다를 수 있다. 사용자가 default 절을 넣는다면 이런 발생할 수 있는 에러가 숨겨질 수 있다.

- Furthermore, a switch expression must either complete normally with a value, or complete abruptly by throwing an exception. This has a number of consequences. First, the compiler checks that for every switch label, if it is matched then a value can be yielded.

```java
int i = switch (day) {
    case MONDAY -> {
        System.out.println("Monday"); 
        // ERROR! Block doesn't contain a yield statement
    }
    default -> 1;
};
i = switch (day) {
    case MONDAY, TUESDAY, WEDNESDAY: 
        yield 0;
    default: 
        System.out.println("Second half of the week");
        // ERROR! Group doesn't contain a yield statement
};

    A further consequence is that the control statements, break, yield, return and continue, cannot jump through a switch expression, such as in the following:

z: 
    for (int i = 0; i < MAX_VALUE; ++i) {
        int k = switch (e) { 
            case 0:  
                yield 1;
            case 1:
                yield 2;
            default: 
                continue z; 
                // ERROR! Illegal jump through a switch expression 
        };
    ...
    }
```
 
## Switch expression 사용시 주의점

>There are switch statements that operate by side-effects, but which are generally still "one action per label". Bringing these into the fold with new-style labels makes the statements more straightforward and less error-prone.
>That the default control flow in a switch statement's block is to fall through, rather than to break out, was an unfortunate choice early in Java's history, and continues to be a matter of significant angst for developers. By addressing this for the switch construct in general, not just for switch expressions, the impact of this choice is reduced.
>By teasing the desired benefits (expression-ness, better control flow, saner scoping) into orthogonal features, switch expressions and switch statements could have more in common. The greater the divergence between switch expressions and switch statements, the more complex the language is to learn, and the more sharp edges there are for developers to cut themselves on.

 <br>

# 참고 자료

[JEP 361: Switch Expressions (openjdk.org)](https://openjdk.org/jeps/361)

[[JDK 14] Switch 문에서 arrow operator 이용하기 (jiniworld.me)](https://blog.jiniworld.me/161#recentComments)

[JDK 17 LTS 정리 - 돈 받고 일하면 프로 (shirohoo.github.io)](https://shirohoo.github.io/backend/java/2022-01-27-jdk17-lts/#switch-expression)