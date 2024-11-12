---
title: "가상 메모리 관련 개념들"
excerpt: "System call / DMA / Segregated list, Demand Zero Memory"

categories:
  - Jungle
tags:
  - malloc
  - CS:APP
---

> 👉 가상메모리 [1)](https://hyeyoo.com/152) [2)](https://dapsu-startup.tistory.com/entry/Malloc-Lab-%EB%8F%99%EC%A0%81-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%95%A0%EB%8B%B91-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC) [3)](https://beeehappy.tistory.com/52)(정글러)

## System call

운영체제의 Kernel이 제공하는 서비스에 대해 응용 프로그램이 커널에 접근하기 위한 인터페이스. 사용자 프로그램이 디스크 파일에 접근하거나 화면에 결과를 출력하는 등 작업이 필요한 경우, 운영체제에 명령을 수행하는 것을 요청하는 것이 시스템 콜이다.

운영체제-응용프로그램 간의 인터페이스 역할을 하며, 응용프로그램이 하드웨어와 상호작용 할수 있도록 한다. 운영체제의 커널모드에서 실행되며, 커널이 하드웨어에 직접 엑세스 할 수 있는 유일한 모드이기 때문이다. 사용자 프로그램은 커널모드에서 실행할 수 없으므로 시스템 콜을 통해 커널에 엑세스해야 한다.

운영체제마다 다르지만, 파일 생성 및 쓰기, 메모리 할당 및 해제, 스레드 생성 및 제어 같은 일반적인 시스템 콜이 존재함. 

## DMA(Direct Memory Access)

입출력 장치(I/O)가 CPU를 우회하여 메모리에 직접 액세스 할 수 있는 기능이다. 이를 통해 데이터를 메모리로 보내거나 메모리에서 읽을 수 있다. CPU보다 빠른 속도로 데이터를 저장해야 하는 경우 자주 사용되며,  

## Malloc Lab: Writing a Dynamic Storage Allocator

카네기 멜론 대학교 Computer Science 강의 [강의 노트](https://drive.google.com/file/d/1gjnMEJ-y_b7PgqTAq-efAWA-y1djKzy6/view?usp=sharing)

mm.h에 정의된 함수들을 mm.c에 구현하여 동적 메모리 할당기를 구현한다.

```c
int mm_init(void);
void *mm_malloc(size_t size);
void mm_free(void *ptr);
void *mm_realloc(void *ptr, size_t size);
```

`mm_init`: `mm_malloc`, `mm_realloc`, `mm_free`를 호출하기 전에, 응용 프로그램은 이 함수를 호출하여 힙 영역을 시작하고 할당하는 초기화를 시작해야 한다. 리턴 값은 초기화시 오류가 발생하면 `-1`을, 아니라면 `0`을 반환한다.

`mm_malloc`: 최소 `size` 바이트의 할당된 블록 payload의 포인터를 반환한다. 블록 전체는 모두 힙 영역 안에 있어야 하며, 다른 할당 덩어리와는 겹쳐져서는 안된다.

c 라이브러리 내의 (libc) malloc과 비교할텐데, 이 함수는 항상 8바이트의 payload pointer를 반환하므로, 구현할 함수도 비슷하게 작동해야 할 것이다.

`mm_free`: `ptr`에 의해 지목된 블록을 해제한다. 아무것도 리턴하지 않으며, 전달받은 포인터가 선행된 `mm_malloc`이나 `mm_realloc`에 의해 반환된 포인터이고, 할당 해제를 겪지 않았어야 한다.

`mm_realloc`: 다음 조건의 최소 size 바이트의 영역의 포인터를 반환해야 한다.

- `ptr`가 NULL이라면, 해당 함수는 `mm_malloc(size);`와 동일하다.
- `size`가 0이라면, 해당 함수는 `mm_free(ptr);`과 동일하다.
- `ptr`이 NULL이 아니라면, `ptr`은 이전의 `mm_malloc`과 `mm_realloc`에 의해 반환되었어야 한다. `mm_realloc`는 `ptr`에 의해 지목된 블록의 사이즈를 `size` 바이트로 바꾸고 새로운 블록의 주소를 리턴한다. 이 새로운 블록의 주소는 이전과 같을수 있으나, 다를수도 있다: 구현 방법과 이전 조각의 내부 fragmentation, 그리고 realloc 함수에 요구된 size에 따라서 달라질 수 있다.
    
    새로운 블록의 내용은 이전 `ptr`블록의 것과 같을 수 있으나, 이전 블록의 크기보다 크고 새로운 블록의 크기까지 가능하다. 다른 것들은 모두 초기화되지 않는다. 예를들어, 8바이트가 이전 크기이고 12바이트가 새로운 크기라면, 이전의 8바이트는 새로운 블록에서 똑같으며, 나머지 4바이트는 초기화되지 않는다. 비슷하게, 8바이트가 이전 크기이고 4바이트가 현재 크기라면, 새로운 블록의 내용은 이전 블록의 4바이트와 동일하다.
    

이상 기능들은 libc의 malloc, realloc, free와 동일하다.

메모리 allocator는 untyped pointer 조작을 다루기 때문에 효율적이고 올바르게 프로그래밍하기 어렵다. heap checker를 통해 heap을 스캔하고 그 지속성을 확인하는 것이 좋다. heap checker를 통해 다음을 확인할 수 있다:

- 모든 free list의 블록이 free인가?
- 블록 통합을 벗어난 연속된 free블록이 있는가?
- 모든 free 블록이 실제로 free list에 있는가?
- free list의 포인터가 유효한 free 블록인가?
- free list의 포인터들이 유효한 free 블록을 가르키는가?
- 할당된 블록들이 겹치는가?
- 힙 블록의 포인터가 유효한 힙 주소를 가르키는가?

int mm_check(void) 함수는 불변량invariants와 지속성 consistency를 확인하며, 힙이 consistence하다면 nonezero값을 반환할 것이다.

이하 규칙들은 해당 문서 참고

# mmap()

https://devraphy.tistory.com/428

# sbrk()

https://en.wikipedia.org/wiki/Sbrk

https://aidencom.tistory.com/208