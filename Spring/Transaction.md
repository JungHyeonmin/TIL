# 트랜잭션(Transaction)

## 1. 트랜잭션이란?
트랜잭션(Transaction)은 **하나의 논리적인 작업 단위(작업 묶음)**를 의미하며, 데이터베이스에서 여러 개의 작업(쿼리)이 실행될 때 **모두 성공하거나, 모두 실패해야 하는 원자적(Atomic) 연산**을 보장하는 개념입니다.

예를 들어, 은행 계좌 이체를 수행할 때 다음과 같은 두 가지 작업이 필요합니다.

1. A 계좌에서 100만 원을 출금
2. B 계좌로 100만 원을 입금

위 두 가지 작업 중 하나라도 실패하면, 전체 작업을 취소해야 합니다. 만약 1번 작업이 수행되었는데 2번 작업이 실패한다면, 돈이 사라지는 문제가 발생할 수 있습니다. 이런 문제를 방지하기 위해 트랜잭션을 사용합니다.

---

## 2. 트랜잭션의 ACID 특성
트랜잭션은 **ACID**라고 불리는 4가지 중요한 특성을 보장해야 합니다.

| 특성                    | 설명 |
|-----------------------|------|
| **Atomicity (원자성)**   | 트랜잭션의 모든 작업이 성공하거나, 실패 시 전부 롤백되어야 함 |
| **Consistency (일관성)** | 트랜잭션이 실행되기 전과 후의 데이터가 일관성을 유지해야 함 |
| **Isolation (독립성)**   | 동시에 실행되는 트랜잭션이 서로에게 영향을 미치지 않도록 독립적으로 실행되어야 함 |
| **Durability (지속성)**  | 트랜잭션이 성공적으로 완료되면 결과가 영구적으로 반영되어야 함 |

---

## 3. 트랜잭션의 동작 과정
트랜잭션의 기본적인 동작 과정은 다음과 같습니다.

1. **BEGIN TRANSACTION** → 트랜잭션 시작
2. **여러 개의 SQL 실행** (INSERT, UPDATE, DELETE 등)
3. **COMMIT 또는 ROLLBACK**
    - `COMMIT` : 모든 작업을 데이터베이스에 반영 (트랜잭션 완료)
    - `ROLLBACK` : 오류 발생 시 모든 작업을 취소 (이전 상태로 복구)

---

## 4. 트랜잭션 격리 수준(Isolation Level)
트랜잭션은 동시에 여러 개가 실행될 수 있으며, 이때 **격리 수준(Isolation Level)**을 설정하여 트랜잭션 간의 간섭을 조절할 수 있습니다.

격리 수준이 낮을수록 성능은 좋아지지만, 데이터 정합성 문제가 발생할 위험이 커집니다.

| 격리 수준 | 설명 | 발생할 수 있는 문제 |
|-----------|------|------------------|
| **READ UNCOMMITTED** | 다른 트랜잭션의 변경 사항을 COMMIT 전에 읽을 수 있음 | 더티 리드(Dirty Read) |
| **READ COMMITTED** | COMMIT된 데이터만 읽을 수 있음 (일반적인 기본 설정) | 반복 가능하지 않은 읽기(Non-repeatable Read) |
| **REPEATABLE READ** | 트랜잭션이 시작되면 같은 데이터를 여러 번 조회해도 같은 결과를 보장 | 팬텀 리드(Phantom Read) |
| **SERIALIZABLE** | 트랜잭션을 직렬화하여 실행 (가장 높은 격리 수준) | 성능 저하 |

✅ **MySQL의 기본 격리 수준**: `REPEATABLE READ`  
✅ **PostgreSQL, Oracle, SQL Server의 기본 격리 수준**: `READ COMMITTED`

---

## 5. 트랜잭션 관리 전략
트랜잭션을 관리하는 방법에는 크게 두 가지가 있습니다.

### 1️⃣ 프로그램 기반 트랜잭션 관리
코드에서 직접 `commit()`과 `rollback()`을 수행하는 방식입니다.

```
Connection conn = null;

try {
    conn = dataSource.getConnection();
    conn.setAutoCommit(false); // 자동 커밋 해제

    // SQL 실행
    updateAccountA(conn);
    updateAccountB(conn);

    conn.commit(); // 트랜잭션 완료
} catch (Exception e) {
    if (conn != null) {
        conn.rollback(); // 오류 발생 시 롤백
    }
} finally {
    if (conn != null) {
        conn.close();
    }
}
```

### ✅ 트랜잭션은 데이터베이스의 무결성을 유지하는 중요한 개념이며, 실무에서 데이터 변경이 이루어질 때 항상 고려해야 한다.