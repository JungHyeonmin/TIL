# ✅ 1. **FetchType.Lazy (지연 로딩)**

## 🔷 개념

* 연관된 엔티티(예: `@ManyToOne`, `@OneToMany` 등)를 **즉시 DB에서 가져오지 않고**, 프록시 객체(가짜 객체)로 대신하여 **실제 참조 시점에 DB에서 로딩**함.
* JPA는 `ByteBuddy` 등의 프록시 기술을 이용하여, `get()` 같은 메서드를 호출할 때 DB 접근을 수행함.

### 📌 예시

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.LAZY)
    private Member member;
}
```

```java
Order order = orderRepository.findById(1L).get(); // 이 시점에는 member가 로딩되지 않음
order.getMember().getName(); // 이 시점에 쿼리가 실행됨
```

## 🔷 언제 사용하는가?

* 성능 최적화가 중요한 경우 (대부분의 경우)
* 연관 객체가 항상 필요하지 않은 경우
* 대규모 테이블, 많은 관계가 걸린 객체 간 로딩이 우려되는 경우
* 복잡한 객체 그래프를 가진 도메인 설계 시

## 🔷 장점

* 초기 쿼리가 간단하여 **쿼리 성능 최적화에 유리**
* **필요한 데이터만 조회**하므로 네트워크, 메모리 자원 절약
* 엔티티 관계가 많을수록 **복잡한 조인 없이 처리 가능**

## 🔷 단점

* 연관 객체 접근 시점에 DB 접근이 발생 → **예측하기 어려운 지연 쿼리**
* 영속성 컨텍스트 종료 후 프록시 접근 시 → `LazyInitializationException`
* 디버깅이나 로그 출력 시 프록시 객체가 혼동될 수 있음

---

# ✅ 2. **FetchType.Eager (즉시 로딩)**

## 🔷 개념

* 연관된 엔티티를 **즉시 로딩하여 한 번의 SELECT (Join 포함) 쿼리로 모두 가져옴**
* JPA는 `JOIN`을 포함하거나 `서브쿼리` 등을 통해 내부적으로 관련 데이터를 함께 조회함

### 📌 예시

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.EAGER)
    private Member member;
}
```

```java
Order order = orderRepository.findById(1L).get(); // 이 시점에서 member도 함께 조회됨 (Join 발생)
```

## 🔷 언제 사용하는가?

* 연관 객체를 **항상 함께 사용**하는 경우 (ex: 코드 테이블, 권한 정보 등)
* **조회 수가 적고**, 객체 관계가 단순한 경우
* 트랜잭션 외부에서 객체가 사용되어 **Lazy 로딩이 불가능한 환경** (ex: DTO 직렬화, API 응답)일 때

## 🔷 장점

* 한 번의 쿼리로 전체 관계를 로딩하여, 이후 추가적인 쿼리 없음
* 트랜잭션 범위를 벗어난 경우에도 연관 객체 접근이 가능
* 디버깅 및 직렬화(예: JSON 변환) 시 에러 가능성 낮음

## 🔷 단점

* **불필요한 데이터 조회**가 발생할 수 있음
* 조인으로 인해 쿼리 복잡도 및 수행 시간 증가
* **N+1 문제** 발생 가능성 높음 (특히 컬렉션 관계에서)
* 엔티티의 재사용성이 떨어짐 (모든 관계를 끌고 다님)

---

# ✅ 3. LAZY vs EAGER 비교 정리

| 항목        | LAZY (지연 로딩)                     | EAGER (즉시 로딩)                |
| --------- | -------------------------------- | ---------------------------- |
| 로딩 시점     | 실제 사용 시 (접근 시점에 쿼리 실행)           | 엔티티 로딩 시 즉시 함께 로딩 (Join 쿼리)  |
| 초기 쿼리     | 단순함 (JOIN 없음)                    | 복잡함 (JOIN 포함, 서브쿼리 가능성)      |
| 성능        | 효율적 (필요한 순간만 로딩)                 | 비효율적일 수 있음 (불필요한 데이터 조회)     |
| 예외 발생 가능성 | `LazyInitializationException` 주의 | 없음 (이미 로딩되어 있음)              |
| 사용 권장 상황  | 대부분의 연관 관계                       | 코드 테이블, 로그인 사용자 등 항상 참조하는 객체 |
| N+1 문제 위험 | 낮음 (프록시 처리)                      | 높음 (컬렉션에서 반드시 해결 필요)         |
| 디버깅 및 직렬화 | 프록시 주의 필요                        | 안정적                          |

---

# ✅ 실무 가이드라인 및 전략적 사용 방법

### ✅ 실무에서 기본 전략

* **모든 관계는 LAZY로 설정**하고, 필요한 곳에서만 `fetch join`이나 `@EntityGraph`를 사용하여 로딩
* 즉시 로딩은 관계가 단순하거나, 반복적인 접근이 예측될 경우에만 제한적으로 사용

### ✅ `@EntityGraph` 예시 (EAGER처럼 작동하지만 쿼리 명시적 제어)

```java
@EntityGraph(attributePaths = {"member"})
@Query("SELECT o FROM Order o WHERE o.id = :id")
Order findWithMember(@Param("id") Long id);
```

### ✅ fetch join 예시

```java
@Query("SELECT o FROM Order o JOIN FETCH o.member WHERE o.id = :id")
Order findByIdWithMember(@Param("id") Long id);
```

---

# ✅ 결론 (의사결정 정리)

| 기준                  | 선택 전략                          |
| ------------------- | ------------------------------ |
| 연관 객체를 항상 사용함       | `EAGER` 또는 `Fetch Join` 사용     |
| 연관 객체를 가끔 사용하거나 조건부 | `LAZY`                         |
| 객체 그래프가 깊고 복잡함      | 무조건 `LAZY`, DTO 사용 분리          |
| 트랜잭션 밖에서 접근해야 함     | `EAGER` 혹은 데이터 추출 전략 변경        |
| API 응답 시            | DTO 매핑 또는 `@EntityGraph` 사용 권장 |
