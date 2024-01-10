---
title: "자바 I/O - 3"
excerpt: "I/O Stream"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Java I/O
  - Java IO Stream
---

IO Stream: Byte or Char의 흐름

## Input Stream, OutputStream
- 추상 클래스
- byte 단위 입출력 클래스는 InputStream, OutputStream의 후손

## Reader, Writer
- 추상 클래스
- char 단위 입출력 클래스는 Reader, Writer의 후손

## IO Stream Hierarchy

출처 - https://java8.info/inputoutput/javaioovw.html

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216728&authkey=%21AJWOaubhatbuf0s&width=1526&height=792" alt="">
  <figcaption>Byte Input Stream Hierarchy</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216729&authkey=%21AHRfgzhU2UJodnY&width=1556&height=835" alt="">
  <figcaption>Byte Output Stream Hierarchy</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216730&authkey=%21AG2uvfLOdyrmbuY&width=1515&height=814" alt="">
  <figcaption>Char Input Stream Hierarchy</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216731&authkey=%21AGpTwvP7NJZiaq8&width=1511&height=810" alt="">
  <figcaption>Char Output Stream Hierarchy</figcaption>
</figure>

## 부가 설명: InputStream & OutputStream

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216732&authkey=%21AEdULDA67WQxkQ0&width=1184&height=804" alt="">
  <figcaption>How to read: InputStream</figcaption>
</figure>

- InputStream은 바이트 단위로 읽어들인다(read 메서드).
- int(4 바이트) 중 마지막 한 바이트만을 유효 데이터로 사용한다.
- 파일을 read 메서드를 통해 읽어들이게 되면 read 메서드가 사용될 때마다 1바이트 씩 파일에서 읽게 된다.
- byte 배열을 사용하면 사용한 byte 배열에 따라 한번에 전달한다. 배열이 클수록 한 번에 읽는 양이 많아진다.
- 그림 오류: read메서드는 int 값을 넣어주는게 아니라 int 값으로 반환된다.
- InputStream은 추상 클래스이기 때문에 자식 객체를 선언하여 사용해야 한다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216733&authkey=%21AHOvTFgcLJn46zo&width=1221&height=768" alt="">
  <figcaption>How to write: OutputStream</figcaption>
</figure>

- read와 마찬가지로 write로 전달되는 int의 마지막 한 바이트가 써야할 대상에 써진다.
- OutputStream도 추상 클래스이므로 역시 자식 객체를 선언하여 사용해야 한다.

### 예제

```java
import java.io.FileOutputStream;
import java.io.OutputStream;

public class HelloIO01 {
    public static void main(String[] args) throws Exception {
        OutputStream out = new FileOutputStream("/tmp/helloio01.dat");
        out.write(1); // 0000 0000   0000 0000   0000 0000   0000 0001
        out.write(255);
        out.write(0);
        out.close();
    }
}
```
메인 메서드에 예외를 던지도록 둔 것은 편의상이므로 주의

- 실행 결과 3바이트 짜리 helloio01.dat파일이 생성된 것을 확인할 수 있다.
- 바이너리 파일을 열어볼 때에는 hexa editor를 사용하니 한 번 확인해보자: https://hexed.it/

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216734&authkey=%21AFDir_wE2MUw4mY&width=927&height=743" alt="">
  <figcaption>16 진수로 입력된 숫자를 확인할 수 있다: 01, FF, 00</figcaption>
</figure>

```java
import java.io.FileInputStream;
import java.io.InputStream;

public class HelloIO02 {
    public static void main(String[] args) throws Exception {
        InputStream in = new FileInputStream("/tmp/helloio01.dat");

        int i1 = in.read();
        System.out.println(i1); // 1

        int i2 = in.read();
        System.out.println(i2); // 255

        int i3 = in.read();
        System.out.println(i3); // 0

        int i4 = in.read();
        System.out.println(i4); // -1 (파일의 끝)

        in.close();
    }
}
```
메인 메서드에 예외를 던지도록 둔 것은 편의상이므로 주의


이 코드는 다음과 같이 단순화 할 수 있다: 반복문을 통해 파일을 EOF까지 읽기

```java
import java.io.FileInputStream;
import java.io.InputStream;

public class HelloIO02 {
    public static void main(String[] args) throws Exception {
        InputStream in = new FileInputStream("/tmp/helloio01.dat");
        
        int buf = -1;
        while((buf = in.read()) != -1) {
            System.out.println(buf);
        }

        in.close();
    }
}
```
메인 메서드에 예외를 던지도록 둔 것은 편의상이므로 주의

1 바이트 씩 파일의 모든 데이터를 출력하는 프로그램이 된다.

## 부가 설명: Reader & Writer

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216735&authkey=%21AM1cHxxYkGHBrqI&width=1182&height=789" alt="">
  <figcaption>How to read: Reader</figcaption>
</figure>

- 문자 단위(char)로 읽는다(2 바이트).
- 리턴 값인 int의 마지막 두 바이트를 사용하여 데이터를 전달한다.
- 그림 오류: byte[]이 아니라 char[]
- InputStream, OutputStream과 마찬가지로 자식 구체 클래스로 선언해야 한다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216736&authkey=%21ABmY0alUuMy6ipc&width=1126&height=771" alt="">
  <figcaption>How to write: Wirter</figcaption>
</figure>

- 들어온 int의 마지막 2 바이트의 데이터를 써준다.
- 그림 오류: byte[]가 아니라 char[]
- InputStream, OutputStream과 마찬가지로 자식 구체 클래스로 선언해야 한다.

### 예제

```java
import java.io.FileWriter;
import java.io.Writer;

public class HelloIO03 {
    public static void main(String[] args) throws Exception {
        Writer out = new FileWriter("/tmp/hello.txt");
        out.write((int) 'a');
        out.write((int) 'h');
        out.write((int) '!');
        out.close();
    }
}
```
메인 메서드에 예외를 던지도록 둔 것은 편의상이므로 주의

- 1 바이트씩 저장된 것을 확인할 수 있다(jdk 12), 한글은 3 바이트
- [읽을거리](https://jeongdowon.medium.com/unicode%EC%99%80-utf-8-%EA%B0%84%EB%8B%A8%ED%9E%88-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-b6aa3f7edf96)

```java
import java.io.FileReader;
import java.io.Reader;

public class HelloIO04 {
    public static void main(String[] args) throws Exception {
        Reader in = new FileReader("/tmp/hello.txt");

        int ch1 = in.read();
        System.out.println((char) ch1);
        int ch2 = in.read();
        System.out.println((char) ch2);
        int ch3 = in.read();
        System.out.println((char) ch3);
        int ch4 = in.read();
        System.out.println((char) ch4); // -1

        in.close();
    }
}
```

마찬가지로 다음과 같이 간단히 쓸 수 있다.

```java
import java.io.FileReader;
import java.io.Reader;

public class HelloIO04 { 
    public static void main(String[] args) throws Exception {
        Reader in = new FileReader("/tmp/hello.txt");

        int cha = -1;
        while((buf = in.read()) != -1) {
            System.out.println((char) cha);
        }

        in.close();
    }
}
```

## 활용

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216737&authkey=%21ALcg4T2EHFOEZyo&width=1654&height=854" alt="">
  <figcaption>How to read: 활용</figcaption>
</figure>

- byte 단위로 읽는 InputStream은 1 바이트로 데이터를 읽어온다.
- Reader는 char 단위로 읽으므로 2 바이트 단위로 읽어온다: 이 때 InputStream은 2번 호출된다.
- BufferedReader의 readLine()은 한 줄을 읽어오므로 한 줄을 읽기 위해 필요한 수만큼 글자(2 바이트)를 읽어온다.

### 예제

```java
import java.io.PrintWriter;
import java.io.FileOutputStream;
import java.io.OutputStreamReader;

public class HelloIO05 {
    public static void main(String[] args) throws Exception {
        PrintWriter out = new PrintWriter(new OutputStreamWriter(new FileOutputStream("/tmp/my.txt")));

        out.println("Hello");
        out.println("World");
        out.println("!!!");

        out.close();
    }
}
```
- FileOutputStream은 "/tmp/my.txt"에 저장
- FileOutputStream은 wirte(int)를 가지고 있음: int의 마지막 byte만 저장
- OutputStreamWriter생성자에 들어온 OutputStream의 write()를 이용하여 쓴다.
- OutputStreamWriter는 write(int); int 끝의 char를 저장
- PrintWriter는 생성자에 들어온 OutputStreamWriter의 wirte()메서드를 사용하여 쓴다.
- PrintWriter는 println(문자열); 문자열을 출력한다.


파일에서 읽을 때에는 다음과 같이 읽을 수 있다.

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;

public class HelloIO06 {
    public static void main(String[] args) throws Exception {
        BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream("/tmp/my.txt")));

        String line1 = in.readLine();
        String line2 = in.readLine();
        String line3 = in.readLine();
        String line4 = in.readLine();

        System.out.println(line1);
        System.out.println(line2);
        System.out.println(line3);
        System.out.println(line4);

        in.close();
    }
}
```

좀 더 간단히 바꾸면,

```java
public class HelloIO06 {
    public static void main(String[] args) throws Exception {
        BufferedReader in = new BufferedReader(new InputStreamReader(new FileInputStream("/tmp/my.txt")));

        String line = null;
        while((line = in.readLine()) != null) {
            System.out.println(line);
        }

        in.close();
    }
}
```

## 어떤 클래스를 사용해야하지?

- 어떤 부품(클래스)가 있는지 알아야
- 이 부품들을 어떻게 사용하는지 훈련이 필요
- 여러 부품들을 조합하여 사용한다.