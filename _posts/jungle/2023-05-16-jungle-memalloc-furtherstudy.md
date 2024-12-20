---
title: "Memory Allocator 예제 코드 발전시키기"
excerpt: "Demand Zero Memory, gdb를 이용한 malloc-lab디버깅과 엔디안의 개념"

categories:
  - Jungle
tags:
  - malloc
  - CS:APP
---
malloc-lab을 구현하는데 사용했던 textbook의 내용은 Implicit Free List / First Fit 을 이용해 메모리 할당을 진행한 코드이다.

## 예제 코드의 발전

### Allocator의 성능

할당기는 다음 두가지 속성의 균형을 잡음으로써 성능을 증가시킬 수 있다:

**처리량throughput**: 단위 시간당 완료되는 요청의 수. 할당과 반환 요청들을 만족시키기 위한 평균 시간을 최소화하여 처리량을 확보할 수 있다.

**이용도utilization**: 디스크 내의 스왑 공간의 양에 의해 제한되는 유한한 가상메모리를 효율적으로 사용하는 정도. 최고 이용도peak utilization을 통해 평가한다: (할당된 블록들의 데이터합)/(힙의 크기)

### 내부 단편화와 외부 단편화

**내부 단편화**: 할당된 블록이 데이터 자체보다 더 클 때. 정량화가 간단, 할당된 블록의 크기와 이들의 데이터 사이의 차이의 합을 구하여 정량화 한다.

**외부 단편화**: 메모리 공간이 전체적으로 공간을 모았을 때에는 충분한 크기가 존재하지만, 요청을 처리할 수 있는 단일한 가용블록은 없는 경우이다. 정량화가 어려운데, 미래의 요청 패턴에 의존하기 때문이다.

정량화가 쉽지 않고 예측이 어려운 외부 단편화를 늘리는 것보다, 더 적은 수의 큰 free block을 유지하여 단편화를 관리하는 방법들이 주로 사용된다.

### 예제 코드의 성능 증가시키기

Memory Allocator를 구현하는데에는 다음과 같이 여러 방법을 사용해 수정할 수 있다:

#### 가용 메모리 블록을 찾는 정책:

- **first fit**: free list를 처음부터 검색해서 크기 맞는 첫 번째 블록을 선택한다. 리스트 마지막에 가장 큰 free block이 남아 큰 블록을 찾는 경우 검색 시간이 늘어난다.
- **next fit**: 처음에서 시작하는 대신 이전 검색이 종료된 지점에서 검색을 시작한다. frist fit에 비해 매우 빠르나 최악의 메모리 이용도를 갖는다. [next fit algorithm](https://www.geeksforgeeks.org/program-for-next-fit-algorithm-in-memory-management/)
- **best fit**: 모든 가용 블록을 검사하며 크기가 맞는 가장 작은 블록을 선택한다. 메모리 이용도는 위 두 방법보다 좋지만 힙을 완전히 다 검색해야 하므로, 이를 단순화하지만 힙을 모두 검색하지 않는 segregated free list가 있다.

#### 가용 가능 블록을 관리하는 방법:

- **암시적 가용 리스트implicit free list**: header와 footer, payload와 padding으로 이루어진 블록을 사용하여, 블록 검색을 힙을 포인터가 순차적으로 돌면서 모조리 다 수행한다. 블록 할당 시간이 전체 힙 블록의 수에 비례하기 때문에 범용 allocator에는 적합하지 않음.
- **명시적 가용 리스트explicit free list**: 블록들을 명시적 자료구조로 구현하는 방법을 통해 위 implicit free list를 보완할 수 있다. 이중 연결 리스트를 payload내에 저장하여 이전을 가르키는 pred, 다음을 가르키는 succ 포인터를 사용한다. 이로써 first fit할당 시간을 전체 블록수에 비례하는 것에서 가용 블록 수에 비례하는 것으로 바꿀 수 있다. 하지만 블록 반환 시간은 블록 정렬 정책에 따라 비례 혹은 상수시간을 가지게 할 수 있다.
    - 반환 방법1: 후입선출LIFO(last-in first-out)식으로 리스트를 유지하여 최근 사용한 블록들을 조사한다. 이 경우 상수시간이 소요된다.
    - 반환 방법 2: 리스트를 주소 순으로 관리한다. 각 블록의 주소가 다음 블록의 주소보다 작게 하는 것이다. 선형 검색 시간이 소요된다. 이 경우 LIFO순서 사용하는 것보다 좋은 메모리 이용도를 가진다.(best fit에 근접)
    
    explicit free list의 경우 블록 크기가 커져 내부 단편화의 위험이 증가한다.
    
- **분리 가용 리스트segregated free list**: 분리 저장장치segregated storage를 사용하여 할당 시간을 줄인다. 다수의 free list를 유지하며, 각 리스트는 동일한 크기의 블록들을 저장한다. 모든 가능한 블록 크기를 크기 클래스size class라는 동일 클래스의 집합으로 분리한다-즉 크기별로 블록을 분류하여 클래스로 관리한다-.여러 논문에서 변형된 분리 저장장치에 대해 설명하고 있으나 두가지 기본 방법은 다음과 같다.
    - **단순한 분리 저장장치simple segregated storage**: 주어진 크기를 할당하기 위해 적절한 가용 리스트를 검색하게 된다. 비어있지 않다면 첫 블록 전체를 할당한다. free block들은 절대로 분할하지 않는다(할당 요청을 만족시키기 위해). 리스트가 비어있다면 할당기는 운영체제로부터 추가 고정크기 메모리 요청(힙) 이를 동일한 크기 블록으로 나누어 연결하여 새로운 free list를 만든다. 블록 반환 시에는 이 블록을 가용 리스트 맨 앞에 삽입한다.
        
        상수시간 연산, 블록의 오버헤드가 1워드succ포인터 뿐으로 매우 작은 점이 장점이다: 메모리 블록은 클래스에 따른 동일한 크기를 가지기 때문에 크기를 표시할 필요 없고 연결 작업도 없기 때문에 할당, 가용 태그가 필요 없으며 헤더와 풋터가 필요없다. 할당/반환 연산이 가용 리스트 시작 부분에서만 발생하므로 이중 연결 리스트일 필요도 없다.
        
        다만, 외부 단편화에 취약하다. 가용 블록들이 절대 연결되지 않기 때문이다.
        
    - **분리 맞춤segregated-fit**: free list의 배열을 관리한다. 각 free list는 size 클래스에 연관되며, 명시/묵시적 리스트로 구성된다. 이들은 서로 다른 크기의 블록들을 포함하며 크기 클래스에 한정된 범위 하에서 존재한다.
        
        할당: 요청받은 크기를 통해 크기 클래스를 결정한다. 해당 free list를 first-fit방식으로 검색한다. 1)검색에 성공하면 (선택적으로) 블록을 분할하고 나머지를 적당한 free list에 삽입한다. 2)찾지 못하면, 다음 크기의 클래스의 free list에서 검색한다. 적절한 블록을 찾을 때까지 반복한다. 그래도 찾지 못하면 운영체제에 힙 메모리를 요청하고 여기에 블록을 할당하고 나머지는 적절한 크기 클래스에 배치한다. 
        
        반환: 적절한 가용 리스트에 배치하고 연결한다.
        
        빠르고 메모리 관리가 효율적이어서 C표준 라이브러리에서 제공하는 GNU malloc같은 패키지에서 사용된다. 힙의 특정부분에서 검색하기 때문에 검색 시간이 준다. 이 방식을 사용한 first fit검색이 전체 힙의 best-fit 검색을 단순화한 개념이라는 사실에서 이용도가 개선됨을 알 수 있다. 
        

## demand zero memory

필요할 때(demand) 0으로 초기화 해주는(zero) 메모리이다.

> Bard의 답변:
> 
> 
> Demand-zero memory는 메모리 관리에서 사용되는 용어로, 프로세스가 특정 영역의 메모리를 요청할 때마다 해당 영역의 메모리를 0으로 초기화하는 것을 의미합니다. 이 방식은 메모리 사용량을 최소화하고, 메모리 할당을 효율적으로 관리할 수 있습니다.
> 
> Demand-zero memory는 주로 가상 메모리 시스템에서 사용됩니다. 가상 메모리 시스템은 프로세스마다 가상 주소 공간을 할당하고, 이 가상 주소 공간을 물리 메모리와 매핑합니다. 프로세스가 특정 영역의 메모리에 접근할 때마다, 가상 메모리 시스템은 해당 영역의 물리 메모리를 할당하고, 해당 영역을 0으로 초기화합니다.
> 
> Demand-zero memory는 다음과 같은 장점이 있습니다.
> 
> - 메모리 사용량을 최소화할 수 있습니다.
> - 메모리 할당을 효율적으로 관리할 수 있습니다.
> - 프로세스 간 메모리 보호를 제공할 수 있습니다.
> 
> Demand-zero memory는 다음과 같은 단점이 있습니다.
> 
> - 메모리 할당이 느릴 수 있습니다.
> - 메모리 영역을 0으로 초기화하는 데 시간이 걸릴 수 있습니다.
> - 프로세스가 특정 영역의 메모리에 접근하기 전에 해당 영역이 0으로 초기화되지 않으면, 해당 영역의 메모리 내용이 손상될 수 있습니다.

더 자세한 내용은 Carnegie-Mellon Univ.의 강의 자료를 참고해 보자.

## gdb를 이용한 malloc-lab 디버깅

[참고 사이트](https://condor.depaul.edu/glancast/374class/docs/malloclab-debugging.html)

<aside>
👉 ./mdriver -V를 통해 rep별로 어떤 결과가 나왔는지 확인해볼 수 있다.

</aside>

## 엔디안Endianness

읽어보기: [엔디안 방식에 구애받지 않는 C코드 작성하기](https://dataonair.or.kr/db-tech-reference/d-lounge/technical-data/?mod=document&uid=237549)

엔디안이란 시스템이 정수를 저장하는 방식을 가리킨다. 최상위 바이트(most significant byte)를 가장 낮은 주소에 저장하는 방식을 빅 엔디안(big endian), 최상위 바이트를 가장 높은 주소에 저장하는 방식을 리틀 엔디안(little endian)이라고 한다.

메모리 배열에서 각 주소는 요소 하나를 저장한다. 요소 하나는 한 바이트를 차지한다. 어떤 메모리 구성에서는 한 주소가 한 바이트를 넘어가기도 하지만 극히 드문 케이스이므로 **일단 모든 메모리 주소가 바이트 단위라고 생각하자**.

엔디안 방식은 여러 바이트에 걸쳐 값을 쪼개 연속적인 메모리 배열에 저장하려는 경우 중요하다. 하드웨어나 소프트웨어 아키텍처를 설계하려면 엔디안 방식을 선택해야 한다. 법칙이 존재하지 않는 탓에 아키텍처마다 빅/리틀 엔디안 선택이 달라진다. 즉 **모든 프로세서는 빅엔디안을 따를지 리틀 엔디안을 따를지 결정해야 한다.(한 프로세서 내에서는 동일한 정책을 따를 것)**

엔디안을 고려할 필요가 없는 경우도 있다. 정수에 **비트 연산을 하는 경우는 엔디안 방식에 영향을 받지 않는다**. 시스템이 다중 바이트를 알아서 배열하므로 연산을 수행한 후에도 최위 바이트와 최하위 바이트가 바뀌지 않는다.