---
title: "내 C/C++ 개발 환경: WSL2"
excerpt: "Visual Studio가 너무 무거워서."

toc: true
toc_sticky: true

categories:
  - C&C++
tags:
  - 개발 환경
---
> Krafton Jungle과 KRISS에서 배운 알고리즘과 자료구조, CS 관련 내용의 실습을 위해 C/C++ 개발환경 구축 한 내용을 정리하였다. Windows 11, WSL2 , Ubuntu 22.04, Visual Studio Code, GDB 사용.

Windows에서 1.Visual Studio를 사용하거나 2.AWS EC2 서비스를 개발환경으로 사용할 수도 있지만

1.  의 경우, 무거운 VS를 사용하기  싫었고, 윈도우 시스템 엔지니어가 될 것도 아니고, C/C++로 대규모 프로젝트를 할 것도 아니고, 아무래도 보편적으로 서버로 사용하는 Linux 환경의 개발을 하는 것이 나아보인다.
2.  나는 지금 나가는 AWS 나가는 돈도 아까운데... 나한텐 커피값이 더 귀함....ㅎ

GDB의 사용이 아직 익숙하지는 않지만 그런대로 빌드하고 사용하는데에는 크게 문제는 없는것 같네...

## 참고 자료

[Get Started with C++ and Windows Subsystem for Linux in Visual Studio Code](https://code.visualstudio.com/docs/cpp/config-wsl)