### 월드컵 discovery 서버

Eureka 사용

-------

**Docker**

도커 이미지
* docker pull tjwjdgks43/code-etc-discoveryserver:<tag이름> (ex v.0.1)

실행 예시
* docker run -d -p 8761:8761 --name="discovery-server" tjwjdgks43/code-etc-discoveryserver:<tag이름>
