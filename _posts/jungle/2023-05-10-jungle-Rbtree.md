---
title: "RB Tree에 대해 알아보자."
excerpt: "RB Tree 개념 정리"

categories:
  - Jungle
tags:
  - RB Tree
---
RB Tree: 이진 탐색 트리(Binary Search Tree)의 일종이다.

다만, 원래 이진 탐색 트리에 있었던 문제를 해결하기 위해 Red, Black이라는 속성을 추가하여 데이터를 관리한다 → 이진 탐색 트리는 최악의 경우 탐색 시에 트리의 높이만큼 시간이 소요되었다: $O(log(n))$ [평균] → $O(n)$ [최악]

![Binary Tree: 최약의 경우](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQSaUNt9lwZ-R60J1qBBJPEMAYqoSEEACcI_9E6cBcJWAT0?width=1024)

RB트리는 데이터의 입/출력 시에 적절한 규칙으로 조절하여 이러한 경우가 발생하지 않도록 보완한 이진 탐색 트리이다. RB 트리는 다음과 같은 규칙을 정하여 이진트리를 관리한다.

1. 모든 노드는 빨간색/검은색 둘 중 하나이다.
2. Root Property: 루트 노드는 검정색이다.
3. External Property: 모든 리프 노드(NIL)는 검정색이다.
4. Internal Property: 어떤 노드가 빨간색이라면, 그 자식 노드는 모두 검정색이다. -즉, 빨간색 노드가 연속으로 발생하지 않아야 한다.-
5. Depth Property: 루트 노드에서 모든 리프 노드까지의 검정색 노드 갯수는 모두 동일하다(루트 노드는 제외, 일명 Black Height).

참고해 볼 만한 블로그 및 사이트들: 블로그: [1)](https://velog.io/@whddn0221/%EB%A0%88%EB%93%9C%EB%B8%94%EB%9E%99%ED%8A%B8%EB%A6%AC-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0) / 레포지토리: [2)](https://github.com/jwowo/rbtree-lab/blob/main/src/rbtree.c) [3)](https://github.com/e-juhee/Red-Black-Tree) 

AVL Tree와 RB Tree의 차이점에 대해 이해해 두는 것이 좋다.