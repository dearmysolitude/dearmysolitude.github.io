---
title: "자바 스레드"
excerpt: "스레드의 개념과 실행 방법"

toc: true
toc_sticky: true

image: https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216775&authkey=%21ALQPk3iXE2NiRpU&width=1280&height=1000
categories:
  - Java
tags:
  - 스레드
  - Thread
  - 암달의 법칙
  - Concurrency
  - Parallel
  - Time Slicing
---
## 성능 향상시키기

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216773&authkey=%21ABX7T9CGEdMebHE&width=363&height=307" alt="">
  <figcaption>Parallelizable한 부분은 한정적이기 때문에 무한정 멀티 스레딩으로 성능을 높일 수는 없다.</figcaption>
</figure>

## 병렬화 시 고려해야할 것들

- 메모리의 속도
- CPU 캐시 메모리
- 디스크
- 네트워크
- 커넥션
- 순차적 실행이 병렬 실행보다 빠른 경우도 있다: 동시 실행에 따르는 오버헤드가 없고, 단일 CPU 알고리즘은 하드웨어 작업에 더 친화적일 수 있기 때문이다.

## 암달의 법칙(Amdahl's Law)

암달의 저주로도 불리며, 컴퓨터 세스템의 일부를 개선할 때 전체적으로 얼마 만큼의 최대 성능 향상이 있는지 계산하는 데 사용된다. 진 암달의 이름을 따왔으며, 이론(theory)이 많은 컴퓨터 과학 분야에서 몇 안되는 법칙(law) 중 하나이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216775&authkey=%21ALQPk3iXE2NiRpU&width=1280&height=1000" alt="">
  <figcaption>암달의 법칙</figcaption>
</figure>

병렬 컴퓨팅을 할 경우, 일부 병렬화 가능한 작업들은 사실상 계산에 참여하는 컴퓨터의 개수에 비례해서 속도가 늘어난다. 이러한 경우 암달의 법칙에 의해서 전체 수행 시간의 개선 효과는 병렬화가 불가능한 작업들의 비중에 크게 영향을 받게 된다. 즉, 아무리 컴퓨터의 개수가 늘어나더라도 위 그림처럼 속도의 한계가 나타난다는 것이다.

## 병렬 vs. 병행

- 병행 Concurrent: 멀티스레드 프로그래밍
- 병렬 Parallel: 멀티코어 프로그래밍

이번에 살펴볼 내용은 병행 프로그래밍이다: 동시성 프로그래밍, 멀티 스레드 프로그래밍

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216776&authkey=%21AHlhD1QuOmf5WtE&width=699&height=453" alt="">
  <figcaption>Concurrent & Parallel</figcaption>
</figure>

윈도우 관리자 / Mac 활성상태 보기
- 각각의 프로세스들은 자신만의 영역을 확보한 채로 실행되고 있는 것을 확인할 수 있다.

## CPU는 어떻게 프로세스를 동시에 실행하나?

### Time slicing

- 단위 시간을 나누어 여러 프로세스가 일정 시간 만큼만 실행된다: slicing이 사람에게 매우 짧기 때문에 동시에 실행되는 것과 같은 경험을 하는 것이다.
- 컨텍스트 스위칭(Context Switching)은 이러한 타임 슬라이싱을 사용하여 여러 프로세스가 실행될 때 한 프로세스가 어느 부분까지 실행했는지 저장하고 실행할 프로세스가 어디부터 실행해야 하는지 찾는 과정을 의미한다: 오버헤드 발생

### 프로세스 Process

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216777&authkey=%21AKWMZmaODKYuOjQ&width=817&height=815" alt="">
  <figcaption>프로세스의 메모리 구조</figcaption>
</figure>

- 각각의 프로세스는 메모리 공간에서 독립적으로 존재한다.
- 각각의 프로세스는 이미지처럼 자신만의 메모리 구조를 가진다.
- 프로세스 A, B, C가 있을 경우 각각 프로세스틑 모두 같은 구조의 메모리 공간을 가진다.
- 독립적인 만큼, 다른 프로세스의 메모리 공간에 접근할 수 없다 → 프로세스 간의 통신: IPC

### IPC

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216778&authkey=%21ABL5um5NZIx8BJw&width=1098&height=775" alt="">
  <figcaption>IPC</figcaption>
</figure>

- 프로세스 A에서 프로세스 B를 직접 접근할 수 없기 때문에 프로세스 간의 통신을 하는 특별한 방식이 필요하다.
- 메일슬롯(mailslot), 파이프(pipe)등이 IPC의 예이다.
- 프로세스는 독립적인 메모리 공간을 지니기 때문에 IPC를 토하지 않고 통신할 수 없다.
- 프로세스가 여럿 병렬적으로 실행되기 위해서는 필연적으로 프로세스 간의 컨텍스트 스위칭이 발생한다. 스레드는 이를 보완하기 위해 만들어진 개념이다.

## 스레드 Thread

<figure style="width: 50%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216779&authkey=%21APGc1ffJC3XptCk&width=560&height=828" alt="">
  <figcaption>스레드 Thread</figcaption>
</figure>

- 하나의 프로그램 내에 존재하는 여러 실행 흐름을 위한 모델이다.
- 우리가 생각하는 프로그램이 실행되기 위해서 하나의 실행 흐름으로 처리할 수도 있지만 다수의 실행 흐름으로 처리할 수도 있다.
- 위 그림과 같이 스레드는 프로세스와 별개가 아닌 프로세스를 구성하고 실행하는 흐름이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216782&authkey=%21AEcgHVuHupnvEX4&width=1251&height=780" alt="">
  <figcaption>스레드 Thread</figcaption>
</figure>

## 스레드는 프로세스와 비교하면...

- 프로세스 안에 존재하는 실행 흐름
- 한 프로세스의 heap, static, code 영역을 공유한다.
- 프로세스의 stack 영역을 제외한 메모리 영역을 공유한다.
- code 영역을 공유하기 때문에 프로세스 내부의 스레드들은 프로세스가 가지고 있는 함수를 모두 호출할 수 있다.
- IPC 없이도 스레드 간의 통신이 가능하다. A와 B 스레드는 통신하기 위해 heap 영역에 메모리 공간을 할당하고, 두 스레드가 자유롭게 접근할 수 있다.
- 프로세스처럼 스케줄링의 대상이다. 이 때 컨텍스트 스위칭이 발생하는데, 스레드는 스레드간 공유하고 있는 메모리 영역 덕분에 그 오버헤드가 프로세스 간 컨텍스트 스위칭에 의해 발생한느 것보다 작다.
  - 동작 중인 프로세스가 바뀔 때에 프로세스는 현재 자신의 상태(context 정보)를 보존한 후, 새롭게 동작하는 프로세스는 이전에 보존해 두었던 자신의 컨텍스트 정보를 다시 복구한다.
  - 스레드의 컨텍스트 정보는 프로세스보다 작아 가볍게 행해진다.
  - 하지만, 실제 스레드와 프로세스 관계는 JVM 구현에 크게 의존한다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216783&authkey=%21AATd5tEBIS_sjRY&width=1528&height=750" alt="">
  <figcaption>멀티 스레드</figcaption>
</figure>

## Java 프로그램에서 스레드 사용하기

### 1. Thread 클래스를 상속받기

<figure style="width: 50%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216784&authkey=%21AEbfjmFM9qM0D8k&width=259&height=712" alt="">
  <figcaption>스레드 클래스</figcaption>
</figure>

```java
class SomeMultiThreadClass extends Thread {
    @Override
    public void run() {
        // 동시 실행 코드
    }
}
```

```java
SomeMultiThreadClass A = new SomeMultiThreadClass();
A.start();
```

#### 예제

```java
public class MyTreadExample01{
    public static void main(String[] args) {
        String name = Thread.currentThread().getName();
        System.out.println("thread name: " + name);
        System.out.println("Start!");

        // 1초마다 *가 10번 출력하는 프로그램
        for(int i = 0; i < 10; i++) {
            System.out.print("*");
            try {
                Thread.sleep(1000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }

        // 1초마다 +가 10번 출력하는 프로그램
        for(int i = 0; i < 10; i++) {
            System.out.print("+");
            try {
                Thread.sleep(1000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("End!");
    }
}
```

이제 두 출력 코드가 동시에 실행되도록 해보자.

```java
// 1. Thread 클래스를 상속받는다.
public class MyThread extends Thread{
    private String str;

    public MyThread(String str) {
        this.str = str;
    }

    // 2. run() 메서드를 오버라이딩 한다.
    // 3. 동시에 실행시키고 싶으 코드 작성
    @Override
    public void run() {
        String name = Thread.currentThread().getName();
        System.out.println("스레드 이름: " + name);
        for(int i = 0; i < 10; i++) {
            System.out.print(str);
            try {
                Thread.sleep(1000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public class MyTreaddExample02{
    public static void main(String[] args) {
        String name = Thread.currentThread().getName();
        System.out.println("thread name: " + name);
        System.out.println("Start!");

        MyThread mt1 = new MyThread("*");
        MyThread mt2 = new MyThread("+");

        // 4. thread는 start() 메서드로 실행
        mt1.start();
        mt2.start();

        System.out.println("End!");
    }
}
```

- thread 클래스의 run() 메서드를 구현 클래스에서 오버라이드
- start() 메서드 실행: thread 클래스의 start()메서드가 실행된다.
    - start() 내부에는 thread를 준비하는 코드와
    - 자기 자신(thread 클래스)이 가지고 있는 run()메서드를 실행한다.
    - run()은 자식(구현 클래스)이 오버라이딩 하고 있으므로 자식의 run()이 실행된다.
- main() 메서드로 main thread가 실행되며(출력되는 스레드 이름이 main인 것을 확인할 수 있다.)
- main thread에서 start 메서드를 만나면 새로운 스레드가 생성되며 (이 코드에서는) mt1 객체의 메서드가 실행된다(가지 치기 하듯이).
- main thread에서는 다음 줄로 넘어가 코드를 실행하여 역시 새로운 스레드로 mt2 객체의 메서드가 실행한다.
- End!가 출력되고 나머지 스레드는 자신의 작업을 수행한다.
- 모든 스레드가 종료되었을 때 프로그램은 종료된다.

### 2. Runnable 인터페이스를 구현하기

<figure style="width: 75%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216785&authkey=%21AABy11HVvv7_HFM&width=996&height=742" alt="">
  <figcaption>Runnable 인터페이스</figcaption>
</figure>

스레드로 만들고자 하는 클래스가 Runnable 인터페이스를 구현할 경우, Thread 클래스를 가지도록 해야한다.

```java
class SomeThreadClass implements Runnable {
    public void run() {
        // 동시 실행할 코드
    }
}
```

```java
SomeThreadClass thread = new SomeThreadClass();
Thread t = new Thread(thread);
t.start();
```

#### 예제

위의 예제를 이번에는 Runnable 인터페이스를 통해 구현해보자.

```java
// 1. Runnable 인터페이스를 구현한다.
public class MyRunnable implements Runnable {
        private String str;

    public MyThread(String str) {
        this.str = str;
    }

// 2. run() 메서드를 구현한다.
    @Override
    public void run() {
        String name = Thread.currentThread().getName();
        System.out.println("스레드 이름: " + name);
        for(int i = 0; i < 10; i++) {
            System.out.print(str);
            try {
                Thread.sleep(1000);
            } catch(InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
public class MyTreaddExample03{
    public static void main(String[] args) {
        String name = Thread.currentThread().getName();
        System.out.println("thread name: " + name);
        System.out.println("Start!");

        MyRunnable mr1 = new MyRunnable("*");
        MyRunnable mr2 = new MyRunnable("+");

        // 3. Thread 인스턴스를 생성하는데, Runnable 인스턴스를 생성자에 넣어준다.
        Thread thread1 = new Thread(mr1);
        Thread thread2 = new Thread(mr2);

        // 4. Thread가 start() 메서드로 실행
        thread1.start();
        thread2.start();

        System.out.println("End!");
    }
}
```