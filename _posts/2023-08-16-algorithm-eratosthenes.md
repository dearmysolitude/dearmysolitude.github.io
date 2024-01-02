---
title: "Seive of Eratosthenes"
excerpt: "에라토스테네스의 체를 사용한 문제 풀이"

toc: true
toc_sticky: true

categories:
  - Algorithm
tags:
  - 에라토스테네스의 체
---
## 1456 거의 소수

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞힌 사람 | 정답 비율 |
| --- | --- | --- | --- | --- | --- |
| 2 초 | 256 MB | 9825 | 2482 | 1682 | 24.215% |

### 문제

어떤 수가 소수의 N제곱(N ≥ 2) 꼴일 때, 그 수를 거의 소수라고 한다.

두 정수 A와 B가 주어지면, A보다 크거나 같고, B보다 작거나 같은 거의 소수가 몇 개인지 출력한다.

### 입력

첫째 줄에 왼쪽 범위 A와 오른쪽 범위 B가 공백 한 칸을 사이에 두고 주어진다.

### 출력

첫째 줄에 총 몇 개가 있는지 출력한다.

### 제한

- 1 ≤ A ≤ B ≤ 10$^{14}$

---

## 에라토스테네스의 체

- 에라토스테네스의 체는 **범위 내에 있는 소수를 구하는 알고리즘**으로, $O(N)$의 시간복잡도를 가진, 소수를 구하는 방법 중 가장 빠른 알고리즘이다. ~~당신이 이걸 고안해내지 않는 바에야 모르면 못 푼다. 못할 건 없겠지만 시간 낭비다.~~
- 어떤 한 수가 소수인지 아닌지 판단하는 소수 판별법과는 다르므로 하나의 수가 소수인지 판단하는 것은 나중에 살펴보자.

<figure style="width: 85%" class="align-center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif" alt="">
  <figcaption>에라토스테네스의 체</figcaption>
</figure>

### 🤔 알고리즘 설명

1. 소수도, 합성수도 아닌 유일한 자연수 1을 제외한다.
2. 2를 제외한 2의 배수를 제거한다.
3. 3을 제외한 3의 배수를 제거한다.
4. 4의 배수는 지울 필요 없으며(이미 지워짐), 5를 제외한 5의 배수를 제거한다.
5. 7을 제외한 7의 배수를 제거한다.
6. 그 다음 남아있는 11의 경우 11$**^{2}**$가 121로, 120을 넘어가므로 더 이상 제거할 필요가 없다.
    
> ❓ Q: 왜 11의 제곱부터 따지지?
>    
> A: 11 X 2, 11 X 3, … 11 X 10 은 이미 이전의 시행에서 모조리 다 지워졌기 때문에.
  
7. 남아있는 수들은 모두 소수이다.

- 그림에서 보듯이 체로 치는 것 같은 방법이고 발견한 고대 그리스 수학자인 에라토스테네스의 이름을 따서 “에라토스테네스의 체” 라고 부른다.
- 수학 쪽으로 관심이 있다면: 모듈러 & 소수 정리를 찾아 보면 될 것 같다.

### 소스 코드

```python
def eratosthenes(n):
		multiples = set()
		primes = []
		for i in range(2, n+1):
				if i not in multiples:
						primes.append(i)
						multiples.update(range(i*i, n+1, i)) # i*i부터 i씩 커지며 지워간다.
		return primes

print(eratosthenes(30)) # 30 까지의 소수를 구하여 출력한다.
```

이 코드는 에라토스테네스의 체 알고리즘을 통해 소수를 구해본 것 뿐이다. 문제 풀이에서는 이렇게 비효율적인 코드는 사용하지 않는다.

### 풀이

```python
import sys
input = sys.stdin.readline

A, B = map(int, input().split())

def eratosthenes(n):
    if n == 1:
        return []
    multiples = set()
    primes = []
    nearprimes = []
    for i in range(2, n+1):
        if i not in multiples:
            primes.append(i)

            a = i*i
            while a < n:
                nearprimes.append(a)
                a *= i

            multiples.update(range(i*i, n+1, i))
    return nearprimes

print(len(eratosthenes(B)) - len(eratosthenes(A)))
```

**⇒ 하하 그렇게 모조리 다 계산하면 메모리 초과가 뜬단다**.

### 개선

> 에라토스테네스의 체 알고리즘을 최적화하는 방법으로는 다음과 같은 것들이 있습니다.
> 
> 1. 홀수만 검사하기: 2를 제외한 모든 짝수는 소수가 아니므로, 3부터 시작하여 홀수만 검사하는 것으로 계산량을 줄일 수 있습니다.
> 2. 제곱근까지만 검사하기: 어떤 수의 약수는 그 수의 제곱근보다 작거나 같은 수 중에서 찾을 수 있습니다. 따라서 에라토스테네스의 체 알고리즘에서도 찾고자 하는 범위의 제곱근까지만 검사하면 됩니다.
> 3. 비트맵 사용하기: 에라토스테네스의 체 알고리즘에서는 각 수가 소수인지 아닌지를 저장해야 합니다. 이때 비트맵을 사용하면 메모리 사용량을 줄일 수 있습니다.
> 
> 이러한 방법들을 적용하여 에라토스테네스의 체 알고리즘을 최적화할 수 있습니다.
> - By Bing AI, generated in 2023/06/29
> 

🤔 하지만 여기에 더해, 비트 마스크를 사용해 풀 수 있는 방법이 있다. 이 경우는 매우 어려운 문제가 아니라면 사용할 필요는 없다: 구상이 어려움, 코드가 길어 시간 부족할 수 있음
    ⇒ (3) [비트마스크를 이용한 에라토스테네스의 체](https://www.crocus.co.kr/593)

이 블로그에서 정리한 에라토스테네스의 체 알고리즘 글들(C++)
- (1) [에라토스테네스의 체](http://www.crocus.co.kr/576)
- (2) [최적화된 에라토스테네스의 체](https://www.crocus.co.kr/805)
- (2)번 글에 잘 증명이 설명되어 있듯이, 주어진 A 까지의 소수를 구할 경우, A의 제곱근까지만 확인해 보면 된다: 합성수 K가 p*q라고 할 때 하나는 sqrt(K)이하, 하나는 sqrt(K)이상일 것이기 때문이다.
- 또한 연산을 진행하여 메모리 사용을 늘리는 대신 비트맵을 통해 과부하를 줄일 수 있다.

제일 위에서 설명한 코드는 그야말로 에라토스테네스의 체를 이용해 리스트를 뽑아내보는 기본적인 코드였을 뿐이다.(AI 사용이 서투른 듯) **에라토스테네스의 체 알고리즘은 메모리를 많이 차지하기 때문에 코드에서 조금이라도 메모리 사용이 높으면 메모리 초과가 발생한다.**

```python
import sys
input = sys.stdin.readline

A, B = map(int, input().split())

sqrtB = int(B ** 0.5)
primes = [True] * (sqrtB + 1)
primes[1] = False

for i in range(2, sqrtB + 1):
    if primes[i]:
        if (i * i) > sqrtB:
            break
        for j in range(i*i, sqrtB+1, i):
            primes[j] = False

cnt = 0
for i in range(1, len(primes)):
    if primes[i]:
        res = i * i
        while True:
            if res < A:
                res *= i
                continue
            if res > B:
                break
            res *= i
            cnt += 1

print(cnt)
```

풀이들이 정형화 되어있지는 않고, 위 셋 중 2번, 3번 방법을 참고하여 최적화한 코드들이 대부분. 그 중 한 가지를 참고하여 작성한 코드이다. 이 외에도 여러 코드들이 있는데 여러 코드들을 살펴보는 것도 사람들이 어떻게 생각하는지 엿볼 수 있는 기회인 듯.

#### 풀이 예시 1

```python
N = 10**7+5
sieve = [True]*(N+100)
sieve[0] = False
sieve[1] = False
sieve[2] = True
prime = 2

while prime**prime <= N:
		if not sieve[prime]:
				prime += 1
				continue
		for t in range(2*prime, N+3, prime): sieve[t] = False
		prime += 1

A,B=map(int, input().split())
cnt = 0
for p in range(2,N):
		if not sieve[p]: continue
		mul = p*p
		while mul <= B:
				cnt += (A <= mul)
				mul *= p
print(cnt)
```

#### 풀이 예시 2

```python
import math, sys
input = sys.stdin.readline

a, b = map(int, input().split())
sqrtb = int(math.sqrt(b))
A = [0] * (sqrtb + 1) # 소수가 아니면 0이다
A[2] = 2

for i in range(3, len(A), 2): # 홀수들만 남겨 둠
    A[i] = i

for i in range(3, sqrtb+1, 2): # 홀수들만 확인해 봄
    if A[i] == 0: continue # ? 왜 있지?
    for j in range(i+i, sqrtb+1, i): # 홀수들의 배수들 제거
        A[j] = 0

cnt = 0
for i in range(2, sqrtb+1):
    if A[i] != 0:
        temp = A[i] # 소수 추출
        while A[i] <= b/temp:
            if A[i] >= a/temp:
                cnt += 1
            temp = temp * A[i]
print(cnt)
```