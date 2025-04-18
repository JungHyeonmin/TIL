## ✅ Common Code란?

### 📌 정의
**공통 코드(Common Code)**란,  
**여러 모듈 또는 여러 기능에서 반복적으로 사용되는 로직, 유틸리티, 설정, 상수 등을 모아놓은 코드**야.

> 🔁 중복 방지, 유지보수 편의성, 코드 일관성 확보가 목적이야.

---

## ✅ Common Code의 예시

| 유형 | 예시 | 설명 |
|------|------|------|
| 유틸리티 | `DateUtils`, `FileUtils`, `JsonUtils` | 날짜 포맷팅, 파일 처리, JSON 변환 등 |
| 상수 | `Constants`, `ErrorCodes`, `Messages` | 코드 전체에서 사용하는 공통 상수 관리 |
| 공통 예외처리 | `GlobalExceptionHandler` | 모든 컨트롤러에서 발생하는 예외를 하나로 처리 |
| 응답 포맷 | `ApiResponse<T>` | 모든 API의 응답을 공통된 구조로 제공 |
| 인터셉터, 필터 | `AuthInterceptor`, `LoggingFilter` | 인증, 로깅 등 공통 로직 |
| DTO 변환 | `Mapper`, `Converter` | Entity ↔ DTO 변환 시 재사용 가능 |

---

## ✅ Common Code를 어디에 두는가?

폴더 구조 설계에서 **공통 코드들을 모아놓은 디렉토리**를 만드는 것이 핵심이야.

---

## ✅ 설계에 따른 폴더 구조 예시

### 🌟 1. 단일 모듈 (Spring Boot 기본 프로젝트)

```bash
src/main/java/com/example/project/
├── common/                 ← 공통 코드
│   ├── dto/
│   ├── exception/
│   ├── utils/
│   ├── constants/
│   └── response/
├── config/                 ← 설정 (Spring 설정 클래스)
├── domain/                ← 비즈니스 도메인
│   ├── user/
│   ├── product/
│   └── order/
├── controller/
├── service/
└── repository/
```

- `common/`에는 어떤 도메인에도 직접 종속되지 않는, **범용적으로 쓸 수 있는 코드**만 둬.
- 도메인 별 비즈니스 로직은 `domain/`에 집중시켜.

---

### 🌟 2. 멀티 모듈 구조

```bash
project-root/
├── common-module/          ← 공통 모듈 (다른 모듈들이 import함)
│   ├── utils/
│   ├── exception/
│   └── dto/
├── user-service/
│   └── com.example.user/
├── product-service/
│   └── com.example.product/
└── order-service/
    └── com.example.order/
```

- 이 구조에서는 `common-module`을 다른 서비스 모듈들이 `dependency`로 사용해.
- MSA나 멀티 프로젝트에서는 이 방식이 많이 쓰여.

---

## ✅ 폴더 구조 설계의 핵심 원칙

| 원칙 | 설명 |
|------|------|
| 💡 관심사 분리 (Separation of Concerns) | 비즈니스 로직, 예외처리, 응답 포맷 등 서로 다른 역할을 나눠 구성 |
| 💡 재사용 가능성 고려 | 유틸 클래스, 상수, 포맷팅 로직은 중복 없이 한곳에 |
| 💡 의존 방향은 항상 단방향 | 공통 모듈은 도메인 모듈을 **참조하지 않음** (순환 의존 금지) |
| 💡 계층 분리 | Controller ↔ Service ↔ Repository 계층 구분 철저히 |
| 💡 이름은 명확하게 | `GlobalExceptionHandler`, `ApiResponse`, `DateUtils`처럼 **의도가 명확한 네이밍** 사용 |

---

## ✅ 실무 예시: 공통 응답 포맷

### 📁 `common/response/ApiResponse.java`

```java
public class ApiResponse<T> {
    private int code;
    private String message;
    private T data;

    public static <T> ApiResponse<T> success(T data) {
        return new ApiResponse<>(200, "Success", data);
    }

    public static <T> ApiResponse<T> error(String message) {
        return new ApiResponse<>(500, message, null);
    }
}
```

> 모든 컨트롤러에서 이 포맷으로 응답 → 프론트가 일관되게 처리 가능

---

## ✅ 실무 예시: 공통 상수

### 📁 `common/constants/ErrorCode.java`

```java
public class ErrorCode {
    public static final String INVALID_INPUT = "ERR_001";
    public static final String NOT_FOUND = "ERR_002";
}
```

---

## 🔚 마무리 요약

| 질문 | 설명 |
|------|------|
| Common Code란? | 프로젝트 전반에서 반복적으로 사용되는 코드 묶음 |
| 왜 나누나? | 중복 제거, 재사용, 유지보수 편의성 |
| 어디에 두나? | 단일 모듈이면 `common/`, 멀티 모듈이면 별도 공통 모듈 |
| 어떻게 나누나? | 유틸, 상수, 예외처리, 응답 포맷 등 역할별로 구조화 |
| 좋은 구조 예시 | `common/`, `config/`, `domain/도메인별/`, `controller/`, `service/` 등 관심사 기반 분리 |
