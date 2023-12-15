---
title: "포인터와 배열, 포인터 연산"
excerpt: "공개 레포지터리에 올리지 말아야하는 파일을 push한 경우에는 어떻게 해야하지?"

toc: true
toc_sticky: true

categories:
  - Git
tags:
--- 
## 문제 발생

프로젝트를 다시 빌드해보는 과정에서 application.yml을 파일이 업로드하고 말았다. 당황한 나머지 해당 파일을 삭제하고 다시 push 했지만 저장소 커밋 로그에는 그대로 남아 있는 상황. roadmap 브랜치에서 PR 후 머지까지 해 버린 것이다.

 
## 상황

1.  application.yml에는 db 접속 키와 gpt api 키가 있었는데, Github에서 제공하는 submodule  기능을 사용해서 공개 리포지터리에서 분리시켜 보조 리포지터리(당연히 private)에 저장하여 해결하였다.
2. 팀프로젝트로 진행할 때는 resources 디렉터리를 전부 submodule로 등록하여 빌드 시에 해당 디렉터리를 submodule repository로부터 불러와서 빌드하도록 처리하였다.
3. 개인 리포에서는 좀 더 세밀하게 다루고 싶어 resources 디렉터리를 건드리지 않고 (차후에 생길지도 모르는) 다른 비공개 파일들을 관리 할 수 있도록 submodule repository에는 파일만 저장하였다.
4. 그리고 build.gradle 파일에 다음과 같이 설정하여 빌드시에 secret 폴더(submodule의 파일들을 저장한 폴더)에 있는 application.yml을 resources 폴더에 복사하도록 하였다.
5. .gitignore에 진작 등록을 해놓지 않아 로컬에서 빌드 하고 application.yml을 지우지 않고 원격 레포에 커밋하면서 문제가 발생하였다.

```yml
task copySecret(type:Copy) {
	copy {
		from './secrets'
		include "*.yml"
		into 'src/main/resources'
	}
}
```
 
## 해결

출처의 내용들을 참고하면 훨씬 더 자세하게 설명되어 있다. application.yml을 푸시한 커밋에서 application.yml 만 제거해서 다시 커밋하고자 하였다.

되돌아가야 하는 커밋 해시값을 확인하기 위해 git log 명령어를 통해 기록을 확인하였다.
로컬 저장소의 변경은 남겨두고, 원격 저장소의 커밋만 제거하기 위해 soft reset 을 하기로 하였다: main 브랜치와 roadmap 브랜치 모두에서 실행함.
- `git reset <Commit Hash>`
- `git push -f origin <branch name>`

하지만 이렇게 할 경우 reset --mixed 옵션을 사용하므로, 작업 내용이 변경되지 않고 인덱스만 해당 커밋의 상태로 변경한다. 이 경우,
- 로컬에서는 .git > object에는 커밋이 진행되는 동안 모든 파일의 내용이 계속 저장되고 있다.
- 원격에서는 여전히 해시 커밋을 통해 접속해보면 PR이 그대로 남아있는 것을 확인할 수 있다.

로컬의 파일들은 참조 기록(Reference Log)을 삭제한 뒤 가비지 컬렉터(Garbage Collector, GC)를 사용하여 불필요한 개체를 자동으로 삭제하면된다:
- `expire --all --expire=now` 이전의 모든 참조 기록을 만료시키는 명령어
- `git gc --prune=now` 불필요한 개체 삭제

원격 파일들은 github에 문의를 보내 PR을 삭제하도록 문의하였다[Github 문의](https://support.github.com/request). 처음 문의를 완료하면, 메일로 발급해준 티켓을 받을 수 있는데, 여기에 추가 문의를 하거나 처리 결과, 메세지 등을 확인할 수 있다.

### reset 명령의 옵션

`--soft`: uncommit changes, changes are left staged (index).

`--mixed (default)`: uncommit + unstage changes, changes are left in working tree.

`--hard`: uncommit + unstage + delete changes, nothing left.

## 출처
[Github PR 제거하기](https://sunnn21.tistory.com/78)

 
### Git의 Reverse & Revert

[version control - What's the difference between git reset --mixed, --soft, and --hard? - Stack Overflow](https://stackoverflow.com/questions/3528245/whats-the-difference-between-git-reset-mixed-soft-and-hard)

[https://wooono.tistory.com/667](https://wooono.tistory.com/667)

[앗! 모르고 깃헙(Github)에 올렸어요! - medium](https://medium.com/daangn/%EC%95%97-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EA%B9%83%ED%97%99-github-%EC%97%90-%EC%98%AC%EB%A0%B8%EC%96%B4%EC%9A%94-50d48b343f0f)

 
### 참고: BFG를 사용하여 민감한 자료 숨기기

[BFG를 사용하여 민감한 자료를 숨기기](https://docs.github.com/ko/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository#github%EC%97%90%EC%84%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%99%84%EC%A0%84%ED%9E%88-%EC%A0%9C%EA%B1%B0)