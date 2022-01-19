# 프로젝트 소스관리

## 버전관리/브랜치 전략

- 기본전략은 [Git-flow](https://techblog.woowahan.com/2553/) 를 참조한다.
  - Git-Flow 전략에 대한 정의는 관리방법 [제안자](https://github.com/orgs/code-etc/people/yunjin-kim)의 다음 의견을 수용한다.

> 1. develop 브랜치에서 기능별로 브랜치 만들어서 개발하고  release 브랜치로 merge 하고 기능 개발 끝난 branch는 삭제하는 방식입니다.
> 2. main 브랜치에는 마지막에 기능개발 전부 완료 merge합니다.

- 그 외 요구 항목은 다음과 같다.
  - 기본적으로 브랜치 생성은 깃헙의 이슈로 관리로 생성한다.
  - 이는 해당 이력과 이슈를 연결방식을 공유를 목적으로 한다.
  - 추후 필요에 따라 새로운 네이밍을 적용한다.
  - 소스병합은 `Pull request` 로 진행하며, 구성원 `code review` 와 이에 대한 __1인 이상의 `Approved`__ 를 필요로 한다.
