# Redis

> [강대명님의 우아한 테크 세미나를 참조했습니다.](https://www.youtube.com/watch?v=mPB2CZiAkKM)

## Redis란

**Remote Dictionary Server, In-memory data store 이다.**

> Dictionary는 Java의 Hash map이다.

## Redis 특징

- Single-Thread 이다.
  - 한 번에 최대 하나의 요청만 처리가능.
  - 그래도 1초에 최대 10만개 요청 처리가능.
  - 그러나 Single-thread 라서 최초에 1개의 요청이 1초가 걸리면 나머지 99,999개 요청은 1초를 기다려야함.
- Redis는 Atomic하기 때문에 Concurrent Transaction이 들어올 때 내부적으로 Race condition을 피하는 소프트웨어가 구현돼있음. (그래도 조심해야함)
  - Thread-safe 하다. 
- Replication을 제공

## Redis Collections

- Strings
- List
- Set
- Sorted Set
- Hash

### String - 단일 key

- 기본 사용법
  - Set (Key) (Value)
  - Get Key
- Set token:12345 asdasveok
- Get token:2345

### List : insert, Pop

- 기본 사용법
  - Lpush (key) A
    - Key A
  - Rpush (key) B
    - Key (A, B)
  - Lpop (key) 
    - Key (B)
    

## Collection 주의 사항

- 하나의 Collection에 너무 많은 Item을 담으면 좋지 않음.
  - 10000개 이하 몇천개 수준으로 유지하는 게 좋음.
- Collection 선택할 때는 Big-O 를 잘 생각해서 사용해야한다.


## Redis 운영

* 메모리 관리를 잘하자
* O(N) 관련 명령어는 주의하자
  * hmset.keys()


# 메모리 관리

* Physical memory 이상을 쓰면 문제가 발생
  * Swap이 있다면 메모리 page를 Disk에 저장해놓고, 필요하면 다시 로딩시킴.
  * 스왑이 발생한 메모리 페이지는 계속 스왑이 일어남. 그럼 그 메모리 페이지에 접근하는 key가 있으면 그때마다 Disk를 읽고/쓰게됨. 그럼 in-memory를 사용하는 이유가 없어짐. 
  
