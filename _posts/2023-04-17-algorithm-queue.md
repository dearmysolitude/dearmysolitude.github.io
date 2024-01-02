---
title: "Queue"
excerpt: "큐!"

toc: true
toc_sticky: true

categories:
  - Algorithm
tags:
  - Data Structure
---
먼저 들어온 데이터가 먼저 나가는 선입선출 구조의 자료구조. FIFO(First In First Out) 삭제 연산이 이루어지는 곳을  Front, 삽입 연산이 이루어지는 것을 rear라고 한다.  큐를 사용하면 데이터를 추가한 순서대로 제거할 수 있기 때문에 비동기 메세징(asynchronous messaging), 스트리밍(streaming), 너비 우선 탐색(breath first serach)등 소프트웨어 개발에 널리 응용되고 있다.

은행 업무, 프로세스 관리, 서비스 센터의 대기시간, 대기열 순서 같은 우선순위 작업 예약 

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216748&authkey=%21APY9QJMDhQtCDbc&width=1219&height=397" alt="">
  <figcaption>선형 큐</figcaption>
</figure>

큐의 rear에 데이터를 추가하는 것을 enaqueue라고 하며, front에서 제거하는 것을 dequeue라고 한다.

## 시간복잡도

| Operation | Average | Worst |
| --- | --- | --- |
| Access | Θ(n) | O(n) |
| Search | Θ(n) | O(n) |
| Insert (enqueue) | Θ(1) | O(1) |
| Delete (dequeue) | Θ(1) | O(1) |

## 구현

스택과 동일

*linked list로 생각해보 어느 방향에서 enqueue 되어야 하는지 dequeu되어야 하는지 파악하기 쉽다.

## 선형큐와 원형큐

선형큐에서 데이터 삭제시마다 이동하지 않고 인덱스로 큐의 연산을 진행하면 rear에만 데이터가 있음에도 꽉 찼다고 인식한다. 원형 큐는 이것을 보완하기 위해 사용한다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216746&authkey=%21AFA0_q1M1KkS0Yw&width=581&height=414" alt="">
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216747&authkey=%21ABQdvgaEqaUIb6M&width=599&height=224" alt="">
  <figcaption>원형 큐</figcaption>
</figure>

# 우선순위 큐(Priority Queue)

일반적으로 큐는 FIFO이다. 우선순위가 적용된 우선순위 큐는 우선순위가 높은 데이터가 먼저 나오는 것이다. 힙(heap) 자료구조를 통해 구현할 수 있다.

배열로 구현할 경우: 우선순위가 높은 것은 상관 없지만, 우선 순위가 중간인 데이터가 입력될 경우 뒤쪽의 인덱스를 모두 밀어야 함. 삽입시 최악의 경우 n개의 자료가 있을 시 시간복잡도는 O(n)이 된다. 이 경우 삭제는 O(1), 삽입은O(n)

연결 리스트로 구현할 경우: 이 경우도 데이터 삽입 시 위치 찾는데에 시간복잡도가 증가한다.

힙의 경우 삭제나 삽입의 경우 모두 부모와 자식간의 비교만 이루어진다. 삭제는 O(log2n), 삽입은 O(log2n)