---
layout: post
title:  "[Spring] 디자인 패턴의 아름다움 3일차"
categories: Spring
author: start-easy
---
* content
{:toc}

* SOLID, KISS, YAGNI, DRY, LoD에 대해 배워보겠습니다.

디자인 패턴 22가지는 코드 확장성을 개선하기 위한 코드가 대부분이며, 개방 폐쇄 원칙을 기본으로 합니다.


> p134
> 설계 원칙을 배울 때, 설계 원칙의 정의를 이해하는 것보다 훨씬 중요한 것은 왜 그 원칙이 생겨날 수밖에 없었는지를 생각하여 목적을 파악하는 것이다.

## SOLID
    
**1. 단일 책임 원칙**

### (SRP, Single Responsibility Principle)
  
"클래스는 단 하나의 책임만 가져야 한다."
  
생겨난 이유:

- 하나의 클래스가 여러 기능을 담당하면 변경이 발생할 때 수정할 부분이 많아지고, 의도하지 않은 사이드 이펙트가 발생할 가능성이 높음.
- 유지보수가 어려워지고, 코드가 복잡해짐.
- 서로 다른 이유로 변경될 가능성이 있는 코드는 분리해야 함.

예제:

- 한 클래스에서 데이터베이스 접근, UI 처리, 비즈니스 로직을 모두 담당하면 변경이 일어날 때마다 영향을 받을 가능성이 큼.

---

**2. 개방-폐쇄 원칙** 

### (OCP, Open-Closed Principle)

"확장에는 열려 있어야 하고, 수정에는 닫혀 있어야 한다."


생겨난 이유:

- 기존 코드를 변경하면 새로운 버그가 발생할 위험이 있음.
- 기능을 확장할 때 기존 클래스를 수정하는 방식이면 지속적인 코드 변경이 필요함.
- 새로운 요구사항이 생길 때 기존 코드 수정 없이 확장할 수 있도록 설계해야 함.

예제:

- `if-else` 문으로 여러 조건을 처리하는 코드가 있을 때, 새로운 기능을 추가하려면 기존 코드를 수정해야 함 → OCP를 적용하면 새로운 클래스를 추가하는 방식으로 기능을 확장 가능.

---

**3. 리스코프 치환 원칙** 

### (LSP, Liskov Substitution Principle)

"서브타입은 기반 타입을 대체할 수 있어야 한다."


생겨난 이유:

- 상속을 잘못 사용하면 다형성이 깨지고, 코드의 일관성이 무너짐.
- 자식 클래스가 부모 클래스의 기능을 온전히 수행하지 못하면, 부모 타입을 기대하는 코드에서 예외가 발생할 수 있음.
- 올바른 상속 구조를 유지하지 않으면 코드의 재사용성이 떨어지고, 예측하지 못한 버그가 발생할 가능성이 높아짐.

예제:

- `Bird` 클래스를 상속받은 `Penguin` 클래스가 `fly()` 메서드를 지원하지 않으면, `Bird`를 기대하는 코드에서 문제가 발생함.
- 상속보다 인터페이스를 활용하거나, `is-a` 관계를 신중하게 고려해야 함.

---

**4. 인터페이스 분리 원칙** 

### (ISP, Interface Segregation Principle)

"클라이언트는 자신이 사용하지 않는 메서드에 의존하지 않아야 한다."


생겨난 이유:

- 하나의 인터페이스가 너무 많은 기능을 포함하면, 클라이언트가 필요하지 않은 메서드까지 구현해야 함.
- 불필요한 의존성이 증가하면 유지보수성이 떨어짐.
- 인터페이스를 작게 나누면 필요한 기능만 구현할 수 있어 유연성이 증가함.

예제:

- `Worker` 인터페이스가 `work()`와 `eat()` 메서드를 가진다면, `RobotWorker` 클래스가 `eat()`을 구현해야 하는 문제가 생김.
- `Workable`, `Eatable` 인터페이스로 분리하면 불필요한 메서드 구현을 방지할 수 있음.

---

**5. 의존 역전 원칙** 

### (DIP, Dependency Inversion Principle)

"상위 모듈이 하위 모듈에 의존해서는 안 된다. 둘 다 추상화에 의존해야 한다."


생겨난 이유:

- 하위 모듈이 변경될 때 상위 모듈도 변경해야 하면 유지보수 비용이 증가함.
- 특정 구현체에 의존하면 코드가 유연하지 않고 확장성이 떨어짐.
- 추상화(인터페이스)를 사용하면 하위 모듈을 변경해도 상위 모듈을 수정할 필요가 없음.

예제:

- `DatabaseService` 클래스가 `MySQLDatabase`를 직접 의존하면, 나중에 `PostgreSQLDatabase`로 변경할 때 수정해야 하는 코드가 많아짐.
- `DatabaseService`가 `DatabaseInterface`에 의존하면, 다양한 데이터베이스 구현체를 쉽게 교체 가능.


---


* 단일 책임 원칙을 지키기 위해 고려할 사항

    1. 클래스에 속성이 너무 많은 경우 - 가독성과 유지 보수성에 영향을 끼치지 않도록 구현
    2. 다른 클래스에 의존하면 높은 응집도, 낮은 결합도의 코드 설계 사상과 부합하지않는다.
    3. private 메서드가 많으면 해당 메서드 들을 별도의 클래스로 분리하고 더 많은 클래스에서 사용할 수 있도록 public 메서드로 변경
    4. 이름을 일반적인 단어로 명시하지 말것(manager, context)
    5. 테이블을 분리했을 때 Serializer, Deserializer 처럼 하나를 바꾸면 다른것도 바뀌는 게 있는지 확인

* 클래스 설계 외에 단일 책임 원칙을 적용할 수 있는 설계에는 어떤 것이 있을 수 있을까?
1. 기능별로 패키지/모듈 구분

```shell
com.example.app
│── domain
│   ├── user
│   │   ├── User.java
│   │   ├── UserRepository.java
│   │   ├── UserService.java
│   │   ├── UserController.java
│   │   └── dto
│   │       ├── UserRequestDto.java
│   │       └── UserResponseDto.java
│   ├── order
│   │   ├── Order.java
│   │   ├── OrderRepository.java
│   │   ├── OrderService.java
│   │   ├── OrderController.java
│   │   └── dto
│   │       ├── OrderRequestDto.java
│   │       └── OrderResponseDto.java
➡️ User와 Order 도메인을 분리하여 단일 책임 원칙을 지킴
➡️ 각 도메인은 독립적으로 관리될 수 있음
```

2. 기능별로 서비스를 분리

```jsx
public class UserService {
    private final UserRepository userRepository;

    public void createUser(User user) {
        userRepository.save(user);
    }
}

public class EmailService {
    public void sendEmail(User user) {
        // 이메일 전송 로직
    }
}

public class LogService {
    public void logActivity(User user) {
        // 로그 기록 로직
        }
}

```

3. CQRS 패턴

읽는 것과 수정,삭제를 별로로 분리하는 것

```jsx
public class UserCommandService {
    public void createUser(User user) {
        // 유저 생성 로직
    }
}

public class UserQueryService {
    public User getUserById(Long id) {
        // 유저 조회 로직
    }
}
```

4. 이벤트 기반 아키텍처 적용

- 카프카 코드와 서비스 코드를 분리

```jsx
public class OrderService {
    private final KafkaTemplate<String, OrderEvent> kafkaTemplate;

    public void placeOrder(Order order) {
        // 주문 저장 로직
        kafkaTemplate.send("order-events", new OrderEvent(order.getId(), "ORDER_CREATED"));
    }
}

```
    

* 리스코프 치환 원칙 안티 패턴

1. 하위 클래스가 구현하려는 상위 크래스에서 선언한 기능을 위반하는 경우
- sortOrdersByAmount() 를 id순서대로 정렬하는데 시간 순서로 정렬하는 경우
2. 하위 클래스가 입, 출력 및 예외에 대한 상위 클래스 계약을 위반하는 경우
- 에러 난경우 null 반환인데 예외를 발생시키는 경우
- 상위 클래스에서는 정수를 받는데 하위 클래스에서 음수이면 에러를 반환하는 경우
- 상위 클래스에서 발생하는 예외가 한가지인데 하위 클래스에서 더 많은 예외가 있는 경우
3. 상위 클래스의 주석에 나열된 특별 지침을 위반하는 경우


---

- getAndIncrement()메서드는 단일 책임 원칙과 인터페이스 분리 원칙을 준수하는 함수인가?
정수에 1을 더하고, 변경되기 전의 값을 반환하는 메서드
    
get 과 increment를 동시에 진행하는 것 → 수정과 가져오는 것을 진행하는 함수이기에 인터페이스 분리의 원칙을 지키지 않는다고 생각한다. 

```
getAndIncrement(), getAndAdd()getAndIncrement() : 현재 값 리턴하고 +1 증가incrementAndGet() : +1 증가시키고 변경된 값 리턴
```

- getAndIncrement() : 현재 값 리턴하고 +1 증가
- incrementAndGet() : +1 증가시키고 변경된 값 리턴

이렇게 메서드 내용이 바뀌면 실제 값이 바뀌고 이는 코드를 더 정확하게 읽지 않으면 코드가 실패할 우려가 있기 때문이다.

하지만 이것을 원자적으로 관리한다는 정의 안에서는 정확하게 의도에 맞게 구현하고 있고, 인터페이스를 분리하기에는 어려운 메서드이기에 오히려 적합한 함수라고 할 수 있다.

```jsx
import java.util.concurrent.atomic.AtomicInteger;

public class SafeCounterWithoutLock {
    private final AtomicInteger counter = new AtomicInteger(0);
    
    int getValue() {
        return counter.get();
    }
    
    void increment() {
        while(true) {
            int existingValue = getValue();
            int newValue = existingValue + 1;
            if(counter.compareAndSet(existingValue, newValue)) {
                return;
            }
        }
    }
}
```
    
---

* 추가 : 원자 연산을 왜 해야하는가?

    모든 클래스를 락을 거는 것은 비용의 문제가 있다. 따라서 syncronized를 사용하지 않고 연산을 하는 것의 필요성이 있다. 이와 관련된 연산이 원자 연산이다.

    **원자 연산 작동 방식**: CPU의 **CAS(Compare-And-Swap) 연산**을 이용하여 동기화 없이 원자적 연산 수행

    **Sync 작동 방식**: 객체 수준에서 **모니터 락(Monitor Lock)**을 획득하여 스레드가 순차적으로 실행되도록 보장

    참고 : https://www.baeldung.com/java-atomic-variables

---

* KISS 원칙과 YAGNI 원칙

> KISS : Keep It Simple and Stupid, Keep it Short and Simple, Keep it Simple and Straight forward

    모든 것을 구현하거나, 짧은 코드가 정답이 아니라 코드 구현이 쉽고 가독성이 좋은 코드를 선택해야한다.

> YAGNI : 추후 사용하게 될 코드를 고려하여 미리 작성할 필요가 없다.


* 앞으로 개발 가능한 것을 고려하지 말라는 뜻인가?

- **장기적 비전 부족**: 때때로 YAGNI는 장기적인 시스템 설계나 아키텍처의 발전을 간과할 수 있습니다. 이는 추후 시스템이 확장될 때 문제가 될 수 있습니다.
- **과도한 단순화**: 일부 경우에는 YAGNI가 너무 강조되어 시스템의 확장성이나 유연성이 무시될 수 있습니다. 이는 장기적으로 더 많은 작업을 초래할 수 있습니다.

- **균형 잡힌 접근**: YAGNI 원칙은 상황에 따라 다르게 적용되어야 합니다. 현재의 필요와 미래의 가능성 사이에 균형을 찾는 것이 중요합니다.
- **팀과의 의사소통**: 개발 팀 내에서 YAGNI 원칙에 대한 명확한 이해와 합의가 필요합니다. 이를 통해 과도한 기능 개발을 방지하고 프로젝트의 방향성을 유지할 수 있습니다.

> 맥킨지 연구에 의하면 기업들은 프로덕트 개발비를 50%초과 지출하면 세후 이익을 3.5% 손실하는 반면, 6개월 늦게 출시하면 33%를 잃는다고 한다. - 찰스 H. 하우스, 레이먼드  L. 프라이스
> 

DRY 원칙 : 중복 코드를 작성하지 말라는 것 (단순 중복이 아니다), 똑같은 의미를 가진 코드가 여러개 중복되면 그것을 하나로 합치는 것이다.

기능적 중복, 코드 실행의 중복 → 코드의 재사용성

> LoD(데메테르의 원칙) : 매우 밀접하게 관련된 유닛에 대해 제한된 지식만 알아야하는 원칙
>