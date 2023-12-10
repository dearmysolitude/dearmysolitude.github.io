---
title: "시간복잡도"
excerpt: "알고리즘과 자료구조를 사용할 때 시간복잡도는 왜 사용할까"

toc: true
toc_sticky: true

categories:
  - Computer Science
tags:
  - Algorithm
  - Data Structure
  - Time Complexity
---
![bigO](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216611&authkey=%21APNDcVe-IhUiceA&width=680&height=697)
## 효율적인 알고리즘

알고리즘에서 문제를 푸는 것 만큼이나 중요한 문제는 공간과 시간의 활용을 어떻게 효율적으로 할 것인지에 대한 문제이다.

공간 복잡도와 시간 복잡도는 다양한 알고리즘의 평가 도구로 사용된다.

    공간 복잡도: 얼마나 메모리를 적게 쓰는가?
    시간 복잡도: 얼마나 빠른가(CPU에 얼마나 부담을 주는가)?

최신의 머신에서는 공간 복잡도에 대해 과거에 비해 고민이 많이 줄었으나(그러나 알고리즘 평가시 보조적인 역할을 한다), 시간 복잡도는 문제를 효율적으로 해결하기 위해 필수적으로 고민해야 하는 이슈이다.

 
## 공간 복잡도 Space Complexity

프로그램이 실행되고 완료되기까지 사용하는 총 저장 공간량을 의미한다.

    고정 공간: 알고리즘과 상관 없는 공간으로 코드와 단순 변수, 상수가 해당된다.
    가변 공간: 알고리즘이 수행되며 동적으로 할당되는 공간이 해당된다.

이를 함수로 나타내면 다음과 같다.

S(P) = c + Sp(n)

c는 고정 공간(상수)를, Sp(n)은 가변 공간을 나타낸다.

앞서 설명했듯 최신 머신에서는 성능이 비약적으로 상승함으로써 그리 중요하게 생각되지 않는 요소이나, 빅데이터 등을 다루는 문제에서는 가끔 다루어질 수 있다.

 
## 시간 복잡도 Time Complexity

계산 복잡도 이론에서 문제를 해결하는데 걸리는 시간과 입력의 함수 관계를 가리킨다. 일반적으로 점근 표기법(asymptotic notation, 함수의 증감 추세를 비교하는 표기법, 란다우 표기법Landau notation이라고도 부른다.)을 사용하여 나타낸다. 최악의 시간 복잡도를 가지고 알고리즘을 분류하는 Big-O 표기법을 주로 사용한다. Big-O(점근적 상한, Upper bound) 외에도, Big-Ω(점근적 하한, Lower bound), Big-θ(둘의 평균) 방법으로 표현할 수도 있다.

엄밀하게 따지면, 컴파일 시간과 실행 시간을 합친 의미이지만, 컴파일 시간은 공간 복잡도의 고정 공간과 마찬가지로 알고리즘에 영향을 받는 지표가 아니기 때문에 알고리즘을 평가할 때에는 고려하지 않는다. 하지만 코드가 실행되는 환경, 언어 등의 여러 요인에 따라 소요되는 실제 시간은 다른 것이 당연하다. 따라서 시간 복잡도를 실행 시간을 초단위로 표기하는 것이 아니라, 명령문의 실행 빈도수에 따라 대략적으로 소요 시간을 나타내기 위해 사용하는 것이다.

### 시간 복잡도의 평가 방법

중심이 되는 특정 연산의 횟수를 세어 평가한다.
데이터의 수에 대한 연산 횟수의 함수T(n)을 구한다: 이는 최악의 경우(worst case)를 기준으로 한다(Thus, Big-O notation).

평균적인 경우는 알고리즘 평가에 도움이 되지만 객관적 평가가 쉽이 않고 계산이 어렵다. 또한 평균이라는 것을 증명하기 어렵고, 상황에 따라 알고리즘의 성능이 계속 달라진다. 반면 최악의 경우는 늘 동일하게 나타난다.

데이터가 많은 경우에 유의미하게 차이가 발생하므로, 데이터 수가 적은 경우의 수행 속도는 큰 의미가 없다.

 
![bigO](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216612&authkey=%21ANsaBynlZyIMGtA&width=359&height=215)

    정의: 모든 N>N0 에 대하여, f(N) ≤ k⋅g(N) 이 성립하는 양의 상수 k 와 N0​ 가 존재하면, f(N) = O(g(N)) 이다.

계수와 낮은 차수의 항을 제외시키는 방법으로 시간 복잡도를 표현한다. 즉, 해당 알고리즘이 나타내어진 차수이거나 그보다 낮은 차수의 시간복잡도를 가진다는 의미이다: 그 즉슨, Big-O 표기법은 알고리즘의 최악의 경우를 표현한다.

실제 상황에서 알고리즘의 속도는 정확하지 않을 수 있지만, 이 방법을 사용하는 이유는, n에 대한 일반적인 추세를 확인할 수 있기 때문이다: 입력값의 변화에 따른 알고리즘의 시간 소비를 예측할 수 있다.

Big-O 계산으로는 O(5n +7) = O(5n) = O(n), O(n² + 25) = O(n²) 를 예로 들 수 있다. 하지만 여기에서 등호는 '같다(equals)'가 아닌 '이다(is)', '정도이다(approx)' 라고 해석해야 혼란을 피할 수 있다.(여기에서의 직관적으로 '같다'라고 판단하는 데에서 Big-O의 오용이 발생했고, 이로 인해 Big-θ notation이 등장하였다.)
![bigO](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216610&authkey=%21AEAWPiCbG3GkIsg&width=426&height=29)
이 때 Big-O는 
![bigO](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216639&authkey=%21AL6lGxIhDbWdoJc&width=59&height=27)
 
## 시간 복잡도의 평가: 데이터 수에 따른 처리 시간 증가/수렴

![timecomplexity](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216599&authkey=%21AB_MgZiUxcSOkxU&width=1080&height=723)

시간 복잡도를 표현하는 방법 중 하나인 Big-O에 의한 여러가지 알고리즘의 분류. 가로 축은 처리해야하는 데이터 양(n), 세로 축은 그에 따른 작업량을 의미한다. (출처: [http://bigocheatsheet.com/](http://bigocheatsheet.com/))

> 선호되는 알고리즘. 단연 시간 소모가 적은 왼쪽이 선호된다: O(1) < O(log₂ n) < O(n) < O(n log₂ n) < O(n²) < O(2ⁿ) < O(n!)

#### O(1)

상수 시간(Constant time): 입력 값의 증감과 관계 없이 실행 시간이 동일하다.

```
def hello_world():
    print("hello, world!")
```
#### O(n)

선형 시간(Linear time): 입력이 증가하면 시간 또는 메모리 사용이 선형적으로 증가한다.
```
def print_each(li):
    for item in li:
        print(item)
```
#### O(log n), O(n log n)

로그 시간(Logarithmic time): 입력 값의 크기가 증가함에 따라 실행 시간은 로그 함수 처럼 증가한다. 주로 입력 크기에 따라 처리 시간이 증가하는 정렬 알고리즘에서 많이 사용된다. 아래는 이진 탐색의 예.
```
def binary_search(li, item, first=0, last=None):
	if not last:
		last = len(li)

	midpoint = (last - first) / 2 + first

	if li[midpoint] == item:
		return midpoint

	elif li[midpoint] > item:
		return binary_search(li, item, first, midpoint)

	else:
		return binary_search(li, item, midpoint, last)
```
#### O( n² )

제곱 시간(Square time): 반복문이 두 번 있는 케이스가 대표적이다.
```
def print_each_n_times(li):
    for n in li:
        for m in li:
            print(n,m)
```

### 시간 복잡도를 구하는 요령(Tip)

    하나의 루프를 사용하여 단일 요소 집합을 반복 하는 경우 : O (n)
    컬렉션의 절반 이상 을 반복 하는 경우 : O (n / 2) -> O (n)
    두 개의 다른 루프를 사용하여 두 개의 개별 콜렉션을 반복 할 경우 : O (n + m) -> O (n)
    두 개의 중첩 루프를 사용하여 단일 컬렉션을 반복하는 경우 : O (n²)
    두 개의 중첩 루프를 사용하여 두 개의 다른 콜렉션을 반복 할 경우 : O (n * m) -> O (n²)
    컬렉션 정렬을 사용하는 경우 : O(n*log(n))

#### 정렬 알고리즘 비교

| Sorting Algorithm | 공간 복잡도 | 최악 | 최선 | 평균 |
| --- | --- | --- | --- | --- |
| Bubble Sort | O(1) | O(n) | O(n2) | O(n2) |
| Heapsort | O(1) | O(n log n) | O(n log n) | O(n log n) |
| Insertion Sort | O(1) | O(n) | O(n2) | O(n2) |
| Mergesort | O(n) | O(n log n) | O(n log n) | O(n log n) |
| Quicksort | O(log n) | O(n log n) | O(n log n) | O(n2) |
| Selection Sort | O(1) | O(n2) | O(n2) | O(n2) |
| Shell Sort | O(1) | O(n) | O(n log n2) | O(n log n2) |
| Smooth Sort | O(1) | O(n) | O(n log n) | O(n log n) |

*최악/최선/평균은 모두 시간 복잡도임

#### 자료구조 비교

**Average Case**

|Data Structure | Search | Insert | Delete |
| --- | --- | --- | --- |
| Array | O(n) | N/A | N/A |
| Sorted Array | O(log n) | O(n) | O(n) |
| Linked List | O(n) | O(1) | O(1) |
| Doubly Linked List | O(n) | O(1) | O(1) |
| Stack | O(n) | O(1) | O(1) |
| Hash table | O(1) | O(1) | O(1) |
| Binary Search Tree | O(log n) | O(log n) | O(log n) |
| B-Tree | O(log n) | O(log n) | O(log n) |
| Red-Black tree | O(log n) | O(log n) | O(log n) |
| AVL Tree | O(log n) | O(log n) | O(log n) |

**Worst Case**

|Data Structure | Search | Insert | Delete |
| --- | --- | --- | --- |
| Array | O(n) | N/A | N/A |
| Sorted Array | O(log n) | O(n) | O(n) |
| Linked List | O(n) | O(1) | O(1) |
| Doubly Linked List | O(n) | O(1) | O(1) |
| Stack | O(n) | O(1) | O(1) |
| Hash table | O(n) | O(n) | O(n) |
| Binary Search | O(n) | O(n) | O(n) |
| B-Tree | O(log n) | O(log n) | O(log n) |
| Red-Black tree | O(log n) | O(log n) | O(log n) |
| AVL Tree | O(log n) | O(log n) | O(log n) |

### 예제: 코드를 Big-O 로 표기해 보자

```python
def fibonacci(n):
    if n < 0:
        return
    if n <= 1:
        return n

    result = 0
    f1 = 0
    f2 = 1

    for _ in range(2, n + 1):
        result = f1 + f2
        f1 = f2
        f2 = result

    return result
```

- 이 함수는 n 번째 피보나치 수열 값을 반환하는 fibonacci 함수이다. 이를 분석해보자.
- 주어진 n값이 0보다 작거나, 1 이하인 경우는 일반적이지 않으므로 고려하지 않는다(n은 1보다 크다).

#### 상수항 부 구하기

- 반복문 밖의 명령문들은 1 번만 수행된다.
- if 문 각각 한 번씩 실행하여 2 회, 변수 값 할당 3 회, 마지막 return 문 1 회가 발생한다. 상수항은 6.

#### 다항식 부 구하기

- for 문에서 최대 n-1 회 반복이 발생한다.
- 반복문 조건 체크 1 회, 반복문 내부 명령 3 회로, 내부적으로 4회의 코드가 실행된다.
- 다항식 항은 4(n-1).

#### 실행 시간 함수 구하기

f(n) = 4n - 2

이를 Big-O로 바꾸기 위해 차수가 가장 높은 n 항만 남기고 계수를 지운다: O(n)

이는 다음과 같이 정리할 수 있다.

1. 실행 빈도 수를 구하고 실행 시간 함수를 찾는다.
2. 실행 시간 함수에 가장 큰 영향을 주는(가장 높은 차수) n에 대한 항 만을 남긴다.
3. 계수를 지우고 O 우측 괄호 안에 표기한다.
 
<br> 

# 참고자료

[[Khan Academy] 점근적 표기법](https://ko.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/asymptotic-notation)

[MIT Lecture](http://web.mit.edu/16.070/www/lecture/big_o.pdf)

[Crocus님 블로그](https://www.crocus.co.kr/217)

[Chulgil님 블로그](https://blog.chulgil.me/algorithm/)

[Hudi님 블로그](https://hudi.blog/time-complexity/)