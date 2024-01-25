---
title: JVM
excerpt: JVM의 구조에 대해 살펴보자.

toc: true
toc_sticky: true

categories:
    - Java
tags:
    - JVM
---

들어가기 전에

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/da54f7d4-7f02-4fce-bb56-75fdf77862db/4f5e7666-bb96-4e7c-a379-3f355fa5702f/Untitled.png)

- static이나 클래스 코드(바이트코드 파일을 읽은 내용)는 **메서드 영역**
    - 멤버 변수, 즉 필드는 여기에 포함 안된다: 힙 영역에 객체에 존재.
- `new` 로 만들어진 객체는 모두 **heap영역**: 멤버 변수
    - 객체의 주소는 메서드 영역의 상수/스택 영역의 변수에서 참조한다.
- 메서드 호출 시마다 생성되는 프레임은 **stack 영역**
    - 각 스레드는 메서드 실행시 생성됨. → ? 이건 좀 확실히 해두는 게
    - 메서드에서 객체 참조 변수를 선언하면: 힙 영역에 생성되어 있는 객체를 참조한다.
- 메서드의 코드는 하나만 있으면 작동할 수 있으므로 클래스가 있는 메서드 영역에서 공유된다.
- 스레드: stack(1Mb) / register로 이루어짐, 실행 코드는 스택 메서드 영역에 있는 코드(클래스 정보)를 찾아가 실행한다.
- GC(스레드로 동작한다!)는 heap 영역을 관리한다.

**1. 메모리**

프로그램을 실행하기 위한 데이터 및 명령어를 저장하는 공간

※ 메모리구조를 공부하는 이유

- 같은 기능의 프로그램이더라도 메모리 관리에 따라 성능이 좌우됨.
- 메모리 관리가 되지 않은 경우 속도저하 현상이나 튕김 현상 등이 일어날 수 있음.
- 한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위함.

**2. 자바 프로그램의 실행구조**

프로그램이 실행되기 위해서는 windows나 linux같은 운영체제(OS)가 제어하고 있는 시스템의 리소스의 일부인 메모리(RAM : 주기억장치)를 제어할수 있어야 하는데, java이전의 c같은 대부분의 언어로 만들어진 프로그램은 이러한 이유때문에 OS에 종속되어 실행되게 되어 있었다.

java프로그램은 JVM(Java Virtual Machine : 자바가상머신)이라는 프로그램만 있으면 실행이 가능한데, JVM이 OS에게서 메모리 사용권한을 할당받고 JVM이 자바프로그램을 호출하여 실행하게 된다. OS에서는 독립되었지만 JVM이라는 프로그램에 종속적이게 된다. (JVM을 실행시키고 다시 JVM이 프로그램을 실행시키는 방식이다 보니 OS에 직접 제어받는 방식보다는 속도면에서는 느리다는 단점을 가진다)

https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb3.phinf.naver.net%2F20130524_83%2Fzeroday7_1369363867407vYoPA_PNG%2Fjvm_exe.PNG%3Ftype%3Dw740%22&type=cafe_wa740

- **JVM이란?**
- Java Virtual Machine
- JAVA와 OS 사이에서 중계자 역할
- JAVA가 OS에 구애받지 않고 재사용을 가능하게 해 줌
- 메모리 관리 기능(Garbage Collection)

**※ JVM(java.exe)은 무엇을 하는가?**

① 메모리를 할당한다.

② bytecode를 interpreter 형태로 OS에 맞추어 번역, 실행한다.

③ 번역, 실행 시 최적화를 수행한다.

**3. 자바프로그램 실행 과정과 JVM메모리 구조**

프로그램이 실행되면, JVM은 OS으로부터 이 프로그램이 필요로 하는 메모리를 할당받고, JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.

JVM은 크게 3부분으로 나눌 수 있다. 클래스 파일을 로딩한 뒤 검증하고 초기화하는 Class loader subSystem, 클래스 파일을 저장하는 Runtime DataArea(이곳은 다시 method area, heap, java stacks, pc registers, native method stacks의 5가지 영역으로 나누어진다), 클래스 파일(바이트코드)를 플랫폼에 맞는 기계어로 변환시켜 실행하는 ****Execution engine 이다.

https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb3.phinf.naver.net%2F20140804_243%2Fzeroday7_1407133845106Jt7t8_PNG%2F%25BD%25BA%25C5%25A9%25B8%25B0%25BC%25A6_2014-08-04_%25BF%25C0%25C8%25C4_3.28.33.png%3Ftype%3Dw740%22&type=cafe_wa740

https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb1.phinf.naver.net%2F20130524_115%2Fzeroday7_1369364459450INoTP_PNG%2Fjvm_struct.PNG%3Ftype%3Dw740%22&type=cafe_wa740

- JAVA Source : 사용자가 작성한 JAVA 코드
- JAVA Compiler　: JAVA 코드를 Byte Code로 변환시켜주는 기능
- Class Loader :　Class파일을 메모리(Runtime Data Area)에 적재하는 기능
- Execution Engine : Byte Code를 실행 가능하게 해석해주는 기능
- Runtime Data Area : 프로그램을 수행하기 위해 OS에서 할당 받은 메모리 공간

**※ 컴파일러(javac.exe)는 무엇을 하는가?**

① 사용자가 생성한 클래스 코드의 문법을 체크한다.

② 사용사가 생성한 클래스 코드에 컴파일러가 추가적으로 필요한 코드와 새로운 문법 코드를 삽입한다.

- java.lang package의 import 기능 등 사용된 클래스들의 전체 패키지 경로를 식별
- 상속이 없으면 object 기본 상속
- 생성자가 없으면 기본생성자 삽입
- interface 라면 메소드에 public abstract 처리
- Java 최신 버전의 문법 코드로 변경

③ 기본적인 최적화 작업을 수행

④ bytecode로 변환

**4. Runtime Data Area**

**① Class Area**

[변수 정보는 Field 클래스에서 관리한다: 나중에 살펴볼 것](https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb4.phinf.naver.net%2F20130524_52%2Fzeroday7_13693703001440OtuB_PNG%2Fclassarea.PNG%3Ftype%3Dw740%22&type=cafe_wa740)

변수 정보는 Field 클래스에서 관리한다: 나중에 살펴볼 것

- Method Area, Code Area, Static Area 로 불리어짐

i) Field Information : 멤버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보

ii) Method Information : 메서드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보

iii) Type Information : - Type의 속성이 Class인지 Interface인지의 여부 저장

- Type의 전체이름(패키지명+클래스명)
- Type의 Super Class의 전체이름

(단, Type이 Interface이거나 Object Class인 경우 제외)

- 접근 제어자 및 연관된 interface의 전체 리스트 저장

iv) 상수 풀(Constant Pool)

- Type에서 사용된 상수를 저장하는 곳(중복이 있을 시 기존의 상수 사용)
- 문자 상수, 타입, 필드, Method의 symbolic reference(객체 이름으로 참조하는 것)도 상수 풀에 저장

v) Class Variable

- Static 변수라고도 불림
- 모든 객체가 공유 할 수 있고, 객체 생성 없이 접근 가능

vi) Class 사용 이전에 메모리 할당

- final class 변수의 경우(상수로 치환되어) 상수 풀에 값 복사

**② Stack Area**

https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb2.phinf.naver.net%2F20130524_124%2Fzeroday7_1369370474822MiKoH_PNG%2Fstack_area.PNG%3Ftype%3Dw740%22&type=cafe_wa740

- Last In First Out (LIFO)
- 메서드 호출 시마다 각각의 스택프레임(그 메서드만을 위한 공간)이 생성
- 메서드 안에서 사용되어지는 값들 저장, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장
- 메서드 수행이 끝나면 프레임별로 삭제

**③ Heap Area**

https://dthumb-phinf.pstatic.net/?src=%22http%3A%2F%2Fcafeptthumb2.phinf.naver.net%2F20130524_192%2Fzeroday7_1369364828182J6j5s_PNG%2Fheap.PNG%3Ftype%3Dw740%22&type=cafe_wa740

- **new 연산자로 생성된 객체와 배열을 저장하는 공간**
- 클래스 영역에 로드된 클래스만 생성가능
- Garbage Collector를 통해 메모리 반환

i) Permanent Generation

- 생성된 객체들의 정보의 주소 값이 저장된 공간

ii) New Area

- Eden : 객체들이 최초로 생성되는 공간
- Survivor : Eden에서 참조되는 객체들이 저장되는 공간

iii) Old Area : New Area에서 일정시간이상 참조되고 있는 객체들이 저장되는 공간

**④ Native method stack area**

- 자바 외의 다른 언어에서 제공되는 메서드들이 저장되는 공간

**⑤ PC Register**

- Thread가 생성 될 때마다 생성되는 공간
- Thread가 어떤 부분을 어떤 명령으로 실행할 지에 대한 기록
- 현재 실행되는 부분의 명령과 주소를 저장

**5. Garbage Collection**

- 참조되지 않은 객체들을 탐색 후 삭제
- 삭제된 객체의 메모리를 반환
- Heap 메모리의 재사용

**① Minor Garbage Collection**

i) New 영역에서 일어나는 Garbage Collection

ii) Eden영역에 객체가 가득 차게 되면 첫 번째 Garbage Collection 발생

iii) Survivor1 영역에 값 복사

iv) Survivor1 영역을 제외한 나머지 영역의 객체들을 삭제

v) Eden영역과 Survivor1영역의 메모리가 기준치 이상일 경우, Eden 영역에 생성된

객체와 Survivor1영역에 있는 객체 중 참조되고 있는 객체가 있는지 검사

vi) 참조되고 있는 객체를 Survivor2 영역에 복사

vii) Surviver2 영역을 제외한 영역의 객체들을 삭제

viii) 일정시간이상 참조되고 있는 객체들을 Old영역으로 이동

ix) 반복

**② Major Garbage Collection (Full Garbage Collection)**

i) Old영역에 있는 모든 객체들을 검사

ii) 참조되지 않은 객체들을 한꺼번에 삭제

iii) Minor Garbage Collection에 비해 시간이 오래 걸리고 실행 중 프로세스가 정지

https://interviewnoodle.com/jvm-architecture-71fd37e7826e