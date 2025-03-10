# **단위 테스트(Unit Test)와 통합 테스트(Integration Test)**

## **1. 단위 테스트(Unit Test)란?**
단위 테스트(Unit Test)는 애플리케이션의 개별적인 모듈(함수, 클래스, 메서드 등)이 올바르게 동작하는지 검증하는 테스트 방법이다.

### **1.1 단위 테스트의 특징**
✅ **개별적인 함수나 메서드를 독립적으로 테스트**  
✅ **빠른 실행 속도**  
✅ **테스트의 자동화가 용이함**  
✅ **Mocking을 활용하여 의존성을 최소화**

### **1.2 단위 테스트의 목적**
- 코드의 작은 단위(함수, 메서드, 클래스 등)가 기대한 대로 동작하는지 검증
- 버그를 조기에 발견하고 수정하여 유지보수 비용 절감
- 리팩토링 및 코드 변경 시 안정성을 보장

### **1.3 단위 테스트 예제 (Java - JUnit5 사용)**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    Calculator calculator = new Calculator();

    @Test
    void testAdd() {
        assertEquals(5, calculator.add(2, 3)); // 2 + 3 = 5
    }

    @Test
    void testSubtract() {
        assertEquals(1, calculator.subtract(3, 2)); // 3 - 2 = 1
    }
}
```

**설명**
- `testAdd()`: `add(2,3)`의 결과가 `5`인지 확인
- `testSubtract()`: `subtract(3,2)`의 결과가 `1`인지 확인

### **1.4 단위 테스트의 장점과 단점**

#### ✅ 장점
- **빠른 실행 속도**: 단일 함수 또는 메서드를 테스트하므로 실행 속도가 빠름
- **디버깅이 쉬움**: 특정 함수에서 버그 발생 시 쉽게 원인을 찾을 수 있음
- **코드 품질 향상**: 테스트 코드를 작성하면서 자연스럽게 모듈화와 유지보수성을 고려하게 됨

#### ❌ 단점
- **비즈니스 로직 전체를 검증하기 어려움**: 작은 단위의 테스트만 수행하므로 시스템 전체적인 동작을 보장할 수 없음
- **Mocking이 필요함**: 데이터베이스, 네트워크 등 외부 의존성이 있는 경우, 가짜(Mock) 데이터를 생성해야 함

---

## **2. 통합 테스트(Integration Test)란?**
통합 테스트(Integration Test)는 여러 개의 모듈이 올바르게 상호작용하는지 검증하는 테스트 방법이다.

### **2.1 통합 테스트의 특징**
✅ **여러 모듈을 결합하여 테스트**  
✅ **실제 환경과 유사한 방식으로 실행**  
✅ **데이터베이스, API 등과의 연동을 포함할 수 있음**  
✅ **단위 테스트보다 실행 속도가 느림**

### **2.2 통합 테스트의 목적**
- 단위 테스트를 통과한 모듈들이 결합될 때, 정상적으로 동작하는지 확인
- 데이터베이스, 네트워크, API 등 실제 외부 시스템과의 연동을 검증
- 예상하지 못한 시스템 간 충돌을 조기에 발견

### **2.3 통합 테스트 예제 (Spring Boot - JUnit & Testcontainers 사용)**

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.jdbc.AutoConfigureTestDatabase;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.jdbc.Sql;
import static org.assertj.core.api.Assertions.assertThat;

@SpringBootTest
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE) // 실제 DB 사용
public class UserRepositoryTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    @Sql("/test-data.sql") // 테스트용 데이터 삽입
    void testFindByUsername() {
        User user = userRepository.findByUsername("testUser");
        assertThat(user).isNotNull();
        assertThat(user.getUsername()).isEqualTo("testUser");
    }
}
```

**설명**
- `@SpringBootTest`: Spring 컨텍스트를 로드하여 전체 애플리케이션 환경에서 테스트
- `@Sql("/test-data.sql")`: 테스트 실행 전에 데이터베이스에 `testUser` 데이터를 삽입
- `userRepository.findByUsername("testUser")`가 정상적으로 동작하는지 검증

### **2.4 통합 테스트의 장점과 단점**

#### ✅ 장점
- **실제 환경과 유사하게 테스트 가능**
- **모듈 간의 호환성 검증 가능**
- **배포 후 발생할 수 있는 시스템 오류를 사전에 발견 가능**

#### ❌ 단점
- **실행 속도가 느림** (DB, 네트워크 연동 등으로 인해 시간이 오래 걸림)
- **디버깅이 어려울 수 있음** (어느 모듈에서 문제 발생했는지 추적이 어려움)
- **테스트 환경 준비가 필요함** (데이터베이스, API 서버 등과의 연동이 필요함)

---

## **3. 단위 테스트 vs 통합 테스트 비교**

|  | **단위 테스트 (Unit Test)** | **통합 테스트 (Integration Test)** |
|-----------------|--------------------------------|--------------------------------|
| **목적** | 개별 모듈(함수, 클래스)이 올바르게 동작하는지 검증 | 여러 모듈이 상호작용할 때 정상적으로 동작하는지 검증 |
| **테스트 범위** | 개별 함수, 메서드, 클래스 | 여러 개의 모듈 또는 전체 시스템 |
| **실행 속도** | 빠름 | 느림 |
| **의존성** | Mock을 활용하여 독립적으로 테스트 | 실제 데이터베이스, API 등과 연동하여 테스트 |
| **버그 발견** | 개별 기능의 버그 발견 | 시스템 간 충돌 및 연동 오류 발견 |
| **테스트 용이성** | 코드가 단순하고 유지보수하기 쉬움 | 환경 설정이 복잡하고 디버깅이 어려움 |

---

## **4. 결론**
- **단위 테스트(Unit Test)**는 **빠르고 독립적인 테스트**를 통해 개별 함수나 메서드가 올바르게 동작하는지 검증한다.
- **통합 테스트(Integration Test)**는 **여러 모듈을 결합하여 전체적인 동작을 검증**하며, 데이터베이스나 API 등 실제 환경과의 연동을 테스트한다.
- **두 테스트는 상호보완적**이며, **단위 테스트 → 통합 테스트 → 시스템 테스트 → 배포**의 단계를 거쳐 안정성을 확보해야 한다.

따라서, **단위 테스트로 기본적인 기능을 검증한 후, 통합 테스트를 통해 시스템의 정상적인 작동을 보장하는 것이 중요하다.** 🚀