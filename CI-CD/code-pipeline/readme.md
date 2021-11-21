# Code pipeline

CI/CD를 제공하는 AWS 서비스이다.

## Code Build

- Code Build는 코드 빌드 및 테스트를 자동화해준다.
- 주로 github과 연동하여 master 브랜치에 코드가 푸쉬될 때 Trigger된다.
- buildspec.yml 파일을 작성하여 Code Build가 트리거될 때 액션을 정의한다.
- 도커를 쓴다면 `Dockerfile`을 읽어서 `image`를 만들어 `docker hub`에 push한다.
- yaml 파일에서 build - commands 명령어가 전부 끝나면 빌드된 파일을 .zip파일로 압축하여 S3 버킷에 저장해놓는다. 그리고 Code Deploy를 트리거한다.

buildspec.yml

```yml
version: 0.2

phases:
  install:
    runtime-versions:
      docker: 18
      nodejs: 10
  build:
    commands:
      - echo "$DOCKER_HUB_PASSWORD" | docker login -u "$DOCKER_HUB_ID" --password-stdin

      - docker build -t 2donny/beanstalk-fullstack-nginx ./nginx
      - docker build -t 2donny/beanstalk-fullstack-backend ./backend
      - docker build -t 2donny/beanstalk-fullstack-frontend ./frontend

      - docker push 2donny/beanstalk-fullstack-nginx
      - docker push 2donny/beanstalk-fullstack-backend
      - docker push 2donny/beanstalk-fullstack-frontend

artifacts:
  files:
    - "**/*"
  discard-paths: no
  base-directory: ./
```

## Code Deploy

- Code Deploy는 배포를 자동화해준다.
- Code Build가 끝나면 Code Deploy가 실행된다.
- `배포 공급자`로 설정한 서비스를 트리거시킨다.
  - Bean stalk, ECS 등이 있다.
- Bean stalk 환경을 사용할 경우 다음과 같은 파일을 같이 빌드시킴으로서 Code deploy에게 어떤 액션을 할지 설정할 수 있다.

```json
{
  "AWSEBDockerrunVersion": 2,
  "containerDefinitions": [
    {
      "name": "frontend",
      "image": "2donny/beanstalk-fullstack-frontend",
      "hostname": "frontend",
      "essential": false,
      "memory": 128
    },
    {
      "name": "backend",
      "image": "2donny/beanstalk-fullstack-backend",
      "hostname": "backend",
      "essential": false,
      "memory": 128
    },
    {
      "name": "nginx",
      "image": "2donny/beanstalk-fullstack-nginx",
      "hostname": "nginx",
      "essential": true,
      "portMappings": [
        {
          "hostPort": 80,
          "containerPort": 80
        }
      ],
      "links": ["frontend", "backend"],
      "memory": 128
    }
  ]
}
```
