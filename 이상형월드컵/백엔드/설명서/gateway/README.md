## Gateway 서버

----
### docker-compose  

중앙 서버가 없기 때문에 팀원간의 실행 환경을 맞추기가 어려운 문제가 있었습니다. <br>
또한 다수의 서버가 존재 했기 때문에 쉽게 관리하기 위해 docker-compose를 사용했습니다

백엔드 서버를 모아 놓은 docker-hub repo 입니다
( [docker hub url](https://hub.docker.com/repository/docker/tjwjdgks43/code-etc-beta) )<br>

현재 사용하는 이미지 (prefix = tjwjdgks43/code-etc-beta) 

  * discovery.0.0 : eureka 서버 입니다
  * gateway.0.0 : api gateway 서버 입니다
  * account.0.0 : 사용자 계정 서버 입니다
  * auth.0.0 : 인증 서버 입니다
  * game.0.0 : 게임 서버 입니다
  * (database mysql:latest 버전 사용)
  
------
### 사용 url 

localhost:8080 포트로 gateway 서버를 접속할 수 있습니다.<br>
mysql은 3306 포트로 접속할 수 있습니다. <br>
나머지 서버들은 gateway 서버를 통해 접속합니다

discovery 서버
> localhost:8080/registry : eureka 서버에 등록된 서버 목록을 볼 수 있다
> localhost:8080/eureka/** 

account 서버
> localhost:8080/accounts/** 

auth 서버
> localhost:8080/auth/**  
> localhost:8080/oauth2/**

game 서버
> localhost:8080/games/**

로그인 url<br>
```
http://localhost:8080/oauth2/authorize/<provider>?redirect_uri= <허가 redirection url>
ex) http://localhost:8080/oauth2/authorize/google?redirect_uri=http://localhost:8080/auth/home

provider
* google
* kakao

허가 redirection url
* http://localhost:3000/oauth2/redirect
* http://localhost:8080/auth/home
```
