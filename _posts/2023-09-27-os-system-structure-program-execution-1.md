---
title: "System Structure & Program Execution 01 / 운영체제"
excerpt: "본격적으로 운영체제에 들어가기 전, 하드웨어 동작에 대해 알아본다."

toc: true
toc_sticky: true

categories:
  - Computer Science
tags:
  - Operating System
---
# 컴퓨터 시스템 구조
![System-structure](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216635&authkey=%21AAklKiqewBBcNUw&width=442&height=334)

## CPU
![CPU](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216613&authkey=%21AERuDg09KF-ea8o&width=116&height=86)

- CPU는 메모리로부터 매 clock cycle마다 기계어 instruction를 읽어들인다
- Register: 메모리보다 더 빠르면서 정보를 저장할 수 있는 공간, 메모리와의 속도 차이를 보완하는 역할을 한다.
- Mode Bit: CPU에서 실행되는 것이 운영체제인지 사용자 프로그램인지 구분
- Interrupt Line: CPU는 Instruction만 실행하는 역할만 수행. 키보드 입력이 들어오거나 디스크에서 읽어오는 것을 완료한 경우 CPU는 이 interrupt를 통해 관련 처리를 수행한다.

메모리의 프로그램 A -> ( 특정 디바이스 접근 요청 ) -> CPU 에서 해당 인스트럭션 처리 -> device controller를 통해 명령 처리 -> interrupt를 통해 관련 처리 완료를 전달하고 관련 작업을 처리, 전달되기 전까지는 CPU는 다른 프로그램을 처리한다.

## Timer
![timer](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216636&authkey=%21ANDyWYI_AvK3IQU&width=663&height=382)

- 만약, 무한 루프를 도는 프로그램이 실행되어 있다고 해 보자. 이 경우 CPU는 명령을 처리중이지만 다른 명령을 수행할 수 없는 상태이다. 그럼 이 상황을 어떻게 컴퓨터는 다룰까? -> Timer
- Timer는 특정 프로그램이 CPU를 독점하는 것을 막기 위해 있는 하드웨어.
- 사용자 프로그램이 실행되면 CPU를 그냥 넘겨주지 않고 timer에 값을 셋팅 후 넘겨준다.
- 여기 할당된 시간을 사용하면 타이머가 CPU에 interrupt를 걸어주어 다른 프로그램으로 제어권을 넘겨준다(다른 프로그램의 instruction을 실행하도록 한다).
- CPU는 instruction을 한 줄 읽고, interrupt line을 확인하고(interrupt가 들어왔는지 확인), 다음 instruction을 실행하고 interrupt line을 확인하는 식으로 작동한다.
- Interrupt가 들어오면 CPU의 제어권이 사용자 프로그램으로부터 운영체제로 자동으로 넘어간다.

## Mode bit
![modebit](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216625&authkey=%21AHJwH6kkTPwIWWs&width=508&height=417)

- Mode bit = 0: 메모리 접근, I/O 디바이스 접근 모두 허용된다.
- Mode bit = 1: 제한된 instruction만 실행할 수 있게 되어 있다.


## 메모리
    CPU의 작업 공간
    명령줄을 전달한다.

## I/O Device

- 키보드, 마우스, 모니터, 프린터
- HardDisk 보조기억장치이기도 하지만 I/O device로 보기도 한다. input, output 모두 담당한다.
- CPU가 작업 공간(메인 메모리)가 있듯이, device controller도 그들만의 작업 공간을 가진다: Local buffer
- 운영체제를 통해서만 I/O device에 접근할 수 있음, 따라서 사용자 프로그램은 I/O device를 사용하기 위해 System call을 통해 운영체제가 I/O device에 접근하도록 요청해야 한다.

사용자 프로그램의 system call: trap 발생(Software Interrupt) > 제어권 운영체제로: 제어권이 인터럽트 벡터가 가르키는 서비스 루틴으로 > 올바른 I/O 요청인지 확인 후 I/O 수행 > I/O 완료 후 device controller가 Hardware Interrupt 발생

### System call
사용자 프로그램의 운영체제에 있는 커널 함수의 호출
![system-call](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216634&authkey=%21AArKalEuo8BL5iE&width=246&height=187)

- 코드가 사용자 프로그램 내에서 함수를 호출하여 메모리 주소를 바꾸는 것과는 조금 다르다.
- Mode bit이 1일 때에는 운영체제 권한의 기능은 수행할 수 없다(즉 여기에서 필요한 기능인 입출력 장치의 접근을 할 수 없다는 것).
- 사용자 프로그램은 interrupt line을 실행하는 instruction을 실행한다: (DMA나 device controller가 아닌) 프로그램이 운영체제에 요청하기 위해 software적으로 interrupt를 걸 수 있다.
- Mode bit 이 0 으로 바뀌고 제어권은 운영체제에 넘어간다.

## Device Controller
![DeviceController](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216629&authkey=%21AB-hjHXwgOGJh0c&width=522&height=414)
- CPU와 I/O device의 속도 차이: CPU ~ HardDisk->100 만배 차이 => 디스크를 CPU가 직접 처리하지 않는다
- 작은 CPU 같은 컨트롤러가 붙어있어 (예를 들어 하드디스크의 경우, 디스크에서 헤드가 어떻게 움직이고, 어떤 데이터를 읽어야 하는지 제어하는 역할을 한다. 즉, 디스크를 통제하는 것은 CPU가 아닌 디스크에 붙어있는 device controller가 수행한다.)
- CPU는 메모리 접근도, local buffer 접근도 가능하지만, device controller들은 자신의 local buffer만 접근 가능하다.

## DMA(Direct Memory Access)

- ISSUE: CPU의 interrupt가 과도하게 많아짐 -> DMA
- CPU와 DMA모두 메모리에 접근할 수 있다.
- CPU는 자기 작업을 진행하고, 로컬 버퍼로 들어오는 데이터들은 작업이 끝났다면 DMA가 직접 메모리에 적재하여 준다. 이 작업이 끝나면 CPU에 interrupt를 한 번만 걸어준다: CPU 에 들어오는 interrupt 빈도 수를 줄여 원활히 작동하도록 도와준다.
- 둘이서 특정 메모리 구역에 동시에 접근하는 경우에는 문제가 발생할 수 있기 때문에 memory controller는 이를 중재하는 역할을 한다.

## 인터럽트(Interrupt)
![interrupt](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216632&authkey=%21AJGrqOrBWbMZNE4&width=514&height=387)

위에서 device들을 설명하면서 언급한 인터럽트들. 현대의 운영체제는 인터럽트에 의해 구동된다.
