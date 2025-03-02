### Dependency Injection (DI) 정리

---

## 1. DI(Dependency Injection)란?

DI는 **의존성 주입**을 의미하며, 객체 간의 의존성을 외부에서 주입받는 방식이다.

### 🔹 의존성이란?
- 어떤 **클라이언트(consumer)** 가 **서비스(service)** 를 호출할 때, 서비스가 어떻게 구성되었는지 알지 않아야 한다.
- 클라이언트는 직접 서비스 객체를 생성하는 것이 아니라 **외부 코드(주입자)** 로부터 주입받아야 한다.
- 이를 통해 **클라이언트는 서비스 구성 방식과 실제 사용되는 서비스에 대해 알 필요가 없다.**

### 🔹 DI의 핵심 개념
- **"클라이언트"** : 특정 서비스의 사용 방식만 정의하고, 실제 구현을 알 필요가 없다.
- **구성의 책임과 사용의 책임을 분리한다.**

---

## 2. DI 미사용 코드 예시 (결합도가 높은 코드)

```java
public class Coffee { ... }

public class Programmer {
    private Coffee coffee = new Coffee();  // 직접 객체 생성

    public void startProgramming() {
        this.coffee.drink();
    }
}
```
**📌 문제점**
- `Programmer` 객체는 `Coffee` 객체에 **강하게 의존**하고 있다.
- `Cappuccino`, `Americano` 등의 다른 커피를 사용하려면 코드를 직접 수정해야 한다.
- 결합도가 높아져 **유연성이 떨어지고 유지보수성이 낮아진다.**

---

## 3. DI 적용 코드 (결합도를 낮춘 코드)

```java
public class Coffee { ... }
public class Cappuccino extends Coffee { ... }
public class Americano extends Coffee { ... }

public class Programmer {
    private Coffee coffee;

    public Programmer(Coffee coffee) { // 의존성 주입
        this.coffee = coffee;
    }

    public void startProgramming() {
        this.coffee.drink();
    }
}
```

**📌 장점**
- `Coffee` 객체를 **외부에서 주입**할 수 있다.
- `Programmer` 객체를 수정하지 않고 `Cappuccino`, `Americano` 를 주입할 수 있어 **유연성과 재사용성이 증가**한다.

---

## 4. 객체지향 원칙과 DI

DI는 **Liskov Substitution Principle (LSP, 리스코프 치환 원칙)** 과 관련이 있다.

### 🔹 LSP란?
- 자식 클래스가 부모 클래스를 대체할 수 있어야 한다.
- 즉, `Coffee coffee = new Americano();` 와 같은 코드가 가능해야 한다.

📌 `Coffee` 가 부모 클래스이고, `Cappuccino`, `Americano` 가 이를 상속하면 다음 관계가 성립:
```
Coffee is a Coffee
Cappuccino is a Coffee
Americano is a Coffee
```
이를 통해 DI를 적용하면 다양한 하위 클래스를 쉽게 교체할 수 있다.

---

## 5. DI의 장점

✅ **유닛 테스트(Unit Test)가 용이**  
✅ **코드의 재사용성이 증가**  
✅ **객체 간 의존성을 줄이거나 제거 가능**  
✅ **객체 간 결합도를 낮춰 유연한 코드 작성 가능**

---

## 6. Spring과 DI

Spring에서는 DI를 편리하게 사용할 수 있도록 지원하며, 주입 방법은 다음 세 가지가 있다.

### 1️⃣ 생성자 주입 (Constructor Injection) ⭐ **(권장 방법)**

```java
@Service 
public class UserServiceImpl implements UserService {   
    private final UserRepository userRepository; 
    private final MemberService memberService; 

    @Autowired
    public UserServiceImpl(UserRepository userRepository, MemberService memberService) { 
        this.userRepository = userRepository; 
        this.memberService = memberService; 
    } 
}
```

**📌 특징**
- 생성자가 호출될 때 **단 한 번만 주입됨** (불변성 유지)
- `final` 키워드 사용 가능 (의존성을 변경할 수 없도록 강제)
- **Spring에서는 생성자가 하나만 있으면 `@Autowired` 생략 가능**

---

### 2️⃣ 수정자 주입 (Setter Injection)

```java
@Service 
public class UserServiceImpl implements UserService { 
    private UserRepository userRepository; 
    private MemberService memberService; 

    @Autowired 
    public void setUserRepository(UserRepository userRepository) { 
        this.userRepository = userRepository; 
    } 

    @Autowired 
    public void setMemberService(MemberService memberService) { 
        this.memberService = memberService; 
    } 
}
```

**📌 특징**
- `setter` 를 사용하여 주입 (객체 변경 가능)
- 필수적인 의존성이 아닐 경우 사용 가능 (`@Autowired(required = false)` 사용)

---

### 3️⃣ 필드 주입 (Field Injection) ❌ **(지양해야 함)**

```java
@Service 
public class UserServiceImpl implements UserService { 
    @Autowired
    private UserRepository userRepository; 

    @Autowired
    private MemberService memberService; 
}
```

**📌 문제점**
- **외부에서 변경이 불가능**하여 테스트 코드 작성이 어려움
- DI 프레임워크(Spring 등)가 없으면 동작하지 않음
- **권장하지 않으며, 설정 파일이나 테스트 코드에서만 사용해야 함**

---

## 7. 생성자 주입을 권장하는 이유

✅ **객체의 불변성 확보** (생성과 동시에 주입)  
✅ **테스트 코드 작성 용이**  
✅ `final` 키워드 사용 가능 (`Lombok`의 `@RequiredArgsConstructor` 활용 가능)  
✅ **순환 참조(Circular Dependency) 에러 방지**

- 생성자 주입은 객체 생성 시점에서 순환 참조를 탐지하여 **즉시 에러를 발생**시킨다.
- 반면, **수정자 주입은 순환 참조를 실행 중에 발견할 가능성이 높아 디버깅이 어렵다.**

---

## 8. 정리

| 주입 방식 | 특징 | 장점 | 단점 | 권장 여부 |
|-----------|------|------|------|---------|
| **생성자 주입** | 생성자 호출 시 1회만 주입됨 | - 불변성 확보 <br> - `final` 사용 가능 <br> - 순환 참조 방지 | 없음 | ✅ **권장** |
| **수정자 주입** | Setter 메서드로 주입 | - 선택적 주입 가능 <br> - DI 프레임워크 없이 사용 가능 | - `final` 사용 불가 <br> - 순환 참조 탐지 어려움 | ⛔ **가급적 지양** |
| **필드 주입** | 필드에 직접 주입 | - 코드가 간결함 | - 테스트 어려움 <br> - DI 프레임워크 필수 <br> - 변경 불가능 | ❌ **지양** |

📌 **Spring에서는 생성자 주입을 기본적으로 사용하자!** 🚀