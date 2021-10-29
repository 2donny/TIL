# 클린 코드

> [유튜브 클린 코더스 강의](https://www.youtube.com/watch?v=60lLSe1phks) 를 듣고 정리한 글입니다.

## 궁극적으로는 OOP를 지향해야한다.

- 객체간의 응집도(Cohesion)는 높고, 결합도(Coupling)는 낮아야한다.
- `객체간의 의존성`이 낮아야지 기능이 추가되거나 유지보수할 때 생산성이 높아진다.


## 캡슐화
- 클래스 내 멤버변수는 private으로 최대한 외부로부터 감춰 객체간의 의존성을 미연에 낮춘다.
- public method를 통해서만 맴버변수를 읽을 수 있다.

## 메소드의 정석
- Procedure의 Getter, Setter를 최대한 지양한다. 
- public method는 오직 ```기능```만 존재해야한다. 
- ```Tell Don't Ask```
  - 데이터를 요청해서 변경하려고 하지말고, 그 데이터를 잘 알고있는 객체에게 기능을 수행하라고 하라.
  - Encapsulation이 유지되고, 코드 읽기도 훨씬 쉬움
```typescript
if(member.getExpiredDate().getTime() < new Date().getTime())
// Bad -> 이렇게 남이 가지고있는 속성으로 내가 판단하지 말고
if(member.isExpired())
// Good -> 이렇게하면 좋은 점이 읽기 쉬울 뿐아니라, expire 조건이 바뀌더라고 해당 메소드 안에서 한번만 바꾸면된다.
```

## 재사용
- 재사용을 이루기 위해선 여러가지 방법이 있다. 그 중 하나가 ```추상화```를 소개한다.
- 추상화를 통해서 상위 수준에서 설계하는데에 도움을 얻을 수 있다.
- 위 강의에서는 하나의 추상화된 interface를 정의하여 그 안에 메소드를 정의하고, Low level 클래스에서 interface를 implements 함으로써 얻는 재사용을 예로든다.

## DI
- 나오게 된 배경) 기존에는 외부 모듈을 가져다 쓰려면은 다음과 같이 ```Factory의 static method```를 사용해 인스턴스 만들었다. 이렇게 하면 Business logic은 재사용이 가능하고 깨끗해지지만 반대로 Factory의 코드는 재사용이 안되고 더러워진다.
- 또한 static method는 매우 큰 의존성이다.
```java
    public void process() {
        LogCollector collector = LogFactory.createCollector();
    }
```

- Dependency injection으로는 이것이 매우 쉽게된다.
- DI 장점으로는 다음과 같은데
  - 큰 class에 method를 계속 넣는 걸 막아준다.
  - 역할이 다른 기능은 별도의 class로 빼고 interface로 상속하게하여 추상화를 통해 기능 단위로 구현을 하게 해준다.
  - 그리고 이것을 mock을 통해 쉽게 유닛 테스트할 수 있다.
