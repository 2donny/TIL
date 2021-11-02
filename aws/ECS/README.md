## ECS (Elastic container service)

### [What is Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)

- ECS is a highly scalable, fast container management service that makes it easy to run, stop, and manage containers on a cluster
- 특정 리전 내 가용 영역에서 컨테이너를 실행하는 과정을 간소화하는 리전 서비스이다.
- `컨테이너들`은 service 내에 task definition에서 정의된다.
- service는
  - task들을 동시에 실행하고, 유지보수할 수 있게 해주는 설정이다.
  - 지속적으로 실행돼야하는 컨테이너(웹 서버)를 서비스에서 생성한다.
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
  - 또 ECS의 `작업` 탭에서 `예약된 작업`의 `고정된 간격으로 실행` 설정을 통해 분/시간/일 단위로 배치 작업을 예약할 수 있다.
- 이처럼 과금을 크게 줄인다.
  - 기존 서버에서는 EC2가 올라가있기 때문에 EC2에 대한 비용을 지불해야했었다면, Fargate를 통해서 container를 띄우는 비용만 지불하면 된다.

## 마이크로 서비스로 배포 Flow

> [공식문서](https://aws.amazon.com/ko/getting-started/hands-on/break-monolith-app-microservices-ecs-docker-ec2/)를 참조하였다.

### ECR repository에 도커 이미지 푸쉬하기

1. 가장 먼저 [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html#cliv2-mac-prereq)를 install하자
   1. 처음 AWS CLi를 인스톨하는 거라면 aws configure도 해야하는데 IAM 사용자를 하나 만들자.
   2. 정책은 다음의 4개를 추가해주자.
      1. AWSCodeDeployRoleForECS
      2. AmazonECS_FullAccess
      3. EC2InstanceProfileForImageBuilderECRContainerBuilds
      4. AWSAppRunnerServicePolicyForECRAccess
   3. 다음 명령어로 ECS에 로그인하자
   ```shell
      aws ecr get-login-password --region ap-northeast-2 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.ap-northeast-2.amazonaws.com
   ```
2. 간단한 express 서버 코드를 작성한다.

```javascript
const express = require("express");
const app = express();

app.listen(80, () => {
  console.log("Server is listening on port 80");
});
```

3. 레포지토리 생성
   1. AWS [ECR 콘솔](https://ap-northeast-2.console.aws.amazon.com/ecr/repositories?region=ap-northeast-2)로 가서 레포지토리 생성 클릭
   2.
4. Docker 이미지 빌드 및 푸쉬
   1. 이미지 빌드 ```docker build -t nestjs-demo . ```
   2. 빌드된 이미지와 ECR 레포지토리를 매핑 ```docker tag nestjs-demo:latest 471011865482.dkr.ecr.ap-northeast-2.amazonaws.com/nestjs-demo:latest```
   3. AWS ECR 레포지토리로 푸쉬 ```docker push 471011865482.dkr.ecr.ap-northeast-2.amazonaws.com/nestjs-demo:latest```


### ECS 클러스터 생성하기
