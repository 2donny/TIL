# OS

1. 프로세스란: 스케줄링의 단위이다. Code, Data, Heap, Stack으로 이뤄져있다.
   1. Code : 말그대로 작성한 코드가 들어있고 프로세스 종료까지 유지된다.
   2. Data : 전역/정적변수, 배열, 객체 등이 저장된다.
   3. Heap : 개발자가 동적으로 사용하는 영역. malloc/free, new/delete로 할당/반환 되는 영역이다.
   4. 프로세스간 자원을 공유하기 위해서는 IPC를 통해서 가능하다.
2. 쓰레드란: 프로세스안에 있는 여러가지 실행의 흐름이다.
   1. 쓰레드는 각자의 Stack을 갖는다.
   2. 프로세스의 자원을 공유하기 때문에, 다른 쓰레드에 의한 결과를 즉시 확인가능
3. IPC(Inter Process Communication)란 프로세스 끼리 자원을 공유하는 방식
   1. Shared memory : 프로세스끼리 공유 메모리를 사용한다.
      1. 장점) Message passing처럼 커널을 거치지 않기 때문에 System-call 부를 필요가 없어 kernel 의존성이 낮다. 그만큼 속도가 빠르다.
      2. 단점) shared memory만큼 공간이 필요하다.
