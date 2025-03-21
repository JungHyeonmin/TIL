# *1. 키(Key)의 종류와 역할**
데이터베이스(DB)에서 **키(Key)**는 데이터를 식별하고 무결성을 유지하는 데 중요한 역할을 합니다. 키는 테이블에서 특정 레코드를 찾거나 관계를 정의하는 데 사용됩니다.

## **(1) 기본 키(Primary Key, PK)**
### **✔ 정의**
- 테이블에서 각 레코드를 **유일하게 식별하는 키**
- **NULL 값을 허용하지 않으며**, 중복될 수 없음
- 보통 `id`와 같은 고유한 값을 사용

### **✔ 사용법**
- `PRIMARY KEY`를 설정하면 자동으로 **인덱스(index)**가 생성됨
- 일반적으로 **자동 증가(AUTO_INCREMENT)**를 사용하여 자동으로 값이 증가하도록 설정

### **✔ 예제**
```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY AUTO_INCREMENT,  -- 기본 키, 자동 증가
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);
```
> **💡 왜 기본 키가 필요할까?**
> - 데이터를 **빠르게 찾고 정렬**하기 위함
> - 다른 테이블에서 **참조(Foreign Key) 가능**

---

## **(2) 후보 키(Candidate Key)**
### **✔ 정의**
- 기본 키가 될 수 있는 **잠재적인 키 후보들**
- **유일성(Unique)과 최소성(Minimality)**을 만족해야 함
- 기본 키로 선택되지 않은 키는 **대체 키(Alternate Key)**가 됨

### **✔ 예제**
```sql
CREATE TABLE employees (
    emp_id INT NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,  -- 후보 키 (기본 키로 사용 가능)
    phone_number VARCHAR(20) NOT NULL UNIQUE,  -- 후보 키
    PRIMARY KEY (emp_id)  -- 기본 키로 emp_id 선택
);
```
> **💡 후보 키는 왜 필요할까?**
> - 여러 개의 유니크한 컬럼 중 **기본 키를 선택하기 위해**
> - 기본 키 변경이 필요할 때 대체 가능

---

## **(3) 대체 키(Alternate Key)**
### **✔ 정의**
- 후보 키 중에서 기본 키로 **선택되지 않은 키**
- `UNIQUE` 속성을 가지며, 기본 키처럼 사용 가능

### **✔ 예제**
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,  -- 기본 키
    national_id VARCHAR(20) UNIQUE  -- 대체 키 (기본 키 후보였으나 선택되지 않음)
);
```
> **💡 대체 키는 언제 사용될까?**
> - 기본 키를 변경할 필요가 있을 때 **대체 키를 새로운 기본 키로 지정 가능**

---

## **(4) 외래 키(Foreign Key, FK)**
### **✔ 정의**
- **다른 테이블의 기본 키를 참조**하는 키
- 테이블 간의 관계를 맺을 때 사용됨
- 외래 키를 설정하면 **참조 무결성(Referential Integrity)**이 유지됨

### **✔ 사용법**
- 외래 키를 설정하면, **해당 값이 참조하는 테이블에 존재해야 함**
- 삭제 또는 수정 시 **제약조건(ON DELETE, ON UPDATE)**을 걸 수 있음

### **✔ 예제**
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(user_id)  -- users 테이블의 기본 키 참조
);
```
> **💡 외래 키의 역할**
> - **데이터 무결성을 보장** (잘못된 참조 방지)
> - **JOIN을 통해 관계형 데이터 연계 가능**

---

## **(5) 슈퍼 키(Super Key)**
### **✔ 정의**
- **유일성을 가지지만 최소성을 만족하지 않는 키**
- 기본 키, 후보 키를 포함하는 더 큰 키

### **✔ 예제**
```sql
CREATE TABLE books (
    book_id INT NOT NULL,
    isbn VARCHAR(20) NOT NULL UNIQUE,  
    title VARCHAR(100) NOT NULL,
    author VARCHAR(50) NOT NULL,
    PRIMARY KEY (book_id)  
);
```
- `(book_id, isbn)` → **슈퍼 키** (유일성 만족, 최소성 X)
- `book_id` 또는 `isbn` → **후보 키** (유일성 + 최소성 만족)

> **💡 슈퍼 키는 왜 중요한가?**
> - 모든 키의 **상위 개념**
> - **정규화를 진행하면서 최소성을 만족하는 후보 키를 추출**

---

## **(6) 복합 키(Composite Key)**
### **✔ 정의**
- **두 개 이상의 컬럼을 조합하여 기본 키로 사용**
- 개별 컬럼으로는 유일성을 만족하지 않지만, **조합하면 유일성을 만족**

### **✔ 예제**
```sql
CREATE TABLE enrollments (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id)  -- 복합 키
);
```
> **💡 복합 키를 언제 사용할까?**
> - **다대다(M:N) 관계를 관리할 때** 사용
> - 개별 컬럼만으로는 유니크하지 않은 경우

---

## **(7) 인덱스 키(Index Key)**
### **✔ 정의**
- 검색 성능을 높이기 위해 사용되는 키
- `PRIMARY KEY`와 `UNIQUE`는 기본적으로 **인덱스가 자동 생성됨**
- 추가적으로 `INDEX`를 생성하여 특정 컬럼 검색을 최적화할 수 있음

### **✔ 예제**
```sql
CREATE INDEX idx_email ON users(email);
```
> **💡 인덱스를 왜 사용할까?**
> - WHERE 조건으로 자주 조회하는 컬럼의 **검색 속도를 향상**
> - 정렬 및 그룹핑 성능 최적화

---

# **2. 정리 및 비교**
| 키 종류 | 유일성(Unique) | NULL 허용 | 역할 |
|---------|--------------|-----------|------|
| **기본 키 (PK)** | ✅ | ❌ | 테이블의 대표 키 |
| **후보 키 (Candidate Key)** | ✅ | ❌ | 기본 키로 선택될 수 있는 후보 |
| **대체 키 (Alternate Key)** | ✅ | ❌ | 기본 키가 되지 못한 후보 키 |
| **외래 키 (FK)** | ❌ (참조 대상 PK가 유일) | ✅ | 다른 테이블과 관계를 형성 |
| **슈퍼 키 (Super Key)** | ✅ | ✅ | 최소성을 만족하지 않는 모든 키 |
| **복합 키 (Composite Key)** | ✅ | ❌ | 여러 컬럼을 조합한 키 |
| **인덱스 키 (Index Key)** | ❌ | ✅ | 검색 최적화 |

---

# **3. 마무리**
- **기본 키**는 테이블에서 각 행을 **고유하게 식별**하는 역할을 한다.
- **후보 키**는 기본 키가 될 수 있는 키들의 후보이다.
- **외래 키**는 **다른 테이블의 기본 키를 참조**하여 관계를 맺는다.
- **복합 키**는 두 개 이상의 컬럼을 결합하여 하나의 키로 사용한다.
- **인덱스 키**는 검색 성능을 높이기 위해 사용한다.