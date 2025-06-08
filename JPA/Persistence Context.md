### **JPA 영속성(Persistence)이란?**
JPA(Java Persistence API)에서 영속성(Persistence)이란, **객체가 데이터베이스에 저장되고 관리되는 과정**을 의미합니다. 쉽게 말해, **객체를 생성하고 변경한 내용을 데이터베이스와 동기화하는 메커니즘**입니다.

---

## **1. JPA의 영속성 컨텍스트(Persistence Context)**
### 🔹 **영속성 컨텍스트란?**
JPA에서 영속성(Persistence)을 관리하는 핵심 요소가 **영속성 컨텍스트(Persistence Context)** 입니다.  
**영속성 컨텍스트란, 엔티티를 영속 상태로 관리하는 메모리 공간(1차 캐시)** 이며, `EntityManager` 가 이를 관리합니다.

> **EntityManager**: JPA에서 엔티티의 생성, 조회, 수정, 삭제를 담당하는 인터페이스

### 🔹 **영속성 컨텍스트의 주요 특징**
1. **1차 캐시**
    - 영속성 컨텍스트는 **엔티티를 메모리에 캐싱(저장)** 합니다.
    - 같은 트랜잭션 내에서 동일한 엔티티를 조회할 때, **DB 조회 없이 1차 캐시에서 가져옵니다.**

2. **트랜잭션을 통한 자동 변경 감지 (Dirty Checking)**
    - 엔티티의 필드를 변경하면, `EntityManager`가 이를 감지하여 **트랜잭션이 끝날 때 자동으로 UPDATE 쿼리를 실행**합니다.

3. **쓰기 지연 (Write-Behind)**
    - `persist()`를 호출하면 즉시 DB에 저장되지 않고, **트랜잭션이 커밋될 때 한꺼번에 SQL을 실행**합니다.

4. **지연 로딩 (Lazy Loading)**
    - `fetch = FetchType.LAZY` 설정 시, 연관된 엔티티를 실제 사용할 때까지 로딩하지 않습니다.
    - 이를 통해 **불필요한 SQL 실행을 방지**하고 성능을 최적화할 수 있습니다.

---

## **2. 엔티티의 생명주기**
JPA에서 엔티티는 **비영속, 영속, 준영속, 삭제** 4가지 상태를 가집니다.

### 📌 **1) 비영속 (Transient)**
- **JPA와 관련 없이 단순히 객체를 생성한 상태**
- 아직 `EntityManager`가 관리하지 않음 (1차 캐시에 없음)

```java
Member member = new Member();
member.setName("JPA 유저");  // 비영속 상태
```

---

### 📌 **2) 영속 (Managed)**
- **EntityManager를 통해 영속성 컨텍스트에 저장된 상태**
- 1차 캐시에 저장되며, 트랜잭션이 끝날 때 DB에 반영됨

```java
EntityManager em = emf.createEntityManager();
EntityTransaction tx = em.getTransaction();
tx.begin();  // 트랜잭션 시작

Member member = new Member();
member.setName("JPA 유저");
em.persist(member);  // 영속 상태

tx.commit();  // DB에 INSERT 실행
```
**✅ `persist()`를 호출하면, 영속성 컨텍스트에서 관리되며 트랜잭션이 커밋될 때 `INSERT` 쿼리가 실행됩니다.**

---

### 📌 **3) 준영속 (Detached)**
- **EntityManager의 관리에서 벗어난 상태**
- 더 이상 변경 사항이 DB에 반영되지 않음

```java
em.detach(member);  // 준영속 상태
member.setName("변경된 이름");  // UPDATE가 실행되지 않음
```

---

### 📌 **4) 삭제 (Removed)**
- **엔티티가 DB에서도 삭제될 예정인 상태**
- `remove()` 호출 시, 트랜잭션이 커밋되면 `DELETE` 실행됨

```java
em.remove(member);  // 삭제 상태
```

---

## **3. 영속성 컨텍스트가 제공하는 주요 기능**
### **1️⃣ 1차 캐시 (First-Level Cache)**
- 같은 트랜잭션 내에서 **같은 엔티티를 다시 조회할 때 DB를 조회하지 않고 1차 캐시에서 가져옵니다.**

```java
Member findMember1 = em.find(Member.class, 1L); // DB에서 조회 후 1차 캐시에 저장
Member findMember2 = em.find(Member.class, 1L); // 1차 캐시에서 조회 (쿼리 실행 X)
```

> **🔥 성능 최적화 효과**
> - 동일한 엔티티를 여러 번 조회해도 DB에 불필요한 요청을 하지 않음
> - 애플리케이션 성능을 향상시킴

---

### **2️⃣ 변경 감지 (Dirty Checking)**
- 엔티티의 값을 변경하면, `EntityManager`가 이를 감지하여 트랜잭션이 끝날 때 **자동으로 UPDATE** 쿼리를 실행합니다.

```java
Member member = em.find(Member.class, 1L);
member.setName("변경된 이름"); // 값을 변경

tx.commit(); // UPDATE 쿼리 실행
```

> **🔥 장점**
> - `UPDATE` 쿼리를 직접 호출하지 않아도 됨
> - 변경된 데이터만 반영하므로 **불필요한 SQL 실행 방지**

---

### **3️⃣ 지연 로딩 (Lazy Loading)**
- `@OneToMany`, `@ManyToOne` 관계에서 기본적으로 `LAZY` 설정을 사용하면 **필요할 때만 데이터를 조회**합니다.

```java
@Entity
public class Member {
    @ManyToOne(fetch = FetchType.LAZY)  // 지연 로딩 설정
    private Team team;
}

Member member = em.find(Member.class, 1L);
Team team = member.getTeam(); // 이때 SQL 실행됨 (프록시 객체가 실제로 접근할 때 조회)
```

> **🔥 장점**
> - 불필요한 연관 데이터를 미리 조회하지 않아 성능 최적화 가능

---

### **4️⃣ 쓰기 지연 (Write-Behind)**
- `persist()`를 호출해도 즉시 `INSERT` 쿼리를 실행하지 않고, 트랜잭션이 커밋될 때 한꺼번에 처리합니다.

```java
Member member1 = new Member();
Member member2 = new Member();
em.persist(member1);
em.persist(member2);

tx.commit(); // INSERT 쿼리 한 번에 실행됨
```

> **🔥 장점**
> - 여러 개의 INSERT, UPDATE, DELETE 쿼리를 한꺼번에 실행해 성능 최적화 가능

---

## **4. 영속성이 중요한 이유**
1. **성능 최적화**
    - **1차 캐시**를 활용해 불필요한 DB 조회를 방지
    - **쓰기 지연**을 통해 한 번에 SQL 실행
    - **지연 로딩**으로 필요한 시점에만 데이터를 조회

2. **코드 단순화**
    - `persist()`, `commit()` 만으로 SQL 처리를 자동화할 수 있음
    - `Dirty Checking` 으로 변경된 내용만 자동 반영

3. **데이터 일관성 유지**
    - 트랜잭션 단위로 엔티티 상태를 관리하여 동시성 문제를 줄일 수 있음

---

## **5. 정리**
| 상태 | 설명 |
|------|------|
| **비영속 (Transient)** | JPA와 관련 없는 객체 |
| **영속 (Managed)** | `persist()` 호출 후 영속성 컨텍스트에서 관리됨 |
| **준영속 (Detached)** | `detach()` 호출하여 관리가 중지된 상태 |
| **삭제 (Removed)** | `remove()` 호출 후 트랜잭션 커밋 시 삭제 |

**🔥 JPA의 영속성 컨텍스트를 활용하면, 성능 최적화와 코드 유지보수가 쉬워지므로 효율적인 데이터 관리를 할 수 있다!** 🚀