---
title: "Binary Searh"
excerpt: "이분 탐색"

toc: true
toc_sticky: true

categories:
  - Algorithm
tags:
  - Binary Search
  - 이분 탐색
---
오름차순 분류된 아이템을 찾고자 할 때 유용한 알고리즘. 반복적으로 아이템이 존재할 만한 구간을 반으로 나누어 계속 확인한다. 퍼포먼스가 매우 좋다.

- `target`: 찾고자 하는 value
- `index`: 찾고 있는 최근 위치
- `left`, `right`: 현재 구간에서 유지하는 표지
- `mid`: 반으로 나눈 구간 중 어느쪽을 탐색해야 할 지 판단하는 조건

## 구현을 위해 생각해야 하는 것

### 경계 조건과 pivot

1. 경계 조건을 잡기 위해 left와 right을 잡는다. 모든 index들은 inclusive range인 `[left, right]`안에서 모두 찾을 수 있다. 보통 `left ≤ right`의 조건을 사용하는 while문을 사용한다.
2. pivot point를 잡는다: 중간 `index`를 통해 중간의 값`nums[mid]`을 구하고, 이를 target과 비교하여 어느 쪽에 target이 있을지 판단한다.
3. 분기 조건
    - `nums[mid] = target` loop 종료
    - `nums[mid] < target` 새로운 탐색 범위: left = ,mid +1
    - `nums[mid] > target` 

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216770&authkey=%21AHmTtF4TMDw9m7c&width=1201&height=401" alt="">
  <figcaption>Binary Search의 시작</figcaption>
</figure>

### 루프의 중단

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216769&authkey=%21APGv4iHEw0A1cKM&width=962&height=371" alt="">
  <figcaption>루프 중단: 원소가 1개인 배열을 예를들어 그림을 확인해 보자</figcaption>
</figure>

세가지 경우 모두 조건에 따라서 루프를 중단한다. 그림오류: 제일 아래 그림은 부등호가 반대이다.

1. 경계 조건: `left = 0`, `right = nums.size -1`
2. 중간 인덱스: `mid = (left +right) // 2`
3. `mid` value와의 비교

target을 찾지 못하고 끝나면, -1을 반환한다.

## Complexity Analysis

nums의 크기를 n이라고 할 때,

시간복잡도: $O(logn)$

nums는 시간마다 둘로 나뉘어진다. 최악의 경우, 원소가 없을 때 까지 계속해서 잘라야 한다.(logarithmic time)

공간복잡도: $O(1)$

loop를 돌 때, left, right, mid와 정해진 배열 공간만 필요로 한다.

```python
def bisearch(nums, target, left, right):
    while True:
        mid = (left + right) // 2
        if nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid - 1
        else: return mid  #target 이 mid에 있는 경우
        if left > right: return -1
# 이분탐색을 진행하는 간단한 함수
```

[ 백준: 1920. 수 찾기](https://www.acmicpc.net/problem/1920)

## 응용: 매개변수 탐색

**결정 문제를 최적화 과정(이진 탐색)을 통해 풀 수 있도록 하는 기술**

> 👉 어떤 조건을 만족하는 경우를 이진 탐색을 통해 제거해 나갈 수 있다.

최적화 문제: 가능한 해들 중 가장 좋은 해를 찾는 것

결정 문제: 답이 이미 결정되었다고 보는 문제

1. 찾고자 하는 것을 매개변수로 잡고, Yes / No 판단 문제로 치환한다.
2. 다음 단계로 넘어가려면, 모든 값에 Yes/No 결정을 내릴 수 있고, 정렬되어있어야 한다(이진 탐색의 조건).
3. Yes / No 를 판단해가며 이진 탐색을 할 수 있도록 알고리즘을 설정한다.

[백준: 2805. 나무자르기](https://www.acmicpc.net/problem/2805) / [백준: 2110. 공유기 설치](https://www.acmicpc.net/problem/2110)

## 응용: 투포인터 알고리즘

Naive방식인 그냥 탐색(반복문)을 사용하다보면 시간 초과가 걸리는 경우가 있다. 이 경우 사용하면 메모리, 시간 효율성을 높일 수 있다. 두가지 경우가 가능한데,

1. 앞에서 시작하는 포인터와 끝에서 시작하는 포인터가 만나는 방식
2. 빠른 포인터(fast runner)가 느린 포인터(slow runner)보다 앞서는 형식

### 1. 포인터가 양쪽에서 시작하여 만나는 방식

예: 두 수의 합이 특정 수인 조합을 구하는 문제

- 두 수의 합이 target보다 작으면 i 포인터를 앞으로 하나 이동(i += 1)
- 두 수의 합이 target보다 크면 j 포인터를 앞으로 하나 이동(j -= 1)

### 2. 토끼와 거북이

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216771&authkey=%21AKQPqamF2RM4IlI&width=824&height=1227" alt="">
  <figcaption>Fast Runner와 Slow Runner</figcaption>
</figure>

- Fast runner은 각 반복문마다 커지고, slow runner는 커지는 데에 조건이 있는 편으로 사용된다.
- Fast runner은 1씩 증가하는 반면, slow runner는 fast runner가 가르키는 값과 같다면 움직이지 않는다.
- 달라지면 slow runner가 움직이고, 해당 요소의 값을 fast runner가 가르키는 것으로 수정한다.
- 이 루프를 반복하다보면 새로 만들어지는 중복 없는 배열은 slow+1이 새로운 배열의 idx 수이다.

요소의 연산 이외에도 두 집합의 합집합에도 사용할 수 있는 것 같다. 어떻게 풀 수 있을까?

⇒ [11728 배열 합치기](https://www.acmicpc.net/problem/11728)