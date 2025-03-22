## **1. SQL 데이터 타입**

### **(1) 문자형(Character)**
- `CHAR(n)`: 고정 길이 문자 타입 (최대 255자)
- `VARCHAR(n)`: 가변 길이 문자 타입 (최대 65535자, DBMS마다 다름)
- `TEXT`: 매우 긴 문자열 저장 (가변 길이, 인덱스 제한 있음)
- `NCHAR(n)`, `NVARCHAR(n)`: 유니코드(UCS-2/UTF-16) 문자 타입  
  *(한글, 일본어 등 멀티바이트 문자 저장 시 사용)*
- `CLOB (Character Large Object)`: 대용량 텍스트 데이터 저장

### **(2) 숫자형(Numeric)**
- 정수형
    - `TINYINT`: 1바이트(8비트), -128~127 또는 0~255
    - `SMALLINT`: 2바이트(16비트), -32,768~32,767
    - `MEDIUMINT`: 3바이트(24비트), -8,388,608~8,388,607
    - `INT`(또는 `INTEGER`): 4바이트(32비트), -2,147,483,648~2,147,483,647
    - `BIGINT`: 8바이트(64비트), 매우 큰 숫자 저장 가능

- 실수형 (부동소수점)
    - `FLOAT(p)`: 4바이트(정밀도 제한 있음)
    - `DOUBLE`(또는 `REAL`): 8바이트(더 높은 정밀도)
    - `DECIMAL(p, s)`(또는 `NUMERIC(p, s)`): 고정 소수점 (정밀한 숫자 연산 가능)

### **(3) 날짜/시간형(Date/Time)**
- `DATE`: `YYYY-MM-DD` (예: `2025-03-22`)
- `TIME`: `HH:MI:SS` (예: `14:30:59`)
- `DATETIME`: `YYYY-MM-DD HH:MI:SS`
- `TIMESTAMP`: `YYYY-MM-DD HH:MI:SS` (UTC 기준)
- `YEAR`: `YYYY` (4자리 연도만 저장)

### **(4) 이진형(Binary)**
- `BLOB (Binary Large Object)`: 이미지, 비디오 등 바이너리 데이터 저장
- `BINARY(n)`: 고정 길이 바이너리 데이터
- `VARBINARY(n)`: 가변 길이 바이너리 데이터

### **(5) 특수 타입**
- `BOOLEAN`: `TRUE`(1) 또는 `FALSE`(0) (DBMS에 따라 `TINYINT(1)`으로 처리)
- `ENUM`: 미리 정의된 문자열 값 중 하나 선택 (예: `'small', 'medium', 'large'`)
- `SET`: 여러 개의 미리 정의된 값 선택 가능

---

## **2. `CHAR` vs `VARCHAR` 비교**
`CHAR`와 `VARCHAR`는 둘 다 문자열을 저장하지만 **공간 할당 방식과 성능에서 차이**가 있어.

| 속성 | CHAR(n) | VARCHAR(n) |
|------|--------|------------|
| 길이 | 고정 길이 | 가변 길이 |
| 저장 방식 | n바이트 고정 할당 | 데이터 길이에 따라 달라짐 (최대 n바이트) |
| 저장 공간 효율 | 공간 낭비 발생 가능 | 더 효율적 |
| 조회 속도 | 빠름 (고정 크기) | 상대적으로 느림 |
| 패딩 처리 | 저장할 때 부족한 공간을 공백(`' '`)으로 채움 | 자동으로 공백 제거 |

### **예제**
```sql
CREATE TABLE test_table (
    fixed_char CHAR(10),
    variable_char VARCHAR(10)
);

INSERT INTO test_table (fixed_char, variable_char) VALUES ('abc', 'abc');
```
- `fixed_char`는 10바이트 공간을 차지하고 `'abc       '`처럼 공백이 추가됨.
- `variable_char`는 `'abc'` 그대로 저장되고, 공간 낭비가 없음.

### **정렬 및 비교 시 차이**
`CHAR` 타입은 **공백이 채워진 상태로 저장되지만 비교 시 자동으로 제거됨**.  
반면 `VARCHAR`는 **입력된 값 그대로 저장됨**.

```sql
SELECT fixed_char = 'abc' FROM test_table;  -- TRUE (공백 제거 후 비교)
SELECT variable_char = 'abc' FROM test_table; -- TRUE
```
그러나, BINARY 비교 시 `CHAR`의 공백이 유지됨:
```sql
SELECT fixed_char = BINARY 'abc      ';  -- FALSE (공백까지 포함해 비교)
SELECT variable_char = BINARY 'abc'; -- TRUE
```

### **언제 `CHAR`를 쓰고 언제 `VARCHAR`를 쓸까?**
- **`CHAR` 사용 추천**
    - ID, 코드, 고정된 길이의 데이터를 저장할 때 (예: 국가 코드 `KR`, 성별 `M/F`)
    - 빠른 조회가 필요한 경우 (인덱스 최적화 효과)
- **`VARCHAR` 사용 추천**
    - 가변 길이 문자열 (예: 이름, 이메일, 주소 등)
    - 긴 문자열 저장 시 공간 효율성을 고려해야 할 때

---

## **3. 다른 문자열 타입과 비교**
| 타입 | 최대 길이 | 특징 |
|------|--------|------|
| `CHAR(n)` | 255 | 고정 길이, 공백 패딩 |
| `VARCHAR(n)` | 65535 | 가변 길이, 공백 없이 저장 |
| `TEXT` | 65535 | 가변 길이, 인덱스 지원 제한 |
| `CLOB` | 매우 큼 | 대량 문자 데이터 저장 |

`TEXT`와 `CLOB`은 `VARCHAR`와 비슷하지만 **인덱싱이 제한적**이므로 검색 속도가 느릴 수 있어.

---

## **결론**
- **짧고 고정된 문자열** → `CHAR(n)`
- **길이가 유동적인 문자열** → `VARCHAR(n)`
- **긴 텍스트(인덱스 필요 없음)** → `TEXT`
- **대량의 문자열 데이터** → `CLOB`