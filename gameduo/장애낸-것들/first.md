# 장애낸 것들

## TypeORM JoinColumn name 정할 때만 Snake case로 해주자.

통상적으로 Javascript Class는 Camel case를 쓰고, DB 테이블은 Snake case를 사용한다.
그래서 난 보통 ormconfig.js에서 { namingStrategy: new SnakeNamingStrategy() } 속성을 추가해서 자동으로 변환시킨다. 하지만 Entity.ts 파일에서 @JoinColumn({ name: 'userId'}) 이런 식으로 작성하고 디비를 확인해보면 userId, user_id 컬럼이 동시에 2개가 생긴다. 이를 막기 위해서 그냥 @JoinColumn({ name: 'fk_uesr_id }}) 이렇게 해주자.
아니면 500에러남..