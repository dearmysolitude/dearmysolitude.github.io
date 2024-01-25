---
title: "Java의 String 클래스"
excerpt: "Java의 String 클래스의 특성을 살펴본다."

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - String
---
```java
public class StringExam{
	public static void main(String[] args) {
 		String str1 = "hello";
		String str2 = "hello";
		String str3 = new String("hello");
    }
}
```
new로 생성하지 않은 경우 상수 취급된다. 따라서 str1과 str2는 같은 것을 참조한다. new를 사용한 str3는 힙 메모리에 항상 새로운 객체를 생성한다. Reference 타입에서 ==는 같은 객체를 참조하는지 확인하는 연산자이다. 그래서 str1, str2, str3 를 비교하게되면 ==로 비교했을 때 1, 2는 참이 나오지만 1, 3을 비교하면 거짓이 나온다. 만약 new로 새로운 hello를 만들어 str3과 비교해도 거짓이다.

## 자바의 String 객체 생성
자바에서 String 객체는 두가지 방법으로 생성할 수 있다. 앞서 보았듯 리터럴을 이용한 방법과 new 연산자를 이용하는 방법이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216690&authkey=%21AJFvfgF4u38ng1k&width=1280&height=720" alt="">
  <figcaption>String constant pool</figcaption>
</figure> 

리터럴을 이용한 방법: 객체는 String Constant Pool이라는 공간에 저장된다. 이는 Heap영역에서 String 객체들을 관리하는 별도의 공간이다.
new 연산자를 이용한 방법: 객체는 Heap 영역에 생성된다.
 
## String Constant Pool
동일한 문자열 리터럴에 대해 메모리를 효율적으로 사용할 수 있게 해준다. 이는 String이 불변(Immutable)하기 때문에 가능한 것으로, 한 번 생성된 String 객체는 변경할 수 없어 동일한 문자열에 대해서는 하나의 String 객체만을 유지하며 메모리를 절약할 수 있는 것이다.

- 문자열 리터럴 방식으로 String을 초기화 할 경우:
    - String Constant pool에 해당 문자열이 있는지 존재 여부를 체크하고 있는 경우 리터럴의 주소를 할당하고
없는 경우에는 해당 문자열의 값을 가지는 객체를 string constant pool에 생성한다.
    - 이 과정을 실행하는 메서드가 String 클래스 내부의 intern()이다. 이 메서드는 string constant pool의 생성된/이미 존재하는 String 인스턴스의 주소값을 리턴한다.
- intern()의 코드는 다음과 같다:

```java
public class Intern {

    public static void main(String[] args) {
        String literal = "brido";
        String object = new String("brido");
        String intern = object.intern();

        System.out.println(literal == object); // false
        System.out.println(literal.equals(object)); // true
        System.out.println(literal == intern); // true
		/* 
			아래 라인에서 true가 리턴되는 이유는 intern()은 "brido"를 
			String Constant Pool에서 검색하고, 존재하는 리터럴이기 때문에
			해당 주솟값을 리턴하기 때문이다.
		*/
        System.out.println(literal.equals(intern)); // true
    }

}
```
string constant pool은 많은 객체를 효율적으로 관리하기 위한 플라이웨이트 패턴을 잘 적용한 대표적인 예이다.

## String 클래스는 왜 불변인가?
자바가 String을 불변으로 처리하여 얻은 이점을 생각해보면 쉽게 파악할 수 있다.

1. String constant pool에서의 설명에서와 마찬가지로, String이 불변이면 변하지 않는 같은 객체를 참조하도록 하여 메모리 공간을 아낄 수 있기 때문이다.
2. 불변이 아니라면 보안상의 문제가 발생할 수 있다: 다음의 정보들은 String으로 관리하므로
    - DB의 username과 password
    - 소켓 통신에서 host과 port정보 등등
3. 멀티 스레드 환경에서 안전(thread-safe)하다: 값의 변경 가능성이 없으므로 동기화 문제에서 자유롭다.
4. Java는 hashcode를 생성 단계에서부터 캐싱한다:
    - String의 hashcode는 매번 계산되지 않는다.
    - 이미 캐싱되어 있기 때문에 다른 객체를 Key로 했을 때보다 더 빠른 속도로 사용할 수 있다.
    - Java에서 String의 hashcode가 캐싱될 수 있는 이유가 String이 불변이기 때문이다.
 
## 가변 String: StringBuffer / StringBuilder
StringBuilder 클래스/StringBuffer 클래스 모두 문자열에 대한 연산을 효과적으로 수행하도록 하는 가변 클래스이다. 이들을 통해 새로운 문자열 객체를 생성하지 않고 문자열 연산을 수행할 수 있다.

차이점이라면, StringBuffer의 경우에는 동기화가 되어있으므로 스레드 안전(thread-safe)이다. 반면에 StringBuilder의 경우에는 동기화 되어있지 않아 스레드 안전하지 않지만 StringBuffer보다 빠르다. 따라서 단일 스레드에서 사용할 경우 StringBuilder를, 멀티 스레드에서 사용할 경우 StringBuffer를 사용한다.

>A mutable(가변적) sequence of characters. [StringBuilder]
>
>A thread-safe(스레드 안전한), mutable(가변적) sequence of characters. [StringBuffer]
>
>-Oracle Javadocs-

Mutable sequence라는 것은, 문자열이 변경될 때마다 새로운 메모리를 할당받는 것이 아니라, 버퍼를 통해 문자열을 관리하고 있다가 toString()을 통해 String객체를 생성하는 것이다.
따라서 문자열 끝에 문자를 추가하는 append()메서드와 원하는 위치에 문자을 삽입할 수 있는 insert()메서드를 제공한다. 생성자는 비어있을수도, 문자열을 넣을수도, 초기 용량(capacity)를 설정할 수도 있다.
String대신 StringBuffer나 StringBuilder를 사용하는 것이 유리한 경우
반복문을 통해 많은 양의 문자열 변경이 일어나야 하는 경우에는 StringBuilder/StringBuffer를 사용하시는 것이 더 좋다.

```java
public class Main {

    private static final int MAX_COMPARE = 100000;

    public static void main(String[] args) {

        // String Test
        String test1 = "";
        long start = System.currentTimeMillis();
        for(int i = 0; i < MAX_COMPARE; i++) {
            test1 += "a";
        }

        long end = System.currentTimeMillis();
        System.out.println("String : " + (end - start) + " ms");

        // StringBuilder Test
        StringBuilder sb = new StringBuilder();
        start = System.currentTimeMillis();
        for(int i = 0; i < MAX_COMPARE; i++) {
            sb.append("a");
        }
        end = System.currentTimeMillis();
        System.out.println("StringBuilder : " + (end - start) + " ms");
    }
}
```
~> 작동시켜 보자... / 출처: https://readystory.tistory.com/141
 
## 그러나 Java 8버전 이후에는...
자바 8버전 이후부터 String 의 + 연산자를 통한 Concatenation에 대해 자바 컴파일러가 자동으로 StringBuilder로 변환해주고 있습니다. 따라서 자바 8 이상을 사용하시는 분이라면 일반적인 상황에서 StringBuilder를 고려하지 않으셔도 됩니다만, 반복문 내에서 이루어지는 Concatenation에 대해서는 StringBuilder로 변환해주지 않기 때문에 명시적으로 StringBuilder를 사용하시는 것이 좋습니다.

## String 클래스의 메서드, `split()`: JVM의 관점에서

```java
String board = "번호,제목,내용,글쓴이";
String[] arr = board.split(",");
```

arr를 가르키는 지역 변수(아마도 대부분의 경우)는 스택에 위치하고(만약 전역이나 static이라면 클래스 정보와 함께 메서드 영역에 있을 것이다.), arr는 힙에 생성되며, arr에 있는 String객체는 JVM이 관리하는 문자열이 저장된 메모리 구역에 위치한다.

## 참고 자료
[Difference between String buffer and String builder in Java (tutorialspoint.com)](https://www.tutorialspoint.com/difference-between-string-buffer-and-string-builder-in-java)

[<Java> String 클래스 초기화와 String Constant Pool (hongchangsub.com)](https://hongchangsub.com/java2/)

[https://readystory.tistory.com/139](https://readystory.tistory.com/139)