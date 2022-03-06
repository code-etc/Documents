# Account Microservice

## Deployment

### A. Profile

> StrangeBrother Resource service use spring `Profile`

| profile     | usage        | note                                      |
|:------------|:-------------|:------------------------------------------|
| local       | private      | not supported for deploy                  |
| development | docker image | API integration of microservices in local |

### B. API integration

#### API integration of microservices in local

1. Run __[MySQL](#Datasource)__ (required by resource server)
2. Run __Account-service__ (as development image)

```sh
# image : putstack/account-service
# version : semantic versioning (=Semver), (ex, 0.0.1)
# profile(optional) : putstack/{image}:{version}[-profile], default is development, (ex. putstack/account-service:0.0.1-development)
docker run -d -it --name account-service -p 8080:8080 putstack/account-service bash
```

> note : development profile, host format is `host.docker.internal`

3. Test in Document (Not support, yet)

## Architecture

<details>
	<summary>Class Diagram</summary>

```
classDiagram
    class Aspect { }
    Aspect <|.. LogAspect
    class LogAspect { }
    LogAspect <|-- ServiceLogAspect
    class ServiceLogAspect { }

    class AccountController {
        AccountService accountService
        ResponseEntity retrieve(Long id)
        ResponseEntity retrieve(String username)
        ResponseEntity update()
        ResponseEntity signUp()
        ResponseEntity withdraw()
    }

    AccountController --> AccountService
    class AccountService {
        <<Interface>>
    }

    AccountService <|.. AccountServiceImpl
    class AccountServiceImpl {
        AccountRepository accountRepository
        AuthorityUtil authorityUtil
        Account query(Long id)
        Account query(String username)
        Account update(Account account)
        Account create(Account account)
        boolean delete(Long id)
        boolean delete(String username)
    }

    AccountServiceImpl --> AuthorityUtil
    class AuthorityUtil {
        boolean verify()
    }

    AuthorityUtil ..> HttpClient : token verify
    class HttpClient { }

    HttpClient <|.. AuthorityClient
    class AuthorityClient { }
    
    JpsRepository <|-- AccountRepository
    AccountServiceImpl --> AccountRepository
    class AccountRepository {
        Account query(Long id)
        Account query(String username)
        Account update(Account account)
        Account create(Account account)
        boolean delete(Long id)
        boolean delete(String username)
    }

    AccountRepository ..> Account
    class Account {
        Long id
        String username
        String password
        Integer age
        Address address
        IDP idp
        getter()
        setter()
    }

    Account *-- Address
    class Address {
        Long id
        address district
        getter()
        setter()
    }

    Account *-- IDP
    class IDP {
        Long id
        int code
        int name
        getter()
        setter()
    }
```

</details>

## Environment

### Datasource

- MySQL Docker 사용

```sh
# MYSQL_DATABASE :: a database to be created on image startup
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=<비밀번호> -e MYSQL_DATABASE=test mysql:latest
```

```yaml
# Use `jdbc:mysql` protocol
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url:  jdbc:mysql://localhost:3306/test
    username: root
    password: <비밀번호>
  jpa:
    hibernate:
    #  ddl-auto: create-drop
    #    show-sql: true
    properties:
      hibernate:
        format_sql: true
```

## Document

### Swagger UI of `Springdoc-openapi`

- [http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html)
