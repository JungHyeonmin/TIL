## 🧱 1. DTO (Data Transfer Object)

### ✅ 개념

* 계층 간 데이터 전달을 목적으로 사용하는 객체
* **Controller → Service → DAO** 등 여러 레이어 간 데이터를 주고받을 때 사용
* 비즈니스 로직을 포함하지 않으며 **순수 데이터 전달용**

### ✅ 왜 사용하는가?

* 불필요한 정보 노출 방지 (예: Entity를 직접 노출하면 보안상 위험)
* 필요한 데이터만 추려서 전달하여 **성능 향상**
* 유지보수성 향상: API 스펙 변경에 유연하게 대응 가능

### ✅ 어디서 많이 사용하는가?

* REST API 응답 및 요청 데이터 구조
* Service ↔ Controller 간 데이터 전달

### ✅ 예시 (Java 코드)

```java
public class UserDTO {
    private String username;
    private String email;

    // 생성자, Getter, Setter
}
```

### 🔄 예전에는?

* DTO 없이 Entity를 직접 전달했음 → **보안 문제, 데이터 과다 노출 문제**

---

## 🧱 2. VO (Value Object)

### ✅ 개념

* **값 그 자체**를 표현하는 객체
* 불변(immutable)한 객체이며, **값이 같으면 같은 객체로 간주**
* 주로 비즈니스 도메인에서 사용

### ✅ 왜 사용하는가?

* **객체 지향적인 설계**를 위해 사용 (ex: Money, Address, Email 등)
* **불변 객체**로 설계되어 안정성 보장

### ✅ 어디서 많이 사용하는가?

* 도메인 모델링
* 금액, 주소, 이메일 같은 **값 중심 객체** 표현

### ✅ 예시 (Java 코드)

```java
public class Money {
    private final int amount;

    public Money(int amount) {
        this.amount = amount;
    }

    public int getAmount() {
        return amount;
    }

    // 값이 같으면 동일한 객체로 판단
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Money)) return false;
        Money money = (Money) o;
        return amount == money.amount;
    }

    @Override
    public int hashCode() {
        return Objects.hash(amount);
    }
}
```

### 🔄 예전에는?

* int나 String으로 단순 값만 전달 → **의미가 불명확하고 유지보수 어려움**

---

## 🧱 3. AO (Application Object 또는 Action Object)

### ✅ 개념

* 특정 프레임워크나 계층에서 동작하는 **작업 단위의 객체**
* 예를 들어 **Struts 프레임워크**에서 사용자 요청을 처리하는 객체

> 현대적인 구조에서는 잘 사용되지 않으며, 과거 프레임워크 중심의 아키텍처에서 등장

### ✅ 왜 사용하는가?

* MVC 구조에서 **Controller 역할 수행**
* 사용자 요청에 대한 처리 로직 포함

### ✅ 어디서 많이 사용하는가?

* **Struts**, **Spring MVC** 같은 프레임워크 (특히 과거 시스템)

### ✅ 예시

```java
public class LoginAO {
    private String username;
    private String password;

    public String execute() {
        // 로그인 처리 로직
        return "success";
    }
}
```

---

## 📈 발전 과정 요약

| 시대 | 구조                               | 사용 객체            | 문제점                |
| -- | -------------------------------- | ---------------- | ------------------ |
| 과거 | 단순 JSP/Servlet                   | Entity만 사용       | 보안/유지보수 문제         |
| 발전 | MVC 도입                           | AO 등장 (Struts 등) | 프레임워크 종속, 유지보수 어려움 |
| 현대 | 계층 분리 (Controller, Service, DAO) | DTO, VO 명확하게 구분  | 객체 역할 명확, 유지보수 용이  |

---

## 📌 정리

| 객체  | 목적     | 특징                      | 대표 사용처               |
| --- | ------ | ----------------------- | -------------------- |
| DTO | 데이터 전달 | 가변, 로직 없음               | Controller ↔ Service |
| VO  | 값 표현   | 불변, equals/hashCode 재정의 | 도메인 모델               |
| AO  | 요청 처리  | 프레임워크 종속, 로직 포함         | 옛날 MVC 구조 (Struts 등) |