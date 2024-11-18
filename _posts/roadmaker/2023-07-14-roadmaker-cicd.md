---
title: "Roadmaker 배포 환경 구축"
excerpt: "Blue/Green 배포 파이프라인 구축하기"

categories:
  - Roadmaker
tags:
  - CI/CD
---
## CI/CD
CI/CD는 지속적인 통합, 배포의 약자이다. 이 두 용어는 개발에 있어서 종종 함께 사용되며, 중요한 개념이다.

- **지속적인 통합CI**
    - 개발자가 코드 변경 사항을 자주 통합하는 것
    - 변경 사항은 자동화된 빌드 및 테스트 프로세스를 거쳐 메인 코드 브랜치에 병합
    - 변경사항이 다른 부분에 영향을 미치는지 확인하고, 문제가 발생할 경우 즉시 수정 가능
- **지속적인 배포CD**
    - CI의 확장
    - 코드 변경 사항이 자동으로 프로덕션 환경에 배포되는 것
    - 개발자는 사용자에게 새로운 기능과 버그 수정 신속하게 제공

> **한마디로 배포 환경 한번 구축으로 버그 수정 등 배포된 코드에 대한 관리가 매우 쉬워짐**

## Blue/Green 무중단 배포 구현하기
다음 글이 참고가 많이 되었다: 감사합니다.
- [Github Actions + CodeDeploy + Nginx로 무중단 배포하기](https://wbluke.tistory.com/39)
- [자동 배포 및 HTTPS 통신](https://jonguk.tistory.com/entry/Spring-Boot-AWS-CodeDeploy-GitHub-Actios-%EC%9E%90%EB%8F%99-%EB%B0%B0%ED%8F%AC-%EB%B0%8F-HTTPS-%ED%86%B5%EC%8B%A0-EC2%ED%8E%B8)
- [Nginx 이해하기](https://whatisthenext.tistory.com/123)
- [블루/그린 무중단 배포](https://www.devjoon.com/56)

## 배포 구현과정 중 발생한 문제들(...ing)
