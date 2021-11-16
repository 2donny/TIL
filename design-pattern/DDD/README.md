# DDD (도메인 주도 개발)

1. 레이어드 아키텍쳐 : 관심사를 레이어 별로 분리한 다는 점에서 좋다.
   1. Presentation -> Business -> Persistence -> Database Layer
   2. Request가 항상 위에서 아래로 흐른다.
   3. 바로 밑을 건너뛸수 없이 항상 인접 레이어를 거쳐서 내려갈 수 있다.
