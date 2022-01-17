## Elastic beanstalk 으로 배포할 때는

항상 빌드하고 배포하자.
혹은 Procfile이 항상 dist 폴더 아래에 있어야한다.

빌드해야하는 이유는
배포 후 Beanstalk이 빌드된 파일을 실행할 때 node ./dist/main.js 이런 명령어를 실행하는데
그러기 위해서는 dist 디렉토리를 생성해야하므로 꼭 해주고 배포하자

아니 그냥 배포 스크립트를 이렇게 쓰자

```json
{
    "deploy": "npm run build && eb deploy"
}
```
