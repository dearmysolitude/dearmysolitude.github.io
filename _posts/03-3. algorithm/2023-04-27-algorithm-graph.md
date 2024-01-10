---
title: "Graph"
excerpt: "그래프"

toc: true
toc_sticky: true

categories:
  - Algorithm
tags:
  - Graph
  - 그래프
---
## 그래프에서 알아둘 것들

1. 그래프의 자료구조에 대한 이해: dense graph / sparse graph에 대해 자료 구조를 적용할 수 있어야 함
2. 작동 방법: **BFS / DFS**
3. 그래프를 이용한 알고리즘의 이해
    
[Dijkstra & Bellman-Ford](https://www.notion.so/Dijkstra-Bellman-Ford-d478400531904a5d9eb1fc4ece2869eb?pvs=21): 최단 경로 찾기

[MST](https://www.notion.so/MST-5fea75bb7bfa44d3a4f6a2d18d279de9?pvs=21): 최적 경로 산출
    

## List / Tree / Graph

- List: One predecessor , oen successor at most
- Tree: One predecessor(parenet), several successors(childrens)
- Graph: ordered collection, but several predecessors and successors: (바이너리) 트리와 달리 노드가 가리킬 수 있는 갯수가 2개로 정해져 있지 않고 마음대로 참조하게 됨→ 어떻게 traverse 할 수 있을 것인가?

Graph G = (V, E)

V = { $v_i$ }: a finite non-empty set of vertices(or nodes)

E = { $e_i$ }: a finite(possibly empty) set of edges (or arcs)

$e_i$ connects two vertices in V

용어: Unabled vertices: 노드들이 무명인 / Labeled vertices:노드들에 이름 붙음 / Labled vertices and weighted edges: 노드에 라벨되어 있고 edge들이 weighted되어 있음

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216756&authkey=%21AAnXB9Ur0EHcX30&width=976&height=228" alt="">
  <figcaption>graph 1</figcaption>
</figure>

## 용어

Adjacent, neighbor

path between A and B: A와 B를 잇는 

Connected graph

Connected Component: graph subset containing the set of vertices reachable from a vertex and their edges

Complete Graph: edge for every pair of vertices

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216754&authkey=%21APcSw5fLM8vF_0Q&width=1185&height=331" alt="">
  <figcaption>graph 2</figcaption>
</figure>

Cycle: a path starting from a node and ending the node itself

Directed Edge: an edge with direction(source and destination)

**Digraph:** Directed graph

**DAG: Directed Acyclic Graph << machine learning**

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216755&authkey=%21AIDYQsN4PyQSCic&width=1174&height=278" alt="">
  <figcaption>graph 3</figcaption>
</figure>

## DFS Depth-First Search & BFS Breadth-First Search

- DFS: 깊이 우선 탐색. 최대한 깊이 내려간 뒤, 깊이 갈 곳이 없을 경우 옆으로 이동한다. 재귀나 stack을 이용하여 구현
- BFS: 너비 우선 탐색.

## 1260. DFS와 BFS

```python
import sys
from collections import deque

N, M, V = map(int, sys.stdin.readline().split())
graph = [[False]*(N+1) for i in range(N+1)] #2차원 배열
visited = [False]*(N+1)

for i in range(M):
    a, b = map(int, sys.stdin.readline().split())
    graph[a][b] = graph[b][a] = True

# print(visited)
# print(graph)

def dfs(x):
    if visited[x] == False:
        visited[x] = True
        print(x, end=' ')
        for i in range(1, N+1):
            if graph[x][i] == True: #길이 있다면
                dfs(i)
#길이 있다면 재귀로 들어가 갔는지 확인하는 플래그를 true로 바꾼다.

def bfs(x):
    queue = deque([x]) #큐를 사용함: 루트에서 갈 수 있는 모든 노드를 큐에 인큐시킬것이다.
    visited[x] = False
    while queue: # 큐가 있는 경우 계속 실행
        x = queue.popleft() # 큐에서 값을 뽑아서
        print(x, end=' ') #인쇄
        for i in range(1, N+1): 
            if visited[i]==True and graph[x][i] == True: #만약 하위 노드들 중 방문하지 않고 길이 있는 경우
                visited[i]=False # 방문으로 체크하고
                queue.append(i) # 큐에 인큐한다
dfs(V)
print()
bfs(V)
```

## Data Structure for graphs

**How to store data for graph?**

→ Store a set of vertexes: 0, 1, 2, 3, 4, 5, … + Store a set of edges: (0,1), (1, 3), …

Storing vertexes: **Linked list, BST, Hash**

- 연결된 것들만 기록하기 때문에 해당 노드에 인접한 노드를 바로 알 수 있다. 메모리 할당이 적다.
- 두 노드의 연결 여부는 한눈에 알기 어렵다

Sore edges: Fundamentally, a pair of values

- **two-dimensional matrix** / Space: $O(V^2)$, Time: $O(1)$
- source, destination에 대한 value를 저장하는 방식, 값이 있으면 1, 없으면 0
- Dense vs. Sparse: dense일 경두
    
    → **adjacency list**: Space: $O(E)$ Time: $O(E)$두
    
- 두 노드간 간선 존재 여부를 바로 알 수 있으나 노드가 많을수록 빈 메모리 공간을 더 많이 필요로 한다.

$Density = {|E| \over |V|(|V|-1)}$ ← directed graph에 대한 density, undirected의 경우에는 vertices의 방향성을 무시하므로 분모가 절반이 될 것이다. Spars 할 수록 0으로, Dense할 수록 1로 간다.

## 그래프 탐색: Basic

### 1991. 트리 순회

분류: 트리, 재귀

```python
import sys
N = int(input())
tree = {}
for i in range(N): ####딕셔너리####
    root, left, right = sys.stdin.readline().rstrip().split()
    tree[root] = [left, right]
def preorder(root):
    if root != '.':
        print(root, end='')
        preorder(tree[root][0])
        preorder(tree[root][1])
def inorder(root):
    if root != '.':
        inorder(tree[root][0])
        print(root, end='')
        inorder(tree[root][1])
def postorder(root):
    if root != '.':
        postorder(tree[root][0])
        postorder(tree[root][1])
        print(root, end='')
preorder('A')
print()
inorder('A')
print()
postorder('A')
```

### 5639. 이진 검색 트리

분류: 재귀, 트리, 그래프 탐색, 그래프 이론

```python
import sys
sys.setrecursionlimit(10**9)
input=sys.stdin.readline

arr = []
while True:
    try:
        arr.append(int(input()))
    except:
        break

def sol(start, end):
    if start > end:
        return
    mid = end + 1 # 모든 노드가 작을 경우를 위하여
    for i in range(start+1, end+1): # start는 판단 기준이 된다.
        if arr[start] < arr[i]:
            mid = i
            break
    sol(start+1, mid-1)
    sol(mid, end)
    print(arr[start])
sol(0, len(arr)-1)재귀 재ㄱ
```

## 참고 자료
[Kaist Open Course: 데이터 구조 및 분석 - 문일철 교수](https://kooc.kaist.ac.kr/datastructure-2019s2/lecture/40350?isDesc=false)