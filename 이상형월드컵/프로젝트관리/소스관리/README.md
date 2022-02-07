# 프로젝트 소스관리

## 브랜치 관리

### Git-flow

- 기본전략은 [Git-flow](https://techblog.woowahan.com/2553/) 를 참조한다.
  - Git-Flow 전략에 대한 정의는 관리방법 [제안자](https://github.com/orgs/code-etc/people/yunjin-kim)의 다음 의견을 수용한다.

> 1. develop 브랜치에서 기능별로 브랜치 만들어서 개발하고  release 브랜치로 merge 하고 기능 개발 끝난 branch는 삭제하는 방식입니다.
> 2. main 브랜치에는 마지막에 기능개발 전부 완료 merge합니다.

- 그 외 요구 항목은 다음과 같다.
  - 기본적으로 브랜치 생성은 깃헙의 이슈관리로 생성한다.
  - 이는 해당 이력과 이슈에 대한 연결방식을 통일해준다.
  - 추후 필요에 따라 새로운 네이밍을 적용한다.
  - 소스병합은 `Pull request` 로 진행하며, 구성원 `code review` 와 이에 대한 __1인 이상의 `Approved`__ 를 필요로 한다.

## 버전 관리

> Versioning 전략은 프로젝트의 규모나 상황에 따라 선택적으로 사용할 수 있다. 일반적인 적략은 [Semver](https://semver.org/), [Calver](https://calver.org/) 가 존재한다. 그 외에도 __4-step__ 인 `Num.Status`, 고전방식인 `Num 90+` 이 있다.

### Project source

> 일반적으로 Versioning 은 `Major`.`Minor`.`Patch` 인 `Semver` 가 가장 접하기 쉽다. 하지만, 각 분류에 대한 정의는 프로젝트 규모 또는 전략에 따라 차이점이 존재할 수 있다. 이에 대해 기본구성은 `Semver` 형식을 사용하되, 각 단계별 분류방식은 개인의 Versioning 전략을 존중하기로 했다.

|참여자|X|Y|Z|비고|
|-|-|-|-|-|
|윤진|Component|Feature|BugFix/Design|3-Step|
|정한|Refactor|Feature|-|2-Step|
|대환|Release|Feature|-|2-Step|
|석찬|Release|Feature|-|2-Step|

> 가장 큰 차이는 __2 vs 3-step__ 이다. 여기서 `2-step` 의견은 예상된 서비스제품이 아니기에 __Overwrite__ 하는 방식이다.

### Docker Imange

> - `X.Y` 인 __2-step__ 버전분기 방법을 사용한다.
>   - 예외적으로 `X.Y-Optional` 형식으로 이미지를 제공할 수 있다.
