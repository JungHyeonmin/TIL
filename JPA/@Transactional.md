## ✅ 개념

`@Transactional`은 **Spring Framework**에서 제공하는 선언적 트랜잭션 관리 어노테이션으로,
해당 메서드 실행을 하나의 **트랜잭션 단위로 처리**할 수 있도록 보장한다.

즉, 메서드 내부에서 수행되는 모든 DB 작업은 \*\*성공 시 전부 반영(commit)\*\*되고,
\*\*예외 발생 시 전부 취소(rollback)\*\*된다.

---

## ✅ 동작 원리

### AOP 기반 트랜잭션 처리

* Spring은 `@Transactional`이 선언된 메서드를 **프록시 객체로 감싸며 트랜잭션을 관리**한다.
* 프록시는 메서드 진입 시 트랜잭션을 시작하고, 종료 시 커밋 또는 롤백을 수행한다.

```java
@Transactional
public void doBusinessLogic() {
    // 트랜잭션 시작
    repository.save(...);
    repository.delete(...);
    // 트랜잭션 커밋 or 롤백
}
```

### 내부 동작 흐름

1. 메서드 진입 → 트랜잭션 시작
2. EntityManager 및 영속성 컨텍스트 활성화
3. 비즈니스 로직(DB 접근 포함) 실행
4. 정상 종료 시 `commit()`, 예외 발생 시 `rollback()`
5. 영속성 컨텍스트 종료

---

## ✅ 트랜잭션 전파/속성 설정

| 속성            | 설명                               |
| ------------- | -------------------------------- |
| `propagation` | 전파 방식 (기본: `REQUIRED`)           |
| `rollbackFor` | 롤백을 유발할 예외 지정                    |
| `readOnly`    | 읽기 전용 여부 (select만 수행할 경우 최적화 가능) |
| `timeout`     | 트랜잭션 제한 시간                       |

```java
@Transactional(readOnly = true, timeout = 5)
public List<Post> getPosts() { ... }
```

---

## ✅ 실무 적용 기준

| 상황            | 트랜잭션 적용 위치                            |
| ------------- | ------------------------------------- |
| 비즈니스 로직 포함    | **Service 계층**에서 `@Transactional` 적용  |
| 단순 조회         | `readOnly = true` 설정 시 성능 향상 가능       |
| Repository 단독 | 이미 내부적으로 트랜잭션 적용 (주의: 복수 작업엔 적합하지 않음) |

---

## ✅ Lazy 로딩과의 관계

JPA에서 `LAZY` 로딩은 \*\*실제로 객체를 사용하는 시점(getter 호출 등)\*\*에 SELECT 쿼리를 날린다.

이때 **트랜잭션이 종료되어 있으면**, 영속성 컨텍스트가 닫혀 있기 때문에
**`LazyInitializationException` 예외가 발생**한다.

### 해결 방법

* **서비스 계층에서 반드시 `@Transactional`을 걸어야 Lazy 로딩이 가능**
* 프록시 객체가 데이터를 로드할 수 있도록 트랜잭션과 DB 세션이 살아 있어야 함

---

## ✅ 주의 사항

| 항목            | 주의 내용                                       |
| ------------- | ------------------------------------------- |
| 내부 메서드 호출     | 같은 클래스 내 메서드 호출은 트랜잭션 적용되지 않음 (프록시 우회)      |
| 커밋 시점         | 메서드 종료 시 커밋됨, 중간에 예외가 발생해야 롤백               |
| 체크 예외         | 기본 설정은 체크 예외 발생 시 롤백 안 함 → `rollbackFor` 필요 |
| Controller 계층 | 트랜잭션 적용 **X**, 비즈니스 로직은 Service로 위임해야 함     |

---

## ✅ 결론

* `@Transactional`은 DB 작업의 **일관성과 원자성**을 보장하는 핵심 어노테이션
* Lazy 로딩이 정상 작동하기 위해서도 트랜잭션 환경이 필수
* 트랜잭션 범위는 메서드 단위로 유지되며, 해당 범위 안에서 DB 세션과 영속성 컨텍스트가 살아 있음
* 실무에서는 대부분 **Service 계층**에 명시적으로 선언하여 명확한 트랜잭션 경계를 유지

---

## 📌 참고 예시: Lazy 로딩 보장

```java
@Transactional
public Post getPostWithWriter(Long id) {
    Post post = postRepository.findById(id).orElseThrow();
    post.getWriter().getName(); // Lazy 로딩 안전하게 가능
    return post;
}
```