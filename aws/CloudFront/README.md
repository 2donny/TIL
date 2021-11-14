# What is Cloud Front

**Amazon CloudFront는 뛰어난 성능, 보안 및 개발자 편의를 위해 구축된 콘텐츠 전송 네트워크(CDN) 서비스입니다.**

> CDN(content delivery network)이란 content를 효율적으로 delivery하기 위해 여러 노드를 가진 network에 데이터를 저장하여 제공하는 시스템을 말한다.

## S3 버킷 origin 서버를 CloudFront로 

### 장점

1. CDN 서버는 여러 리전에 분배돼있기 때문에 전 세계에서 오는 트래픽에 대해서도 빠르게 리소스를 delivery할 수 있다.
2. 하나의 origin 서버에 트래픽이 분산되는 것을 막아준다.
3. CDN 프록시 서버로부터 요청을 받기 때문에 캐싱에 역할을 해준다.
   1. HTTP 헤더에 Cahe 설정을 해줌으로써 stale한 데이터를 주기적으로 업데이트 해줘야 한다.


### 적용

실제로 S3 Bucket origin에 대하여 AWS CloudFront를 적용해보자

1. AWS Cloud front 콘솔에 들어간다.
2. 배포 생성 클릭
3. "원본 도메인"에서 AWS 서비스를 선택한다(S3 도메인)하고 "배포 생성"
4. "배포 도메인 이름"을 도메인으로 사용한다.

### 응용

Lambda@edge 서비스로 CloudFront, Lambda를 같이 사용할 수 있다.
CloudFront로 요청이 들어오면 Lambda 함수를 트리거하는 기술이다.
On fly 상태에서 이미지를 실시간으로 리사이징할 때 주로 사용하는 기술이다.

