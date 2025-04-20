## 🔍 **1. 페이징(Paging)이란?**

**페이징이란?**
- 데이터를 한 번에 모두 조회하지 않고, **일정한 단위로 나누어 조회**하는 기술.
- 예: 게시판에서 100개의 글을 10개씩 10페이지로 나눠서 보여주는 것.

---

## 📦 **2. 왜 필요한가?**

| 이유 | 설명 |
|------|------|
| ✅ 성능 향상 | 대용량 데이터를 한 번에 다 가져오면 **DB와 네트워크 부하**가 크기 때문. |
| ✅ UX 개선 | 사용자에게 한번에 너무 많은 정보를 주면 **가독성이 떨어짐**. |
| ✅ 메모리 절약 | 서버/클라이언트 양쪽에서 **메모리 효율**이 좋아짐. |

---

## ⚙️ **3. 어떻게 사용하는가? (기본 개념)**

- 클라이언트가 다음과 같은 파라미터를 서버에 넘김:
    - `page`: 현재 페이지 번호 (1부터 시작)
    - `size` 또는 `limit`: 한 페이지당 보여줄 데이터 수

예시 요청:
```
GET /posts?page=2&size=10
```

---

## 🧮 **4. SQL 페이징 예시 (MySQL 기준)**

```sql
-- 2페이지에서 10개씩 보여주기
SELECT * FROM posts
ORDER BY created_at DESC
LIMIT 10 OFFSET 10; -- OFFSET = (page - 1) * size
```

### ✍️ 설명
```sql
LIMIT 10         -- 10개 가져오기
OFFSET 10        -- 앞에 10개는 건너뛰기
```

---

## 💡 **5. Spring Data JPA에서 페이징 처리**

```java
// Repository
Page<Post> findAll(Pageable pageable);

// 사용 예시
Pageable pageable = PageRequest.of(1, 10); // 2페이지, 10개
Page<Post> result = postRepository.findAll(pageable);

// 응답 데이터
result.getContent();     // 실제 데이터
result.getTotalPages();  // 전체 페이지 수
result.getTotalElements(); // 전체 데이터 개수
result.getNumber();      // 현재 페이지
```

---

## 🛠️ **6. 기술 발전 흐름**

| 과거 방식 | 현재 |
|-----------|------|
| 모든 데이터를 불러와서 클라이언트에서 자르기 | 서버에서 `LIMIT`, `OFFSET` 또는 **커서 기반 페이징(Cursor Paging)** 사용 |

---

## 🆚 **7. Offset Paging vs Cursor Paging**

| 방식 | 설명 | 장점 | 단점 |
|------|------|------|------|
| Offset Paging | `LIMIT`, `OFFSET` 사용 | 구현 쉬움 | 페이지가 커질수록 느려짐 |
| Cursor Paging | 특정 ID나 정렬 기준 값으로 다음 데이터 가져옴 | 성능 좋음 | 구현 복잡함 |

예:
```sql
-- Cursor 기반: id > 마지막으로 본 id
SELECT * FROM posts WHERE id > 123 ORDER BY id ASC LIMIT 10;
```

---

## 📌 **8. 페이징이 없었을 때는?**

- 클라이언트는 서버에 전체 데이터를 요청해서 **자바스크립트로 slicing 처리**하거나,
- **서버가 전체 데이터를 한 번에 JSON으로 내려줌** → 큰 응답, 느린 성능, 비효율

---

## ✅ 정리

- 페이징은 성능과 사용자 경험을 위한 필수 기술
- 기본은 `LIMIT`, `OFFSET`이며
- 대용량 환경에선 **커서 기반 페이징(Cursor paging)** 추천
- Spring Data JPA는 `Pageable` 객체로 간단하게 처리 가능