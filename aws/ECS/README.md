## ECS (Elastic container service)

### [What is Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
- ECS is a highly scalable, fast container management service that makes it easy to run, stop, and manage containers on a cluster
- 특정 리전 내 가용 영역에서 컨테이너를 실행하는 과정을 간소화하는 리전 서비스이다.
- ```컨테이너들```은 service 내에 task definition에서 정의된다
- service는 
  - task들을 동시에 실행하고, 유지보수할 수 있게 해주는 설정이다.
  - 지속적으로 실행돼야하는 컨테이너를 서비스에서 생성한다.
- Task definition은 작업을 실행하거나, 서비스를 만드는데 사용된다.
- 도커 이미지는 DockerFile 파일을 통해 빌드되고, 빌드된 이미지를 ECR나 Docker hub에 푸쉬한다.
- Task definition을 생성할 때 메모리, 네트워크, 프로토콜을 설정해주고, 앞에서 푸쉬한 ECR에서의 이미지 uri를 선택할 수 있다.
- 그리고 서비스 내에 몇개의 concurrent한 task를 실행할지도 결정한다.




 ECS가 각광받는 이유중 Fargate를 들 수 있다. 장점은 다음과 같다.
### Fargate 장점
- AWS Fargate는 컨테이너만 생각
- EC2를 관리할 필요 X, EC2위에 도커를 올릴 필요 X
  - 빠른 개발, 유지보수가 가능해진다.
- 컨테이너에 대한 비용만 지불할 수 있음.
- 배치 작업들을 할 때 유용함.
  - 크롤링과 같은 지속적으로 running할 필요 없고 한번 실행되고 죽어도 되는 작업을 할 때 fargate가 유리하다. 쓸데없이 인스턴스를 올려놓고 대기시키는 과금도 save할 수 있다. [참고링크](https://www.youtube.com/watch?v=bEr_98NRlzc)
  - 또 ECS의 ```작업``` 탭에서 ```예약된 작업```의 ```고정된 간격으로 실행``` 설정을 통해 분/시간/일 단위로 배치 작업을 예약할 수 있다.
- 이처럼 과금을 크게 줄인다.
  - 기존 서버에서는 EC2가 올라가있기 때문에 EC2에 대한 비용을 지불해야했었다면, Fargate를 통해서 container를 띄우는 비용만 지불하면 된다.


### Flow
1. 