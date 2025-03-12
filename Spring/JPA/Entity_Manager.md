## Entity Manager란?
**Entity Manager(엔티티 매니저)**는 **JPA(Java Persistence API)**에서 **영속성 컨텍스트(Persistence Context)**를 관리하는 핵심 인터페이스입니다.  
쉽게 말해, **데이터베이스와 객체(Entity) 간의 상호작용을 중개하는 역할**을 합니다.

### 📌 주요 기능
1. **엔티티의 생성 및 저장 (`persist`)**
2. **엔티티의 조회 (`find`, `getReference`)**
3. **엔티티의 수정 (변경 감지)**
4. **엔티티의 삭제 (`remove`)**
5. **트랜잭션 관리**
6. **JPQL(Query) 실행 (`createQuery`)**
7. **영속성 컨텍스트 관리 (`clear`, `detach`, `close`)**

---

## 🏗 Entity Manager의 내부 동작 원리
### 1️⃣ 영속성 컨텍스트(Persistence Context)란?
- 엔티티 매니저가 관리하는 **엔티티 객체의 저장소(캐시)**
- **1차 캐시** 역할을 하며, 데이터베이스와의 직접적인 상호작용을 최소화함
- 영속 상태의 엔티티는 자동으로 변경 사항이 감지되어 데이터베이스에 반영됨

### 2️⃣ Entity Manager의 라이프사이클
#### 엔티티 상태 변화
1. **비영속 (Transient)**
    - `new` 키워드로 엔티티 객체 생성 후 아직 관리되지 않는 상태
   ```java
   Member member = new Member();
   member.setName("John");
   ```
2. **영속 (Persistent)**
    - `persist()` 호출하여 **영속성 컨텍스트에 저장**됨
   ```java
   em.persist(member); // 영속 상태가 됨
   ```
    - 이 시점에서 INSERT 쿼리는 실행되지 않음(트랜잭션 commit 시 실행)
3. **준영속 (Detached)**
    - `detach()`, `clear()`, `close()` 호출 시 영속성 컨텍스트에서 분리됨
   ```java
   em.detach(member); // 더 이상 변경 감지가 되지 않음
   ```
4. **삭제 (Removed)**
    - `remove()` 호출하면 엔티티가 영속성 컨텍스트에서 제거됨
   ```java
   em.remove(member);
   ```

---

## ✨ Entity Manager 주요 메서드 정리
| 메서드 | 설명 |
|--------|-----|
| `persist(entity)` | 엔티티를 영속성 컨텍스트에 저장 |
| `find(entityClass, primaryKey)` | 엔티티 조회 (1차 캐시 → DB 조회) |
| `getReference(entityClass, primaryKey)` | 프록시 객체 조회 (실제 객체 조회 지연) |
| `remove(entity)` | 엔티티 삭제 |
| `detach(entity)` | 특정 엔티티를 준영속 상태로 변경 |
| `clear()` | 영속성 컨텍스트 초기화 |
| `close()` | 엔티티 매니저 종료 |
| `flush()` | 영속성 컨텍스트 변경 사항을 DB에 반영 |
| `merge(entity)` | 준영속 엔티티를 다시 영속 상태로 변경 |
| `createQuery(query, class)` | JPQL 실행을 위한 쿼리 객체 생성 |

---

## 🚀 Entity Manager의 필요성
1. **트랜잭션을 관리하며 데이터 정합성을 유지**
2. **객체 중심 개발이 가능 (SQL이 아닌 엔티티 기반 개발)**
3. **1차 캐시를 통한 성능 최적화**
4. **변경 감지(Dirty Checking)로 자동 업데이트 반영**

### ❌ Entity Manager 없이 어떻게 했을까?
- JDBC 또는 MyBatis를 이용하여 SQL을 직접 실행
- SQL 작성 및 결과 매핑 과정이 필요해 코드가 복잡해짐
- 트랜잭션을 수동으로 관리해야 함

---

## 🎯 정리
- `EntityManager`는 **JPA에서 영속성 컨텍스트를 관리하는 핵심 객체**
- `persist()`, `find()`, `remove()`, `clear()` 등의 메서드를 제공
- **1차 캐시, 변경 감지(Dirty Checking), 트랜잭션 관리** 등의 기능을 통해 데이터 처리 효율성을 높임
- 기존 SQL 기반 개발보다 **객체 중심의 개발이 가능**하며 **자동화된 관리**가 가능

➡ **JPA를 사용한 백엔드 개발에서는 `EntityManager`를 잘 활용하는 것이 필수!** 🚀