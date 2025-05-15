## ✅ `HandlerExceptionResolver`

### 🔍 개념

* 컨트롤러나 서비스에서 예외가 발생했을 때, 해당 예외를 처리할 수 있는 **에러 처리 전략**을 결정해주는 인터페이스야.

### 📌 사용 이유

* try-catch 없이도 예외를 전역에서 처리할 수 있음.
* 사용자에게 적절한 에러 페이지 또는 JSON 응답을 제공 가능.

### 🧠 어떻게 동작?

```text
[Controller에서 예외 발생] → DispatcherServlet → HandlerExceptionResolver → 적절한 에러 처리
```

### ✨ 대표 구현체

* `SimpleMappingExceptionResolver`
* `ExceptionHandlerExceptionResolver` (→ `@ExceptionHandler` 사용 가능하게 해줌)

---

## ✅ Spring MVC 전체 요청 흐름 요약

```text
1. 클라이언트 요청
2. DispatcherServlet 수신
3. HandlerMapping → 어떤 Controller 처리할지 결정
4. Controller 로직 수행
5. View 이름 반환
6. ViewResolver → 논리 이름을 실제 뷰로 변환
7. DispatcherServlet이 View 렌더링
8. 사용자에게 응답 반환
```

---

## 🔁 이 기술이 없었을 때는?

* 서블릿에서 `if/else` 문으로 URL 분기
* 뷰 이름은 직접 경로 작성 (`request.getRequestDispatcher("jsp/home.jsp")`)
* 예외는 매번 try-catch로 처리 (코드 중복, 유지보수 어려움)

---

## ✨ 마무리 요약

| 컴포넌트                       | 역할                  | 없었을 때            |
| -------------------------- | ------------------- | ---------------- |
| `HandlerMapping`           | URL → Controller 매핑 | `if/else` URL 분기 |
| `ViewResolver`             | 뷰 이름 → 실제 경로 변환     | 직접 경로 지정         |
| `HandlerExceptionResolver` | 예외 처리 전략 적용         | 직접 try-catch     |