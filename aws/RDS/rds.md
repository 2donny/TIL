# AWS RDS

## 학습 배경

기존에 개발시에는 localhost:5432 에서 DB 서버를 키고, production으로 배포할 때에는 heroku를 통해 Nodejs 서버와 db 서버를 같이 배포하곤 했었다.
헤로쿠로 배포하면 정말 쉽게 배포가 가능한 장점이 있지만, custom manipulation이 어렵기 때문에 aws rds를 공부하게 되었다.


## 장점
[공식 문서](https://aws.amazon.com/ko/rds/)에서 여러가지 장점을 소개하고 있는데 가장 와닿는 점은 ```Multi AZ로 인한 백업```, ```하드웨어 프로비저닝```, ```DB 인스턴스 스펙 변경가능```, ```데이터 시각화```를 뽑았다. 

## 설정
개발 시에는 '퍼블릭 엑세스 가능성'을 '예'로 설정하여 public ip address를 할당 받아 편리하게 개발한다.
하지만, RDS 인스턴스를 배포하게 된다면 database 엔드포인트를 알고있다면 누구나 접속하게 되므로 '아니오'로 바꾼다.

이를 보완하기 위해 특정 서버에서만 db에 접속할 수 있도록 해주는 것이 VPC이다. 
VPC 보안 그룹을 설정해야한다.
