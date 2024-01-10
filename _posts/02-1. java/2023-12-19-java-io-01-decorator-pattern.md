---
title: "자바 I/O 01"
excerpt: "자바의 IO에 대해 알아보자"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Java I/O
  - Design Pattern
  - 디자인 패턴
  - Decorator Pattern
  - 데코레이터 패턴
---
- Input & Output, 즉 입출력
- 입력은 키보드, 네트워크, 파일 등으로부터 받을 수 있다.
- 출력은 화면, 네트워크, 파일 등에 할 수 있다.

## Java IO도 객체다.
- Java IO에서 사용되는 객체는 자바 객체이다.
- Java IO에서 제공하는 객체는 어떤 대상으로부터 읽어들여 어떤 대상에게 쓰는 일을 한다.

## Decorator 패턴
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216723&authkey=%21AHaye-X6bb3nrFc&width=1198&height=679" alt="">
  <figcaption>Decorator 패턴</figcaption>
</figure>
Decorator 패턴: 장식 패턴, 장식할 대상이 있다.

- 장식할 대상(주인공): ConcreteComponent
- 장식: Decorator
- 마름모 화살표: Decorator는 Component를 가질 수 있다.
- Decorator의 필드에 Component를 포함한다는 의미임: 생성자에 초기화가 포함됨
- Component를 상속받는 것들도 가질 수 있다.

## Java IO는 여러 부분을 조립하여 사용한다.

[자바 API: java.io 패키지](https://docs.oracle.com/javase/8/docs/api/java/io/package-summary.html)

- Component(장식 대상, 주인공): InputStream, OutputStream, Reader, Writer
    - 어떤 대상에게서 읽어들일지, 쓸지를 결정한다.
    - 1byte or byte[] 단위로 읽고 쓰는 메서드를 가진다.
    - 1char or char[] 단위로 읽고 쓰는 메서드를 가진다.
- Decorator(장식)
    - 장식은 InputStream, OutputStream, Reader, Writer(Component = 장식 대상, 주인공)를 생성자에서 인자로 받는다
        - 이들이 Component이므로 Decorator 생성자 클래스에서 인자로 받는다.
        - 추상 클래스이므로 new로 객체 생성할 수 없음
    - 장식은 다양한 방식으로 읽고 쓰는 메서드를 가진다.
- Java IO에서 무엇이 component고 무엇이 decorator인지 구분할 수 있어야 한다.

## Java IO의 특수한 개체
- System.in: 표준 입력(InputStream), 키보드로부터 입력
- System.out: 표준 출력(PrintStream), 화면에 출력
- System.err: 표준 에러 출력(PrintStream), 화면에 에러 출력

## Java IO 클래스 상속도
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216724&authkey=%21AITJP0bI-qm9YB4&width=1105&height=577" alt="">
  <figcaption>Java IO 클래스 상속도</figcaption>
</figure>

- 사실 UML 상에서 상속을 나타내기 위해서 상속을 하는 Object 방향에 삼각형 화살표 가 있어야 한다.

| 클래스 이름 | 설명 |
| :-------: | :-----: |
|Stream으로 끝나는 클래스 | byte 단위 입출력 클래스 |
| InputStream으로 끝나는 클래스 | byte 단위로 입력을 받는 클래스 |
| OutputStream으로 끝나는 클래스 | byte 단위로 출력을 하는 클래스 |
| Reader로 끝나는 클래스 | 문자 단위로 입력을 받는 클래스 |
| Writer로 끝나는 클래스 | 문자 단위로 출력을 하는 클래스 |
| File로 시작할 경우(File 클래스 제외) | File로부터 입력이나 출력을 하는 클래스 |
| ByteArray로 시작할 경우 | 입력 클래스의 경우 byte 배열로부터 읽어들이고, 출력 클래스의 경우 클래스의 내부의 자료구조에 출력을 한 후 출력된 결과를 byte 배열로 반환하는 기능을 가진다. |
| CharArray로 시작할 경우 | 입력 클래스의 경우  char 배열로부터 읽어들이고, 출력 클래스의 경우 클래스 내부의 자료구조에 출력을 한 후 출력된 결과를 char 배열로 반환하는 기능을 가진다. |
| Filter로 시작할 경우 | Filter로 시작하는 입출력 클래스는 직접 사용하는 것보다 상속받아 사용하며, 사용자가 원하는 내용만 필터링할 목적으로 사용된다. |
| Data로 시작할 경우 | 다양한 데이터형을 입출력할 목적으로 사용한다. 특히 기본형 값(int, float, double 등)을 출력하는데 유리하다. |
| Buffered로 시작할 경우 | 프로그램에서 Buffer라는 말은 메모리를 의미한다. 입출력 시에 병목현상을 줄이고 싶을 경우 사용한다. |
| RandomAccessFile | 입력이나 출력을 모두 할 수 있는 클래스로서, 파일에서 임의의 위치의 내용을 읽거나 쓸 수 있는 기능을 제공한다. |

## 생성자/상속관계가 중요하다.
- 생성자의 인자로 무엇을 받는지가 component와 decorator를 구분하는 방법임.
- [Class ByteArrayInputStream](https://docs.oracle.com/javase/8/docs/api/java/io/ByteArrayInputStream.html)
    - InputStream을 상속받고 있음
    - 생성자가 인자로 InputStream, OutputStream, Reader, Writer를 받지 않음: 이 클래스는 주인공임
    - byte[] 로부터 읽어들이기 위한 클래스
- [Clas BufferedInputStream](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedInputStream.html)
    - InputStream을 상속받고 있음
    - 생상자 인자가 InputStream임: 이 클래스는 장식임
- [Class File](https://docs.oracle.com/javase/8/docs/api/java/io/File.html)
    - Decorator도 아니고, component도 아님
- [Class PipedInputStream](https://docs.oracle.com/javase/8/docs/api/java/io/PipedInputStream.html)
    - byte[]를 받으므로 Component 임
    - 하지만 생성자 중 하나에서 인자로 받는 [PipedOutputStream](https://docs.oracle.com/javase/8/docs/api/java/io/PipedOutputStream.html)을 들어가보면 이 클래스는 OutputStream을 상속받고 있다. 그래서 이 클래스는 Decorator로 보기도 함.


## 예제
**Java IO를 잘 다루기 위해서는 API를 꼭 읽어봐야 한다.✨✨✨**

키보드로부터 한 줄씩 입력받아 화면에 한 줄씩 출력하시오

이 문제를 해결하려면 어떻게 해야할까?

```java
public class KeyboardIOExample {
    public static void main(String[] args) throws Exception {
        // 모든 예외는 편의를 위해 그냥 JVM이 처리하도록 던져버림

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in)); 
        // 3. 생성자가 알맞게 초기화하도록 완성한다.

        String line = null;
        while((line = br.readLine()) != null) {
            // 2. readLine 메서드는 더이상 입력이 없다면 null을 반환한다.
            // 프로그램 실행 시 cmd + d/ctrl + d를 눌러 null 값이 들어가게 할 수 있음(키보드로부터 EOF, end of file 정보, 즉 더이상 읽을 것이 없다는 것을 전달함)
            System.out.println("읽어들인 값: " + line);
        }
    }
}
```

**1. 문제 분석**
- 키보드: System.in (InputStream, 주인공: component, 어디에서 읽을 것인지)
- 화면 출력: System.out (PrintStream, 주인공: component, 어디에서 쓸 것인지)
- 키보드로 입력받는다면 문자를 입력받는다: char 단위 입출력
- char 단위 입출력 클래스는 Reader, Writer

**2. 필요한 메서드 찾기**
- 한 줄 읽기: BufferedReader클래스는 readLine이라는 메서드를 가지고 있더라
    - 더이상 읽을 것이 없다면 null을 반환: While문을 추가하였다.
    - decorator 이다
- 한 줄 쓰기: PrintStream, PrintWriter

**3. 생성자 완성하기**
- `BufferedReader()`는 빈 생성자가 없음: Reader형 자료를 인자로 받는다(decorator).
    - Reader는 추상 클래스이므로 이 클래스의 자식 클래스를 인자로 받게 해야 한다.
    - 무엇으로부터 입력받는지 결정하는 것이다: 키보드로부터 입력받는 클래스를 찾아보자
    - [Class Reader](https://docs.oracle.com/javase/8/docs/api/java/io/Reader.html)에서 보듯이, direct know subclass들 중 하나가 들어가야 할 것.
        - `BufferedReader`: 사용 클래스이므로 이 클래스는 아닐 것
        - `CharArrayReader`: char[]를 받는 component로, 문자로부터 입력받는다.
        - `FilterReader`: 또 Reader를 받아야
        - `InputStreamReader`: InputStream을 받는 decorator 클래스, InputStream은 키보드로부터 받는 입력이므로 이 클래스를 사용할 수 있다.

## (간단히) 무슨일이 일어나나?

[참고 자료: Reading data From keyboard, Java T point](https://www.javatpoint.com/Input-from-keyboard-by-InputStreamReader)

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216725&authkey=%21AK9dA_DsfPTo-N4&width=593&height=258" alt="">
  <figcaption>Java IO의 키보드 입력</figcaption>
</figure>

- Device buffer는 운영체제가 관리한다: 키보드로 콘솔에 입력하여 엔터를 치지 않은 상태에 여기에 저장되어 있음
- Device buffer를 System.in이 받아들인다.
- 버퍼에 있는 값을 BufferedReader가 한 줄씩 입력받아 자바 프로그램에 전달해 준다.
- 좀 더 내부적인 작동은 다음 글에.