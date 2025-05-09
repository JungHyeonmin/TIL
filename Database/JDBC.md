# JDBC란?

JDBC는 자바 프로그램과 데이터베이스 간의 통신을 가능하게 해주는 API다. 이를 통해 자바 애플리케이션은 데이터베이스에 연결하고, 데이터를 쿼리하거나 업데이트할 수 있다. JDBC는 SQL을 사용하여 데이터를 주고받으며, 데이터베이스 시스템의 구체적인 구현과는 독립적이다. 즉, MySQL, Oracle, PostgreSQL 등 다양한 데이터베이스와 호환됩니다.

### SQL이란?

관계형 데이터베이스 관리 시스템(RDBMS)에서 데이터를 관리하고 조작하는 데 사용되는 표준 언어다. SQL은 데이터르 삽입, 수정, 삭제, 조회하는 등 다양한 작업을 수행할 수 있도록 설계되어있다.

- 주요 명령어
    - `INSERT`: 새로운 데이터를 테이블에 삽입할 때 사용됩니다.
    - `UPDATE`: 기존 데이터를 수정할 때 사용됩니다.
    - `DELETE`: 데이터를 삭제할 때 사용됩니다.
    - `SELECT`: 데이터를 조회할 때 사용됩니다.
    - `GRANT`: 사용자에게 특정 권한을 부여할 때 사용됩니다.
    - `REVOKE`: 사용자로부터 권한을 회수할 때 사용됩니다.
    - `COMMIT`: 트랜잭션을 영구적으로 저장할 때 사용됩니다.
    - `ROLLBACK`: 트랜잭션을 취소하고, 데이터베이스 상태를 이전 상태로 되돌릴 때 사용됩니다.

# 왜 사용하는 걸까?

과거에는 각 데이터베이스마다 접근 방식이 달라서, 특정 데이터베이스에 맞는 코드를 작성해야 했다. **이는 유지보수와 확장에 큰 부담이었다.** JDBC는 이러한 문제를 해결하기 위해 등장했으며, 데이터베이스 접근을 표준화하여 다양한 데이터베이스에 대해 동일한 방식으로 작업할 수 있도록 돕는다. JDBC는 자바 표준 라이브러리의 일부로 포함되며, JAVA 애플리케이션의 데이터베이스 연동에 중요한 역할을 한다.

# JDBC의 발전 과정

JDBC는 자바 초기부터 중요한 역할을 했으며, 지속적으로 개선되어 왔다. 초기 버전에서는 기본적인 데이터베이스 연결과 SQL실행만 지원했지만, 점차 발전하면서 더 나은 성능, 트랜젝션 관리, 배치 처리, Connection Pooling 등을 지원하게 되었다.

### 트랜젝션 관리

데이터베이스에서 일련의 작업을 하나의 단위로 묶어서 실행하는 개념. 이 작업은 모두 성공하거나, 모두 실패하는 원자젹(Atomic) 성질을 갖는다.

→ 즉, 트랜잭션 내의 모든 작업이 성공하면 변경 사항이 커밋되고, 하나라도 실패하면 모든 변경사항이 롤백된다.

- **`COMMIT`**: 트랜잭션을 완료하고, 데이터베이스에 변경 사항을 영구적으로 반영합니다.
- **`ROLLBACK`**: 트랜잭션을 취소하고, 변경 사항을 이전 상태로 되돌립니다.

### 배치 처리

대량의 데이터를 한꺼번에 처리하는 방법. 이를 통해 자주 반복되는 작업이나 대규모 데이터를 더 효율적으로 관리할 수 있다.

### Connection Pooling

데이터베이스와의 연결을 효율적으로 관리하기 위한 기술.

일반적으로 데이터베이스 연결은 비용이 많이 드는 작업이기 때문에, 자주 연결하고 끊는 것은 시스템 성능에 부담으 준다. 이를 해결하기 위해 미리 **여러개의 연결(Connection)을 생성해 놓고, 필요할 때마다 이를 재사용**하는 것이 Connection Pooling이다.

# JDBC와 DB, Spring의 관계

JDBC는 강력하지만, 그 자체로 사용하기엔 상당히 복잡하고 많은 코드를 작성해야 한다. Spring 프레임워크는 이런 불편함을 줄이기 우해 Spring JDBC를 제공한다. Spring JDBC는 데이터베이스와의 상호작용을 단순화 하고, 예외처리, 리소스 관리, 트랜잭션 관리를 자동화 하여 개발자가 좀 더 직관적으로 작업할 수 있도록 해준다.

# 주요 메서드

### `Connection` **:** SQL 쿼리를 실행할 때 사용되는 객체

- DriverManager.getConnection() : 데이터베이스와의 연결을 설정한다.

### `Statement` : SQL 쿼리를 실행할 때 사용되는 객체

- executeQuery() : SELECT 쿼리를 실행하고, ResultSet객체를 반환한다.
- executeUpdate() : INSERT, UPDATE, DELETE 같은 쿼리를 실행하며, 영향을 받은 행의 개수를 반환한다.

### `PrePareStatment` : 미리 컴파일된 SQL 쿼리를 실행할 때 사용되는 객체

- SQL 인젝션 공격을 방지하는 데 유리하다.
    - SQL 인젝션이란?
- setString(), setInt() : 쿼리의 파라미터 값을 설정한다.
- executeQuery() : 쿼리를 실행하고 결과를 반환한다.

### `ResultSet` : 쿼리 결과를 저장하는 객체

- next() : 다음 행으로 이동하면서 데이터가 있으면 true를 반환한다.
- getString(), getInt() : 특정 열의 데이터를 반환한다.

# JDBC Template

JDBC Template는 Spring 프레임워크에서 제공하는 도구로, 자바 애플리케이션이 데이터베이스와 상호작용할 때 필요한 반복적인 JDBC 코드를 단순화하고, 더 쉽게 사용할 수 있게 만들어 준다. JDBC Template은 기존 JDBC API를 추상화하여, 데이터베이스 작업을 보다 효율적으로 처리하고 갭라자가 작성할 코드를 줄여준다.

## JDBC Template의 주요 목적

### 1. 보일러플레이트 코드 제거

JDBC API를 직접 사용할 경우, Connection, Statement, ResultSet같은 리소스를 열고 닫는 코드를 반드시 작성해야 한다. 이러한 반복적인 작업을 JDBC Template이 자동으로 처리한다.

### 2. 예외 처리 통일

JDBC 에외는 여러가지 복잡할 수 있지만, Spring의 DataAccessException으로 통일된 예외 처리를 제공한다.

### 3. 트랜잭션 및 리소스 관리 간소화

Connection이나 ResultSet을 열고 닫는 일을 개발자가 직접 처리할 필요 없이, 자동으로 리소스를 관리해준다.