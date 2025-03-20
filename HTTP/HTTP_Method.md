HTTP 메서드는 클라이언트가 서버에 요청을 보낼 때 사용하는 방식으로, 주로 웹 애플리케이션에서 데이터를 가져오거나 수정하는 데 사용됩니다. 각각의 HTTP 메서드는 특정한 역할과 의미를 가지며, 주요 메서드는 다음과 같습니다.

---

## 1. **GET**
- **설명**: 서버에서 데이터를 조회할 때 사용
- **특징**:
    - 요청에 바디(Body)가 없음
    - URL에 쿼리 스트링(Query String)으로 데이터를 포함 가능
    - 캐싱 가능
    - 멱등성(Idempotent) 보장 (같은 요청을 여러 번 보내도 결과가 동일)
- **사용 예시**:
  ```http
  GET /users?id=1 HTTP/1.1
  Host: example.com
  ```
  ```java
  // Spring Boot 컨트롤러에서 GET 요청 처리
  @GetMapping("/users/{id}")
  public ResponseEntity<User> getUser(@PathVariable Long id) {
      User user = userService.findById(id);
      return ResponseEntity.ok(user);
  }
  ```

---

## 2. **POST**
- **설명**: 서버에 새로운 리소스를 생성할 때 사용
- **특징**:
    - 요청 바디에 데이터를 포함
    - 멱등성이 없음 (같은 요청을 여러 번 보내면 리소스가 중복 생성될 수 있음)
- **사용 예시**:
  ```http
  POST /users HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
      "name": "John",
      "email": "john@example.com"
  }
  ```
  ```java
  // Spring Boot 컨트롤러에서 POST 요청 처리
  @PostMapping("/users")
  public ResponseEntity<User> createUser(@RequestBody User user) {
      User savedUser = userService.save(user);
      return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
  }
  ```

---

## 3. **PUT**
- **설명**: 서버의 기존 리소스를 수정할 때 사용 (전체 업데이트)
- **특징**:
    - 요청 바디에 데이터를 포함
    - 멱등성 보장 (같은 요청을 여러 번 보내도 결과가 동일)
    - 리소스가 존재하지 않으면 새로 생성할 수도 있음
- **사용 예시**:
  ```http
  PUT /users/1 HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
      "name": "John Updated",
      "email": "john_updated@example.com"
  }
  ```
  ```java
  // Spring Boot 컨트롤러에서 PUT 요청 처리
  @PutMapping("/users/{id}")
  public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
      User updatedUser = userService.update(id, user);
      return ResponseEntity.ok(updatedUser);
  }
  ```

---

## 4. **PATCH**
- **설명**: 서버의 기존 리소스를 부분적으로 수정할 때 사용
- **특징**:
    - 요청 바디에 일부 데이터만 포함
    - 멱등성 보장 안 됨 (특정 구현에 따라 다를 수 있음)
- **사용 예시**:
  ```http
  PATCH /users/1 HTTP/1.1
  Host: example.com
  Content-Type: application/json

  {
      "email": "new_email@example.com"
  }
  ```
  ```java
  // Spring Boot 컨트롤러에서 PATCH 요청 처리
  @PatchMapping("/users/{id}")
  public ResponseEntity<User> updateUserPartial(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
      User updatedUser = userService.partialUpdate(id, updates);
      return ResponseEntity.ok(updatedUser);
  }
  ```

---

## 5. **DELETE**
- **설명**: 서버에서 리소스를 삭제할 때 사용
- **특징**:
    - 요청 바디 없이 사용 가능
    - 멱등성 보장 (같은 요청을 여러 번 보내도 결과가 동일)
- **사용 예시**:
  ```http
  DELETE /users/1 HTTP/1.1
  Host: example.com
  ```
  ```java
  // Spring Boot 컨트롤러에서 DELETE 요청 처리
  @DeleteMapping("/users/{id}")
  public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
      userService.delete(id);
      return ResponseEntity.noContent().build();
  }
  ```

---

## 6. **HEAD**
- **설명**: `GET` 요청과 동일하지만, 응답 바디 없이 헤더만 반환
- **특징**:
    - 리소스의 존재 여부를 확인할 때 사용
    - 캐싱 관련 메타데이터 확인 가능
- **사용 예시**:
  ```http
  HEAD /users/1 HTTP/1.1
  Host: example.com
  ```

---

## 7. **OPTIONS**
- **설명**: 특정 리소스가 지원하는 HTTP 메서드를 확인할 때 사용
- **사용 예시**:
  ```http
  OPTIONS /users HTTP/1.1
  Host: example.com
  ```
    - 응답 예시:
      ```http
      HTTP/1.1 200 OK
      Allow: GET, POST, PUT, DELETE, OPTIONS
      ```

---

## 8. **TRACE**
- **설명**: 네트워크를 통해 요청이 서버까지 도달하는 경로를 확인하는 데 사용
- **특징**:
    - 보안상의 이유로 거의 사용되지 않음
- **사용 예시**:
  ```http
  TRACE /users HTTP/1.1
  Host: example.com
  ```

---

## 9. **CONNECT**
- **설명**: 프록시 서버를 통해 터널을 생성할 때 사용 (예: HTTPS 요청)
- **특징**:
    - 일반적인 API 요청에서는 사용되지 않음
- **사용 예시**:
  ```http
  CONNECT example.com:443 HTTP/1.1
  ```

---

### 🚀 **HTTP 메서드 요약**
| 메서드   | 설명                          | 멱등성 | 캐싱 가능 |
|---------|-----------------------------|------|------|
| GET     | 리소스 조회                    | ✅   | ✅   |
| POST    | 리소스 생성                    | ❌   | ❌   |
| PUT     | 리소스 전체 수정                | ✅   | ❌   |
| PATCH   | 리소스 일부 수정                | ❌   | ❌   |
| DELETE  | 리소스 삭제                    | ✅   | ❌   |
| HEAD    | `GET`과 같지만 응답 바디 없음 | ✅   | ✅   |
| OPTIONS | 사용 가능한 메서드 확인         | ✅   | ❌   |
| TRACE   | 네트워크 요청 경로 확인         | ✅   | ❌   |
| CONNECT | 프록시 터널 생성                | ✅   | ❌   |

---

### 🔥 **정리**
- `GET` → 조회
- `POST` → 생성
- `PUT` → 전체 수정
- `PATCH` → 부분 수정
- `DELETE` → 삭제
- `HEAD` → 응답 헤더만 가져오기
- `OPTIONS` → 가능한 메서드 확인