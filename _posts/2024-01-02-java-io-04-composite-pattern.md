---
title: "자바 I/O - 4"
excerpt: "Composite pattern으로 다시 보는 Decorator pattern"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Java I/O
  - Composite pattern
---
## Composite pattern

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216738&authkey=%21AKVrs5pbsVOhJyI&width=591&height=526" alt="">
  <figcaption>파일과 폴더의 관계</figcaption>
</figure>

- 파일과 폴더의 관계는 composite pattern으로 표현된다.
- 위의 두가지 방법 모두 파일과 폴더의 관계를 나타내는 방법이다.
- 두번째 도식에서는 공통점을 뽑아내 인터페이스로 표현하였다.
- 인터페이스 대신 추상클래스도 가능하다.
- 폴더는 인터페이스를 가지고 있고, 이는 인터페이스(혹은 추상 클래스)의 (구체 클래스)자식들이 폴더에 포함될 수 있다는 의미이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216739&authkey=%21AII1kVimQStmMaQ&width=1257&height=696" alt="">
  <figcaption>Composite pattern</figcaption>
</figure>
출처: https://medium.com/@achillesmoraites/design-pattern-composite-a77e570e9105

- Java IO의 decorator pattern은 composite pattern과 유사하다.
- Folder는 FileComponent를 가진다.
- FileComponent는 인터페이스나 추상클래스이므로 FileComponent는 인스턴스가 될 수 없다.
- 이를 구현하거나 상속받는 파일이나 폴더가 인스턴스가 될 수 있다. 이 구현하거나 상속받은 파일이나 폴더를 Folder가 가지는 것이다.

### 예제
```java
public abstract class Node {
    private String name;

    public Node(String name) {
        tihs.name = name;
    }

    public String getName() { return name; }

    public void setName(String name) { this.name = name; }

    public abstract long getSize();
    public abstract boolean isFolder();
}
```

```java
public class File extends Node {
    private long size;

    public File(String name, long size) {
        super(name);
        this.size = size;
    }

    @Override
    public long getSize() { return this.size; }

    @Override
    public boolean isFolder() { return false; }
}
```

```java
public class Folder extends Node {
    private List<Node> nodes;

    public Folder(String name) {
        super(name);
        nodes = new ArrayList<>();
    }

    public void add(File file) { nodes.add(file); }

    public void add(Folder folder) { nodes.add(folder); }

    @Override
    public long getSize() {
        long total = 0L;
        for(int i = 0; i < nodes.size(); i++) {
            total = total + nodes.get(i).getSize();
        }
    }
}
```

- Composite Pattern
- 공통 요소인 Node를 추상클래스/인터페이스로 둠으로써 File과 Folder를 Node로 취급하여 관리할 수 있다.

```java
public class CompositePatternDemo {
    public static void main(String[] args) {
        File f1 = new File("file1", 10L);
        File f2 = new File("file2", 20L);
        File f3 = new File("file3", 30L);

        Folder folder1 = new Folder("folder1");
        Folder folder2 = new Folder("folder2");
        folder1.add(f1);
        folder1.add(folder2);
        folder2.add(f2);
        folder2.add(f3);j

        System.out.println(folder1.getSize()); // 60
    }
}
```

## Decorator pattern

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216723&authkey=%21AHaye-X6bb3nrFc&width=1198&height=679" alt="">
  <figcaption>Decorator 패턴</figcaption>
</figure>

- 이제 다시 Java IO의 decorator pattern을 살펴보자.
- Decorator pattern에서 윗부분은 앞서 살펴 본 decorator pattern과 동일하다.

### 예제

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216741&authkey=%21ACw0HSq97U_ZFo0&width=915&height=586" alt="">
  <figcaption>Decorator 패턴</figcaption>
</figure>

```java
public abstract class Node {
    private String name;

    public Node(String name) {
        this.name = name;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public abstract long getSize();
    public abstract boolean isFolder();
}
```

```java
public abstract class Shape {
    public abstract void draw();
}
```

```java
public class Circle extends Shape {
    @Override
    public void draw() { System.out.println("Shape: Circle"); }
}
```

```java
public class Rectangle extends Shape {
    @Override
    public void draw() { System.out.println("Shape: Rectangle"); }
}
```

```java
public class ShpaeDecorator extends Shape {
    protected Shape decoreatedShape; // Shape 를 가질 수 있음

    public ShapeDecorator(Shape decoratedShape) { this.decoratedShape = decoratedShape; }

    public void draw() { decoratedShape.draw(); }
}
```

```java
public class RedShapeDecorator extends ShapeDecorator {
    public RedShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        decoratedShape.draw();
        setRedBorder(decoratedShape);
    }

    private void setRedBorder(Shape decoratedShape) {
        System.out.println("Red ================= Start");
        decoratedShape.draw();
        System.out.println("Red ================= End");
    }
}
```

```java
public class RedShapeDecorator extends ShapeDecorator {
    public RedShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        decoratedShape.draw();
        setGreenBorder(decoratedShape);
    }

    private void setGreenBorder(Shape decoratedShape) {
        System.out.println("Green ***************** Start");
        decoratedShape.draw();
        System.out.println("Green ***************** End");
    }
}
```

```java
public class DecoratorPatternDemo {
    public static void main(String[] args) {
        Circle circle = new Circle();

        RedShapeDecorator redShapeDecorator = new RedShapeDecorator(circle);
        redShapeDecorator.draw();
        
        GreenShapeDecorator greenShapeDecorator = new GreenShapeDecorator(redShapeDecorator);
        greenShapeDecorator.draw();

        Shape shape = new GreenShapeDecorator(new RedShapeDecorator(new Rectangular()));
        shape.draw();

        // Java IO에 적용해보자.
        InputStream in = new DataInputStream(new FileInputStream("a.txt"));
    }
}
```

- Shape => InputStream (추상 클래스)
- Rectangle => FileInputStream
- RedShapeDecorator => DataInputStream
- **재강조**: 주인공과 장식을 구분할 수 있어야 한다.
- 따라서 Java IO API를 읽어보아야 한다.
- 연습을 통해 익숙해진다.

## 예제

### BufferedReader
키보드로부터 한 줄 씩 읽는 프로그램

```java
public class IOExam01 {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = br.readLine();
        System.out.println(line);
        br.close();
    }
}
```

- 키보드로부터 한 줄 씩 입력받는다.
- BufferedReader의 readLine()을 이용하여 한 줄 씩 입력받을 수 있다.
- BufferedReader는 장식 역할: Reader를 인자로 받는다.
- 키보드: System.in: 주인공 역할, InputStream

### DataInputStream, DataOutputStream

- 문제, 이름, 국어/영어/수학 점수를 파일에 저장하시오.
- 파일은 tmp/score.dat

```java
public class IOExam02 {
    public static void main(String[] args) throws Exception {
        String name = "kim";
        int kor = 90;
        int eng = 50;
        int math = 70;
        double total = kor + eng + math;
        double avg = total / 3.0;

        DataOutputStream out = new DataOutputStream(new FileOutputStream("tmp/score.dat"));
        out.writeUTF(name);
        out.writeInt(kor);
        out.writeInt(eng);
        out.writeInt(math);
        out.writeDouble(total);
        out.writeDouble(avg);
        out.close();
    }
}
```

- 파일에서 다양한 정보 읽어오기

```java
public class IOExam03 {
    public static void main(String[] args) throws Exception {
        DataInputStream in = new DataInputStream(new FileInputStream("tmp/score.dat"));
        String name = in.readUTF();
        int kor = in.readInt();
        int eng = in.readInt();
        int math = in.readInt();
        double total = in.readDouble();
        double avg = in.readDouble();
        in.close();

        System.out.println(name);
        System.out.println(kor);
        System.out.println(eng);
        System.out.println(math);
        System.out.println(total);
        System.out.println(avg);
    }
}
```

- 순서대로 읽어온다!
- 메서드 추출을 통해 in 으로 정해진 파일 포멧을 읽어오도록 만들 수 있다.

```java
public class IOExam03 {
    public static void main(String[] args) throws Exception {
        DataInputStream in = new DataInputStream(new FileInputStream("tmp/score.dat"));

        printStudent(in);
        printStudent(in);

        in.close();
    }

    private static void printStudent(DataInpuStream in) throws IOException {
        String name = in.readUTF();
        int kor = in.readInt();
        int eng = in.readInt();
        int math = in.readInt();
        double total = in.readDouble();
        double avg = in.readDouble();

        System.out.println(name);
        System.out.println(kor);
        System.out.println(eng);
        System.out.println(math);
        System.out.println(total);
        System.out.println(avg);
    }
}
```

### ByteArrayInputStream, ByteArrayOutputStream
byte[]에 데이터 읽고 쓰기

```java
public class IOExam04 {
    public static void main(String[] args) throws Exception {
        int data1 = 1;
        int data2 = 2;

        ByteArrayOutputStream out = new ByteArrayOutputStream();
        out.write(data1); // data1의 마지막 한 바이트만 저장한다.
        out.write(data2);
        out.close();

        byte[] array = out.toByteArray();
        System.out.println(array.length);
        System.out.println(array[0]);
        System.out.println(array[1]);
    }
}
```

- ByteArrayOutputStream이 내부적으로 메모리를 가지고 있음.
- write후 close를 하게 되면 ByteArrayOutputStream에 데이터가 차곡차곡 저장된다.
- `toByteArray()`: 저장한 바이트를 가져올 수 있다.

```java
public class IOExam05 {
    public static void main(String[] args) throws Exception {
        byte[] array = new byte[2];
        array[0] = (byte)1;
        array[1] = (byte)2;

        ByteArrayInputStream in = new ByteArrayInputStream(array); // byte array 를 받는다.
        int read1 = in.read(); // 1바이트를 읽어 리턴
        int read2 = in.read();
        int read3 = in.read(); // -1

        in.close();

        System.out.prinlnt(read1);
        System.out.prinlnt(read2);
        System.out.prinlnt(read3);
    }
}
```

- 실제 사용해보기 전까지는 활용하기는 어려울 수 있음.
- 그러나 알아두면 필요할 때 사용할 수 있을 것.

### CharArrayReader, CharArrayWriter
char[]에 데이터 읽고 쓰기

### StringReader, StringWriter

문자열 읽고 쓰기

```java
public class IOExam06 {
    public static void main(String[] args) throws Exception {
    StringWriter out = new StringWriter();
    out.write("hello");
    out.wirte("world");
    out.write("!!!");
    out.close();

    String str = out.toString();
    System.out.println(str);
    }
}
```

앞선 예시처럼 생성자에 아무것도 없는 경우는 메모리에 쓰는 것으로 이해하면 될 것 같다.

```java
public class IOExam07 {
    public static void main(String[] args) throws Exception {
    StringReader in = new StringReader("HelloWorld!!!");

    int ch = -1;
    while((ch = in.read()) != -1) {
        System.out.println((char)ch); // <-
    }
    in.close();

    }
}
```

마지막 두 바이트만 글자일 것이므로 형변환 해주어야 한다.