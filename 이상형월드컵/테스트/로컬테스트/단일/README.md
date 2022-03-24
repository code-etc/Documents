# 단일 Docker 이미지 테스트

### 사용한 이미지
* tjwjdgks43/code-etc-authserver:v.0.3
* tjwjdgks43/code-etc-gateway:v.0.0
* tjwjdgks43/code-etc-beta:discovery.0.0
* mysql:latest
```
요구 사항 

auth 서버 포트 8080:8080
gateway 서버 포트 8888:8080 (기존 8080포트에서 8888 포트로 변경했습니다)
discovery 서버 포트 8761:8761
mysql db 포트 3306:3306

account 서버 포트 8081:<>(외부 포트는 8081 강제)
game 서버 포트 8082:<>(외부 포트는 8082 강제)
```

### oauth2.0 url 예시
```
http://localhost:8080/oauth2/authorize/{provider}?redirect_uri=http://localhost:8888/auth/home

provider
  -kakao
  -google
  
authorizedRedirectUrls
 - http://localhost:8080/auth/home
 - http://localhost:8888/auth/home
```

### gateway 서버

discovery 서버
> localhost:8888/registry : eureka 서버에 등록된 서버 목록을 볼 수 있다
> 
> localhost:8888/eureka/** 

account 서버
> localhost:8888/accounts/**  

auth 서버
> localhost:8888/auth/** 
> 
> localhost:8888/oauth2/**

game 서버
> localhost:8888/games/**
> 
> localhost:8888/matches/**
> 
> localhost:8888/plays/**
> 
> localhost:8888/rounds/**
