---
title: "Github - Commit Convention"
excerpt: "팀프로젝트에서 사용할 commit convention을 살펴보자. 실제로 적용에는 연습이 필요할 듯."

categories:
  - Roadmaker
  - Git
tags:
  - Git
---
팀 프로젝트에서 팀원들과 사용할 커밋 컨벤션을 정리해보았다. 

## 커밋 메세지 규약
AngularJS를 살펴보자.

- [Git - 커밋 메세지 규약 정리](https://velog.io/@outstandingboy/Git-%EC%BB%A4%EB%B0%8B-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B7%9C%EC%95%BD-%EC%A0%95%EB%A6%AC-the-AngularJS-commit-conventions)

## 커밋 잘하기
- **하나의 액션은 커밋 메세지 한 줄**: commit 단위에 대해 생각해보자. 기본 원칙은 '1 Action, 1Commit'. 추후에 혹시 모를 revert등을 통한 롤백을 쉽게 하기 위함이다.
    - 합칠 순 있지만 나눌 순 없다: 커밋은 합칠 수 있지만 나눌 수 없으므로, 가장 좋은 선택은 잘게 나누고 나중에 합치는 것이다.
- **전역으로 커밋하지 않기**: 예를 들어, '말 줄임표 처리'를 프로젝트 전역에서 한 것을 한 번에 커밋하는 것보다, '유저 정보 페이지에서 말 줄임표 처리'등과 같이 처리하여 전역에서 한 번에 커밋하지 않도록 한다: revert 시킬 때 너무 많은 코드들이 영향 받기 때문
- **커밋 시점**
    1. 일하는 흐름대로 커밋
        A 파일에서 A1코드의 버그를 수정, B/C파일의 코드를 고쳤다. A2 코드의 최적화를 위해 리팩터링 후 D/E파일의 코드를 수정한 경우, A,B,C,D,E파일의 수정을 커밋하고 메세지를 다음과 같이 설정: "A컴포넌트의 XX버그를 고치고, A컴포넌트의 YY한 부분을 리팩토링"
        - pros: 집중을 깨트리지 않고 일을 연속적으로 처리 - 생산성 / 효율 증가
        - cons: 코드를 되돌려야 하는 경우, 되돌리기가 쉽지 않다
    2. 액션대로 커밋
        위와 같은 상황에서 버그만 처리한 시점에서 커밋: "A컴포넌트의 XX 버그 수정"과 "A컴포넌트의 YY한 부분을 리팩토링"으로 커밋을 두 번 한다.
        - 장단점은 위 경우와 반대이다.

    결국 두 경우에 대해서 적절한 절충점을 찾아 커밋하는 것이 생산성이나 코드 유지보수에 도움이 된다.

## 버그 발생한 커밋 찾기

`git bisect`는 버그가 발생했을 때 원인을 찾는 과정에 유용한 기능 중 하나이다. 일반적으로 앱 기능 테스트 중 버그가 발생한 경우, 의심되는 코드의 위치에서, 

1. 브레이크 포인트를 걸고 디버깅
2. git log를 통해 히스토리 확인

을 통해서 대부분 해결할 수 있으나, 이러한 방법으로 해결이 어려운 경우도 있다. 그 때에는 문제를 일으킨 커밋을 찾아 코드 변경 내역을 찾아내는 것이 편리하다.

### `git bisect`
커밋의 특정 범위 내에서 이진 탐색을 통해 문제가 발생한 최초 커밋을 찾는데 도움을 주는 git의 기능.

1. 문제 commit을 찾아 Hash 값을 기록한다. `1d45c75`
2. 발생되지 않는 commit을 찾아 Hash 값을 기록한다. `4b58458`
3. 다음과 같이 입력하면,
    ```zsh
    git bisect start
    git bisect bad 1d45c75
    git bisect good 4b58458
    ```
    이런 문구가 출력된다.
    ```zsh
    Bisecting: 7 revisions left to test after this (roughly 3 steps)[1c0b7a04ea7802037ec1b1ed0d98ecbfc5f5747d] Commit Message~
    ```
    이는 bad / good 커밋 사이에 7 개의 커밋이 있고 bisect를 3 번 수행하면 문제의 커밋을 찾을 수 있다는 의미이다. 그 아래에는 bisect를 수행할 첫 커밋으로 checkout되면서 커밋의 정보가 출력된다.

4. 이 커밋에서 문제가 발생하는지 확인한다.
이 때 문제가 발생하면 `git bisect bad`를, 발생하지 않으면 `git bisect good`을 입력하여 문제의 commit을 찾는다.

5. 첫 문제의 커밋을 찾으면 다음과 같이 출력된다: 
```shell
ccd1a6daca3bf01df7fda12ace86f5d518e9831f is the first bad commit
```

6. bisect 과정이 끝나면 아래 명령어로 초기화 해주어 bisect를 처음 시작했던 commit으로 checkout 된다.
```shell
git bisect reset
```

## 참고
- [커밋을 잘게 쪼개자 - 커밋은 언제 하는 것이 가장 좋을까?](https://jaeheon.kr/257)
- [버그가 발생한 커밋 찾기(git bisect)](https://simsi6.tistory.com/97)