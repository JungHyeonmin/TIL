### **1. 웹소켓(WebSocket)이란?**
웹소켓은 **클라이언트와 서버 간의 실시간 양방향 통신을 가능하게 하는 프로토콜**입니다.  
HTTP의 요청-응답 방식과 달리, **한 번 연결되면 지속적으로 데이터를 주고받을 수 있어** 실시간성이 중요한 서비스에 최적화되어 있습니다.

---

### **2. 웹소켓이 필요한 이유 (HTTP와의 차이점)**

| 비교 항목 | HTTP | 웹소켓(WebSocket) |
|-----------|------|------------------|
| 연결 방식 | 요청이 있을 때만 응답 | 지속적인 연결 유지 |
| 데이터 흐름 | 클라이언트 → 서버 요청 후 응답 | 클라이언트 ↔ 서버 양방향 실시간 통신 |
| 네트워크 부하 | 높은 편 (요청마다 새 연결) | 낮음 (한 번 연결 후 유지) |
| 지연 시간 | 상대적으로 높음 | 낮음 (실시간성 우수) |
| 주요 사용처 | 정적 웹사이트, API | 채팅, 주식 거래, 실시간 알림 |

💡 **즉, 웹소켓은 실시간 데이터 처리가 필요한 애플리케이션에 적합합니다.**

---

### **3. 웹소켓의 특징**

- **양방향(Full-Duplex) 통신**: 클라이언트와 서버가 동시에 데이터를 주고받을 수 있음
- **한 번 연결 후 유지**: HTTP와 다르게 매 요청마다 새 연결을 생성하지 않고 연결을 유지
- **낮은 오버헤드**: HTTP Polling보다 네트워크 리소스를 적게 사용
- **실시간 데이터 처리에 적합**: 채팅, 주식 거래, 실시간 알림 등

---

### **4. 소켓과 웹소켓 비교**

| 비교 항목    | 소켓(Socket)                 | 웹소켓(WebSocket)              |
|--------------|-----------------------------|--------------------------------|
| **프로토콜** | TCP, UDP                    | WebSocket (HTTP 업그레이드)    |
| **연결 방식** | 일반적으로 요청-응답 기반    | 양방향 연결 유지               |
| **사용 사례** | 서버-서버, 클라이언트-서버   | 브라우저-서버 (웹 기반)        |
| **통신 방식** | 직접 연결 (OS 레벨)          | HTTP에서 업그레이드 후 동작    |
| **주요 사용처** | 게임 서버, VoIP, 네트워크 프로그래밍 | 채팅, 실시간 알림, 스트리밍    |

---

### **5. 웹소켓이 나오기 전의 기술들**

웹소켓이 나오기 전, HTTP 기반의 **Polling** 기법들이 사용되었습니다. 이 방식들의 문제점은 네트워크 리소스를 많이 사용하고 실시간성에 한계가 있다는 점입니다.

- **HTTP Polling**: 클라이언트가 주기적으로 서버에 요청을 보내며 데이터를 확인
- **Long Polling**: 서버가 데이터를 준비할 때까지 응답을 지연하여 서버 요청 횟수를 줄임
- **SSE (Server-Sent Events)**: 서버가 클라이언트에게 단방향으로 데이터를 전송

웹소켓은 이러한 문제들을 해결하며 실시간 양방향 통신을 가능하게 했습니다.

---

### **6. 자바 백엔드 개발자가 웹소켓을 사용하는 실제 사례**

#### **1) 실시간 채팅 시스템**
- 예: 카카오톡, Slack 등
- **기술 스택**: `Spring Boot WebSocket`, `STOMP`, `Redis Pub/Sub`

#### **2) 주식 거래 시스템 & 금융 데이터 스트리밍**
- 예: 증권사의 MTS(모바일 트레이딩 시스템)
- **기술 스택**: `Spring Boot WebSocket`, `Kafka`, `Redis Stream`

#### **3) 실시간 알림(Notification) 시스템**
- 예: SNS, 전자상거래 사이트에서 새로운 알림 전송
- **기술 스택**: `Spring Boot WebSocket`, `Redis Pub/Sub`, `FCM`

#### **4) 온라인 협업 도구**
- 예: Google Docs, Figma에서 실시간 협업
- **기술 스택**: `Spring Boot WebSocket`, `Redis`, `WebRTC`

#### **5) HTML5 기반 멀티플레이 웹 게임**
- 예: 브라우저에서 실행되는 멀티플레이어 게임
- **기술 스택**: `Spring Boot WebSocket`, `Netty`, `WebRTC`

#### **6) IoT(사물인터넷) 시스템**
- 예: 스마트홈 시스템에서 실시간 센서 데이터 전송
- **기술 스택**: `Spring Boot WebSocket`, `MQTT`, `Kafka`

---

### **7. 웹소켓을 사용할 때 고려해야 할 점 (단점 및 대안)**

| 단점 | 해결 방법 |
|------|----------|
| **브라우저 지원 제한** | SockJS와 같은 폴백 라이브러리 활용 |
| **서버 부하 증가** | WebSocket 연결 수 제한, 로드 밸런싱 |
| **보안 취약점** | WSS(SSL/TLS) 사용 |

대안으로 **Server-Sent Events(SSE)**나 **gRPC**를 고려할 수 있습니다.

---

### **8. 자바 백엔드 개발자의 웹소켓 관련 기술 스택**

| 기술 | 설명 |
|------|------|
| **Spring Boot WebSocket** | 웹소켓을 구현하는 데 사용되는 자바 라이브러리 |
| **STOMP** | 메시징 프로토콜, 웹소켓에서 구조적인 메시지 처리 |
| **SockJS** | 웹소켓을 지원하지 않는 환경에서 폴백 라이브러리 |
| **Redis Pub/Sub** | 여러 서버 간 메시지 공유를 위한 기술 |
| **Kafka** | 대용량 실시간 데이터 스트리밍 처리 |

---

### **9. 결론: 자바 백엔드 개발자가 웹소켓을 사용해야 하는 경우**

- **실시간 데이터 처리**가 필요한 애플리케이션에 최적
- **HTTP Polling보다 효율적**으로 네트워크 부하를 줄이고 성능 개선
- **Spring Boot WebSocket과 STOMP**를 사용하면 쉽게 구현 가능

웹소켓은 **채팅, 금융 데이터, 실시간 알림, 협업 도구, IoT, 온라인 게임**에서 필수적인 기술입니다.