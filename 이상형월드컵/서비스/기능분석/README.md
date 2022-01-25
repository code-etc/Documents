# 기능분석

## Usecase Diagram

![dd](https://www.plantuml.com/plantuml/png/bLN1RjD04BtlLunyIW_95rHLRvoGkF01KMfAH6cGipqWg6Gr6uJ0XqeXA29nv8fIeAg4sxYgA_f5k-D_ODsCqPfrus15KVBclTcPoTi9s-xpjiSFNkwnFVSP1zu3hxlxdCDErtCxlDlRPwpTuJqFd4S7RfyxnRUdVTTps3q6cs-RJSW1nbwrEo_QkouuA6RgLk27KmVUC0Rw-HkILm7E1U7Bfw7EVwgJK2qIcSORiDbgGGFZLCR-Xa4YP7bU0wUVr6Y6lw-1y8U8Al3nOuJHE1id-FLEds0fx6JGQcr18viANHFEZm55b0r2Gs5dhPCMMZj64daLoyKT3O6PqnbTwkdOZHDk4Hzit59OoQpgBOMTTRMqkl6I1CD4dOa2Mul-AyNl_jgAR32JDsc9VdZ_NdPczYN0wR5CV7LsKVOdlWjmE0DrakV1C2r6BAylPEeRUBq4RvFyDJcjKDa9gG6gBFPnFZJOHgvL0gCHueq5JWSc_lZr4pq2kdfQew9lsLXN7upoSiYJrNHoNKuttBHIiLm162RwcBhaHmdWpO_iSmsHEjQ-r3J0znFzoMomAamdB8Gw7nROMegZYQxzVnJoCjPt9RTi8I8J7Z9QeitcrZzhq0oKLqE-Qqb0ZYuGstPcpFpnJPpcoDYskx-Z_ml-0000)

<details>
    <summary>
    PlantUML 소스코드
    </summary>

```plantuml
@startuml
left to right direction

actor "Guest"
actor "User"
Guest <|-- User

package "서비스" {

    package "게임" {
        (목록보기) <.. (참가)

        (참가) <|-- (월드컵 참가)
        (참가) <|-- (대신정해주기 참가)
        (월드컵 참가) ..> (후보자 선택)
        (대신정해주기 참가) ..> (후보자 선택)

        (후보자 선택) ..> (결과보기)


        (등록) <|-- (월드컵 등록)
        (등록) <|-- (대신정해주기 등록)
        (월드컵 등록) ..> (후보자 등록)
        (대신정해주기 등록) ..> (후보자 등록)

        (후보자 등록) ..> (이름 등록)
        (후보자 등록) ..> (이미지 등록)
        (후보자 등록) ..> (태그 등록)
    }
    
    package "회원관리" {
        (소셜 로그인) <|-- (구글 로그인)
        (소셜 로그인) <|-- (카카오 로그인)
        (구글 로그인) ..> (회원가입)
        (카카오 로그인) ..> (회원가입)

        (로그아웃)

        (MyPage) <.. (입력한 댓글 보기)
        (MyPage) <.. (등록한 월드컵 보기)
        (MyPage) <.. (게임이력 보기)
        (MyPage) <.. (내 취향 보기)
        (MyPage) <.. (회원정보 수정)

        (회원정보 수정) <.. (별명 수정)
        (회원정보 수정) <.. (나이 수정)
        (회원정보 수정) <.. (거주지 수정)
    }
}

Guest --> (목록보기)
Guest --> (결과보기)
User --> (등록)
User --> (소셜 로그인)
User --> (로그아웃)
User --> (MyPage)

@enduml
```

</details>

## Flowchart Diagram

### 로그인

> - OAuth2 로 진행된다.
> - OAuth2 은 기본적인 정보활용 동의를 구한다.
> - IDP(IDentity Provider)의 제공정보를 회원정보로 활용한다.

```mermaid
flowchart LR
    a1([시작])
    a2[소셜 로그인]
    a3{기존 정보확인}
    a4[회원정보 등록]
    a5[/토큰 발급/]
    a6([완료])

    a1 --> a2
    a2 --> a3
    a3 -->|없음| a4
    a3 -->|있음| a5
    a4 --> a5
    a5 --> a6
```

### 게임 등록

> - __월드컵/대신정해주기__ 는 게임정보(주제 등)와 후보자를 등록한다.
> - 후보자는 이름/태그/이미지를 갖게 되며, __2개 이상의 후보자__ 등록이 요구된다.
> - 태그 등록시 편리성을 위해 추천기능을 제공한다.
>   - 월드컵 __카데고리별 연관성__ 추천
>   - __낱말 완성__ 추천

```mermaid
flowchart LR
    a1([시작])
    a2[주제 등록]
    a31[이름 등록]
    a32[태그 등록]
    a33[이미지 등록]
    a4{추가 등록}
    a5{후보자 수 >= 2}
    a6([완료])

    a1 --> a2
    a2 --> a3
    subgraph a3[후보자 등록]
        direction LR
        a31 --> a32
        a32 --> a33
    end
    a3 --> a4
    a4 -->|Yes| a3
    a4 -->|No| a5
    a5 -->|Yes| a6
    a5 -->|No| a3
```

### 게임 진행

> - 게임진행 시 프론트는 다음 형식의 데이터를 사용한다.
> - 대신정해주기 게임의 경우, __1회성 문답만__ 진행한다.
> - 이미지는 request traffic 을 고려하여 __base64__ 방식으로 전달한다.
> - __HATEOAS__ 방식으로 다음 요청주소를 제공한다.

```json
{
    "ID-A" : {
        "name" : "후보A",
        "picture" : "base64 data",
        "tagList" : {
            ...
        }
    },"ID-B" : {
        "name" : "후보B",
        "picture" : "base64 data",
        "tagList" : {
            ...
        }
    },
    "next-match" : "next url"
}
```

```mermaid
flowchart LR
    a1([시작])
    a2[게임선택]
    a3[후보자 매치]
    a4{다음 후보자}
    a5[게임결과]
    a10([완료])

    a1 --> a2
    a2 --> a3
    a3 --> a4
    a4 -->|Exist| a3
    a4 -->|None| a5
    a5 --> a10
```

### 게임 결과

> - 후보자별 통계순위를 제공한다.
> - 댓글 목록/등록을 제공한다.
>   - 댓글은 로그인(Oauth)을 요구한다.

### 그 외

#### 회원정보 수정

> - 수정은 RESTful 방식의 __PUT__ 을 준수한다.
> - Validation 은 프론트 & 백엔드 __두번의 절차로 검증한다.__
