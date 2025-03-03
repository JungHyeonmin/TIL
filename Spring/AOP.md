## AOP(Aspect-Oriented Programming)란?

AOP는 Aspect Oriented Programming의 약자로 관점 지향 프로그래밍이라고 불린다.
관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것이다.
여기서 모듈화란 어떤 공통된 로직이나 기능을 하나의 단위로 묶는 것을 말한다. 
예로들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다.
또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.
AOP에서 각 관점을 기준으로 로직을 모듈화한다는 것은 코드들을 부분적으로 나누어서 모듈화하겠다는 의미다. 이때, 소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드들을 발견할 수 있는 데 이것을 흩어진 관심사 (Crosscutting Concerns)라 부른다.

위와 같이 흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지다.

**AOP(Aspect-Oriented Programming, 관점 지향 프로그래밍)**은 객체지향 프로그래밍(OOP)의 한계를 보완하기 위해 등장한 개념으로, **핵심 비즈니스 로직과 공통 관심 사항을 분리하여 코드의 모듈성을 향상시키는 프로그래밍 기법**이다.

---

## 1. AOP가 필요한 이유
### 1) **공통 관심 사항(Cross-Cutting Concerns) 문제**
객체 지향 프로그래밍(OOP)에서는 특정 기능(예: 로깅, 보안, 트랜잭션 관리 등)이 여러 클래스에 걸쳐 중복되는 경우가 많다. 이러한 코드가 여기저기 흩어져 있으면 유지보수가 어려워진다.

**예제:**  
아래처럼 모든 서비스 메서드에서 로그를 남겨야 한다고 가정하면, 각 메서드마다 동일한 코드가 반복된다.

```java
public class OrderService {
    public void placeOrder() {
        System.out.println("로그: 주문을 생성합니다.");
        // 주문 생성 로직
        System.out.println("로그: 주문 생성 완료.");
    }
}

public class PaymentService {
    public void processPayment() {
        System.out.println("로그: 결제를 진행합니다.");
        // 결제 처리 로직
        System.out.println("로그: 결제 완료.");
    }
}
```
위처럼 **각 클래스에서 반복되는 코드(로깅 코드)가 많아지면 유지보수성이 떨어진다.**  
이러한 문제를 해결하기 위해 AOP를 사용한다.

---

## 2. AOP의 핵심 개념
AOP에서는 **공통 관심 사항(Cross-Cutting Concerns)** 을 별도로 분리하여 `Aspect(관점)`으로 관리한다.

### AOP 주요 개념
| 개념 | 설명 |
|------|------|
| **Aspect** | 공통 기능을 모듈화한 것 (예: 로깅, 트랜잭션 관리) |
| **Advice** | 언제(어떤 시점) 공통 기능을 실행할지 정의 (Before, After 등) |
| **Join Point** | Advice가 적용될 수 있는 지점 (예: 메서드 실행, 예외 발생 등) |
| **Pointcut** | Advice가 적용될 특정 조건 (예: 특정 패키지의 모든 메서드) |
| **Weaving** | Pointcut에 따라 Advice를 실제 코드에 적용하는 과정 |

---

## 3. AOP 적용 방법 (Spring AOP 예제)
Spring에서는 **AspectJ** 기반으로 AOP를 지원하며, `@Aspect` 어노테이션을 활용하여 적용할 수 있다.

### 1) AOP 설정 및 적용
#### **Step 1: 의존성 추가 (Spring Boot 환경)**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

#### **Step 2: Aspect(공통 기능) 작성**
아래 예제에서는 모든 `Service` 클래스의 메서드 실행 전에 로깅하는 AOP를 적용한다.

```java
import org.aspectj.lang.annotation.*;
import org.aspectj.lang.JoinPoint;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    // 모든 Service 클래스의 메서드 실행 전에 동작하도록 설정
    @Before("execution(* com.example.service.*.*(..))")
    public void logBeforeMethodExecution(JoinPoint joinPoint) {
        System.out.println("로그: " + joinPoint.getSignature().getName() + " 메서드 실행");
    }

    // 메서드 실행 후 동작하는 Advice
    @After("execution(* com.example.service.*.*(..))")
    public void logAfterMethodExecution(JoinPoint joinPoint) {
        System.out.println("로그: " + joinPoint.getSignature().getName() + " 메서드 실행 완료");
    }
}
```

#### **Step 3: AOP가 적용되는 서비스 코드**
```java
import org.springframework.stereotype.Service;

@Service
public class OrderService {
    public void placeOrder() {
        System.out.println("주문을 생성합니다.");
    }
}
```

#### **실행 결과**
```plaintext
로그: placeOrder 메서드 실행
주문을 생성합니다.
로그: placeOrder 메서드 실행 완료
```
→ `placeOrder()` 메서드가 실행되기 전후에 자동으로 로깅이 수행됨.

---

## 4. AOP의 주요 적용 사례
AOP는 다양한 곳에서 활용된다.

### 1) **로깅(Logging)**
- 모든 메서드 실행 시 로깅을 추가하는 데 사용됨.
- 성능 모니터링에도 활용 가능.

### 2) **트랜잭션 관리(Transaction Management)**
- Spring에서 `@Transactional`이 AOP 기반으로 동작함.
- 특정 메서드 실행 시 자동으로 트랜잭션을 적용.

### 3) **보안(Security)**
- 메서드 실행 전 사용자 권한 검사를 수행할 때 활용됨.
- 예: `@PreAuthorize("hasRole('ADMIN')")`

### 4) **예외 처리(Exception Handling)**
- 특정 예외가 발생했을 때 공통적으로 처리하는 로직을 적용 가능.

---

## 5. AOP 적용 전후 코드 비교
| 구분 | 적용 전 | 적용 후 (AOP 사용) |
|------|--------|----------------|
| **코드 중복** | 여러 클래스에서 동일한 코드 반복 | 별도의 `Aspect`에서 관리 |
| **유지보수성** | 변경 시 모든 클래스 수정 필요 | `Aspect` 수정만으로 반영 가능 |
| **가독성** | 핵심 비즈니스 로직과 공통 기능이 섞임 | 핵심 로직과 분리되어 깔끔 |

---

## 6. AOP의 한계
1. **디버깅이 어려움**
    - 코드가 명시적으로 호출되지 않기 때문에, 예상치 못한 동작을 할 수 있음.
2. **남용 시 가독성 저하**
    - 지나치게 사용하면 코드의 흐름이 불분명해질 수 있음.
3. **Weaving 비용 발생**
    - 런타임에서 AOP 적용 시 성능 오버헤드가 있을 수 있음.

---

## 7. 정리
- **AOP는 핵심 로직과 공통 관심 사항을 분리하여 유지보수성을 향상시키는 기법이다.**
- **Spring AOP를 사용하면 로깅, 트랜잭션, 보안 등을 쉽게 적용할 수 있다.**
- **Pointcut을 활용해 특정 클래스나 메서드에만 AOP를 적용할 수 있다.**
- **과도한 AOP 사용은 코드 가독성을 저하시킬 수 있으므로 신중하게 적용해야 한다.**

백엔드 개발을 할 때 트랜잭션 관리나 로깅, 보안 등의 기능을 효율적으로 관리하려면 AOP를 적극 활용하는 것이 좋다! 🚀