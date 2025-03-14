## 1. **API (Application Programming Interface)란?**
API는 애플리케이션이 서로 상호작용할 수 있도록 돕는 **인터페이스**입니다.  
소프트웨어 간의 데이터 교환을 가능하게 하며, 주로 클라이언트-서버 간 통신을 위해 사용됩니다.

### **API의 역할**
- 프로그램 간 **데이터 및 기능 공유**
- 특정 기능을 **추상화하여 제공** (예: 결제 API, 지도 API)
- **보안 및 표준화** 제공 (직접 데이터베이스 접근을 차단하고, 인증 방식 제공)

---

## 2. **REST API (Representational State Transfer API)란?**
REST API는 **HTTP 프로토콜을 기반으로 한 API**입니다.  
2000년대 초반, **로이 필딩(Roy Fielding)**이 정의한 REST 원칙을 따르는 API 방식입니다.

### **REST의 핵심 원칙**
1. **클라이언트-서버 구조**
    - 클라이언트(웹, 모바일)와 서버(백엔드 API)가 분리됨
2. **무상태성 (Stateless)**
    - 서버가 요청 간 상태(세션)를 저장하지 않음 → 확장성(Scalability) 증가
3. **캐시 가능 (Cacheable)**
    - 응답 데이터는 캐싱 가능해야 하며, 성능 최적화 가능
4. **계층화된 시스템 (Layered System)**
    - API는 여러 레이어(인증, 로드 밸런싱)를 통해 구성될 수 있음
5. **일관된 인터페이스 (Uniform Interface)**
    - API의 URL 규칙과 요청 형식을 일관되게 유지
6. **Code on Demand (선택적 원칙)**
    - 서버에서 실행 가능한 스크립트 또는 코드 전송 가능 (거의 사용되지 않음)

---

## 3. **RESTful API란?**
REST 원칙을 **충실히 따르는 API**를 **RESTful API**라고 합니다.  
즉, RESTful API는 **REST 아키텍처 스타일을 준수한 API**입니다.

### **RESTful API의 특징**
✅ **HTTP 메서드 활용**  
| 메서드  | 기능 | 예시 (Users API) |
|--------|-------------|-----------------|
| `GET`  | 데이터 조회 | `GET /users` (모든 유저 조회) |
| `POST` | 데이터 생성 | `POST /users` (유저 생성) |
| `PUT`  | 데이터 수정 | `PUT /users/{id}` (id로 유저 수정) |
| `PATCH`| 데이터 부분 수정 | `PATCH /users/{id}` (id로 일부 정보 수정) |
| `DELETE` | 데이터 삭제 | `DELETE /users/{id}` (id로 유저 삭제) |

✅ **리소스 기반 설계 (명사 사용)**
- **좋은 예시**: `/users`, `/products`, `/orders`
- **나쁜 예시**: `/getUsers`, `/deleteProduct?id=1`

✅ **JSON 데이터 형식 사용**
```json
{
  "id": 1,
  "name": "정현민",
  "email": "test@example.com"
}
```

✅ **URI는 동작이 아닌 리소스를 표현**
- `GET /users` ✅ (올바른 RESTful 방식)
- `GET /getUsers` ❌ (RESTful 원칙 위반)

---

## 4. **API의 종류**
API는 제공 방식과 사용 목적에 따라 여러 가지로 나뉩니다.

### **1) 개방형 API (Public API)**
- 누구나 사용할 수 있는 API
- 예) **카카오 맵 API, 네이버 로그인 API, 오픈웨더 API**
- 보안이 중요하므로 **OAuth 인증**을 사용함

### **2) 사설 API (Private API)**
- 내부 서비스용 API (외부 공개 X)
- 예) **사내 ERP 시스템 API, 내부 결제 API**
- 사내 개발팀만 접근 가능하며, **보안 및 성능 최적화** 초점

### **3) 파트너 API (Partner API)**
- 특정 기업과 제휴된 기업만 사용할 수 있는 API
- 예) **페이팔 결제 API, 은행 금융 API**
- **API 키, 토큰 인증**이 필요함

### **4) 그래프QL API (GraphQL API)**
- REST API의 단점을 해결한 API 방식
- **필요한 데이터만 요청 가능 → 최적화된 응답**
- 예) **페이스북, GitHub GraphQL API**

**👉 REST API vs GraphQL API 비교**
| 비교 항목 | REST API | GraphQL API |
|----------|---------|-------------|
| 요청 방식 | 여러 개의 엔드포인트 | 하나의 엔드포인트 |
| 응답 크기 | 정해진 데이터 구조 | 필요한 데이터만 요청 가능 |
| 성능 | 오버페치/언더페치 발생 가능 | 최소한의 데이터 전송 |

---

## 5. **API가 없었을 때는?**
- 과거에는 애플리케이션들이 **직접 데이터베이스를 조작**하거나, **파일을 통한 데이터 공유**를 했음
- **비효율적**이고 **보안 취약점**이 많았음
- API가 등장하면서 **표준화된 방식**으로 **안전하고 빠르게** 데이터를 주고받을 수 있게 됨

---

## 6. **정리**
- **API**: 애플리케이션 간 통신을 위한 인터페이스
- **REST API**: HTTP 기반 API로, REST 원칙을 따름
- **RESTful API**: REST 원칙을 철저히 준수한 API
- **API 종류**: Public, Private, Partner, GraphQL API

