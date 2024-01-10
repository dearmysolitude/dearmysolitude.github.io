---
title: "자바의 예외처리"
excerpt: "자바의 예외와 예외처리에 대해 알아보자"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - 예외
  - 예외 처리
---
## Error
- 컴파일 에러: 컴파일 시 발생하는 에러
- 런타임 에러: 실행 시 발생하는 에러

## 자바에는 2가지 형태의 오류가 발생
- Error: 수습할 수 없는 심각한 오류
- Exception(예외): 예외 처리를 통해 수습할 수 있는 덜 심각한 오류
- 메모리 부족, 스택 오버플로우(stack overflow)등이 발생하여 프로그램이 죽는 것은 프로그래머가 제어할 수 없다.

### 예제

```java
public class ExecptionExample{
    public static void main(String[] args) {
        ExceptionObj1 exobj =  new ExceptionObj1();
        int value = exobj.divide(10, 0);
        System.out.println(value);
    }
}

class ExceptionObj1 {
    public int divide(int i, int j) {
        int value = 0;
        try {
            value = i / j;
        } catch (ArithmaticException e) {
            System.out.println("0으로 나눌 수 없어요.");
            System.out.println(e.toString());
        }
        return value;
    }
}
```

이 코드는 두 가지 문제점을 가지고 있다.
- 예외가 발생해도 프로그램이 죽지 않고 0을 리턴하여 오작동을 일으킬 수 있다.
- 사용자가 원하지 않는 메세지를 출력하고 있다.

## 예외 떠넘기기(throw)

예외가 발생하는 메서드에서 처리하지 않고 메서드를 호출하는 곳에서 처리하도록 예외를 떠넘길 수 있다.

```java
public class ExecptionExample{
    public static void main(String[] args) {
        ExceptionObj2 exobj =  new ExceptionObj2();
        try {
            int value = exobj.divide(10, 0);
        } catch (ArithmeticException ae){
            System.out.println("0으로 나눌 수 없어요.");
        }
        System.out.println(value);
    }
}
```
```java
public class ExceptionObj2 {
    /**
    * i를 k로 나눈 나머지를 반환한다.
    * @param int
    * @param int
    * @return 
    * @throws ArithmeticException
    */
    public int divide(int i, int j) throws ArithmeticException {
        int value = 0;
        value = i / j;
        return value;
    }
}
```

이전보다 나은 코드를 작성하게 되었음
- value값이 출력되는 기능이 사용되지 않았다.
- 사용하는 클래스에서 예외를 처리를 하였다.

## Runtime Exception과 Checked Exception

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216716&authkey=%21AGsx0Z2Q4mLDzWU&width=610&height=507" alt="">
  <figcaption>Runtime Exception의 예</figcaption>
</figure>

- RuntimeException을 상속받은 모든 예외는 Runtime Exception이다.
- 컴파일 시에는 예외가 발생하지 않아 IDE가 잡지 않는다.
- 런타임 시에 발생하는 예외

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216717&authkey=%21AHuoKag6JEYFGfg&width=466&height=454" alt="">
  <figcaption>Checked Exception의 예</figcaption>
</figure>

- RuntimeException을 상속받지 않고 Exception을 상속받은 모든 예외를 Checked Exception이라고 한다.
- ✨✨IntelliJ의 상속관계 다이어그램을 보여주는 기능을 사용해보자.

## 예외 만들때에는
- 되도록 Runtime Exception을 상속받는 예외를 만들자
- Checked Exception이 많아지면 처리가 많아지고 클래스를 사용하는 사용자에게 강요하게 되는 코드가 많아진다.

## 다중 Exception 처리
```java
public class Exception {
    public static void main(String[] args) {
        int[] array = {4, 0};
        int[] value = null;
        try {
            value[0] = array[0] / array[1];
        } catch (ArrayIndexOutOfBoundsException aioobe) {
            System.out.println(aioobe.toString());
        } catch (ArithmeticException ae) {
            System.out.println(ae.toString());
        } catch (Exeception e) {
            System.out.println(e)
        }
    }
}
```

→ ArithmeticException 발생한다.

**Case 1: array를 적절하게 수정해보자.**

`array`의 초기화를 {3, 1}로 바꾸어도 예외가 발생한다: NullPointerException, `value`가 가르키는 것이 null을 참조하므로, Exception을 잡는 catch문에서 예외가 발생하는 것

**Case 2: value를 제대로된 배열을 참조하도록 수정하자.**
- `int[] value = null;`을 `int[] value = new int[1];`로 수정하면 예외가 발생하지 않을 것
- `int[0]' 으로 하면 원소가 0개 짜리 배열이 생성된다 → NullPointerException

**Case 3: `int[] array = {4};`로 수정해보자.**

`array`의 참조 범위를 벗어났으므로 ArrayIndexOutOfBoundsException이 발생한다.

**왜 예외를 구분해서 다중 예외처리를 하지?**

발생하는 예외별로 처리를 개별적으로 하기 위함.

## 사용자 정의 Exception
```java
public class MyException extends RuntimeException {
    public MyException(String msg) {
        super(msg); //super에 메세지 넘겨줌
    }

    //또다른 Exception이나 RuntimeException을 받을 수 있음, 그냥 Exception을 받아도 된다.
    public MyException(Throwable cause) {
        super(cause); //super에 Throwable 넘겨줌
    }
}
```

- 주로 오류 메세지를 설정하거나 발생한 Exception을 감싸려고 할 때 만들게 된다.
- 통상적으로 메세지와 Throwable을 받는 생성자를 사용한다.

이렇게 만든 예외는 다음과 같이 처리할 수 있다.
```java
public class ExecptionExample3{
    public static void main(String[] args) {
        ExceptionObj2 exobj =  new ExceptionObj2();
        try {
            int value = exobj.divide(10, 0);
            System.out.println(value);
        } catch (MyException me){
            System.out.println("사용자 정의 Exception이 발생해요.");
        }
    }
}

class ExceptionObj3 {
    public int divide(int i, int j) throws MyException {
        int value = 0;
        try {
            value = i / j;
        } catch (ArithmaticException e) {
            throw new MyException("0으로 나눌 수 없어요.");
        }
        return value;
    }
}
```

왜 이런 사용자정의 Exception을 사용할까?
- 생성한 클래스 내부에서 다양한 예외가 발생할 것이다.
- 이 때 내부에서 발생한 다양한 예외들을 catch하여 사용자 정의 예외를 발생시키는 것이다.
- 그리고 내부에서 예외를 처리하지 않고 throws를 통해 사용자 정의 예외를 호출 메서드에 전달한다.
- 메서드를 사용하는 쪽에서는 **다양한 예외를 간단한 코드로 처리할 수 있다**: `catch (MyException me)`