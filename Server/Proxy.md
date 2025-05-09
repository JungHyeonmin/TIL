### 프록시(Proxy)란?
프록시는 클라이언트와 서버 사이에서 중계 역할을 하는 서버 또는 소프트웨어를 의미합니다. 클라이언트가 직접 서버에 요청을 보내는 것이 아니라, 프록시 서버를 통해 요청을 전달하고 응답을 받는 방식으로 동작합니다.
보안상의 문제로 직접 통신을 주고 받을 수 없는 사이에서 프록시를 이용하여 중계를 하는 개념
---

## 1. **프록시의 종류와 역할**
프록시는 용도에 따라 여러 가지로 나뉩니다.

### Proxy Server 사용 목적
- 익명성으로 보안의 목적으로 사용 
- 캐시를 이용한 속도향상
- 차단된 사이트를 IP 우회하여 접속 하지 않는 사이트를 차단

### ① **정방향 프록시(Forward Proxy)**
- 일반적인 프록시 서버를 말하며, 클라이언트와 웹 서버의 중계역할로 클라이언트가 프록시 서버에 요청한 내용을 프록시 서버에서 캐시로 저장해 두면,
  나중에 다시 데이터를 요청할 때 캐시된 데이터를 사용하면 되므로 전송 시간을 절약할 수 있습니다.
- 프록시 서버는 클라이언트가 요청하기 전까지 웹 서버의 주소를 알 수 없습니다.
- 캐싱 기능을 제공하면서 동시에 특정 사이트는 접근 불가능하도록 제한을 걸 수도 있습니다.
- 클라이언트가 요청을 전달하면 프록시 서버가 목적 사이트의 내용을 받아와서 전달을 '대신' 해주는 것입니다.
- 사용 사례:
    - **IP 우회 및 익명성 제공**: 사용자의 IP를 숨길 수 있음 (VPN과 유사)
    - **콘텐츠 필터링**: 기업이나 학교에서 특정 사이트 차단
    - **캐싱(Cache)**: 자주 요청되는 데이터를 저장하여 성능 최적화
- 
- 예시: 웹 브라우저에서 VPN을 설정하여 특정 웹사이트를 우회하는 경우

---

### ② **역방향 프록시(Reverse Proxy)**
- 클라이언트가 요청을 보낼 때 서버 앞단에 위치하여 요청을 중개하는 프록시입니다.
- 클라이언트와 내부망(Private Network) 서버 사이에 위치하여 제어역할을 합니다.
  클라이언트가 바로 서버에 데이터를 요청하여 받아올 수도 있지만, 그렇게 하면 중요한 데이터가 있는 DB 가 노출될 수 있따는 위험이 존재합니다. 따라서 중간에 프록시 서버를 두고 내부망을 보호하는 역할을 담당하게 하는 것 입니다.
- 클라이언트가 프록시 서버에 데이터를 요청하면 프록시 서버는 내부망 서버에서 데이터를 받아와 클라이언트에게 전달해 주는 개념입니다.
- 사용 사례:
    - **로드 밸런싱(Load Balancing)**: 여러 개의 서버로 요청을 분산시켜 부하를 줄임
    - **보안 강화**: 직접 서버 IP를 노출시키지 않음
    - **SSL/TLS 암호화 처리**: SSL 인증을 프록시 서버에서 수행하여 백엔드 서버 부담 감소
- 예시: **NGINX**, **Apache HTTP Server**를 이용한 리버스 프록시 설정

---

### ③ **트랜스페어런트 프록시(Transparent Proxy)**
- 클라이언트가 프록시 서버를 사용하고 있다는 것을 인식하지 못한 채 동작하는 프록시입니다.
- 사용 사례:
    - **기업 또는 ISP(인터넷 서비스 제공업체)에서 트래픽 모니터링**
    - **네트워크 성능 최적화**

---

## 2. **프록시가 없을 때의 문제점**
프록시가 없으면 클라이언트와 서버가 직접 통신해야 하며, 다음과 같은 문제가 발생할 수 있습니다.
- **서버 부하 증가**: 대량의 요청을 서버가 직접 처리해야 함
- **보안 취약**: 클라이언트가 직접 서버와 연결되므로 공격에 노출될 가능성이 높음
- **IP 차단 문제**: 특정 지역에서 IP 차단이 되면 접근 불가능

---

## 3. **프록시 서버의 발전 과정**
| 시대 | 특징 |
|------|------|
| **초기(1990년대)** | 단순한 캐싱과 요청 전달 역할 |
| **웹 2.0 시대(2000~2010년대)** | 로드 밸런싱, SSL 터미네이션 역할 강화 |
| **클라우드 & 컨테이너 시대(현재)** | API Gateway, Kubernetes Ingress Controller로 발전 |

---

## 4. **정리**
- **프록시는 클라이언트와 서버 사이에서 요청을 중개하는 역할을 한다.**
- **정방향 프록시**는 클라이언트가 외부 서버에 접근할 때 사용하며, **역방향 프록시**는 클라이언트 요청을 내부 서버로 전달하는 역할을 한다.
- **캐싱, 보안, 부하 분산, IP 우회** 등의 용도로 많이 사용된다.
- **대표적인 예시로 NGINX, Squid, Apache, HAProxy** 등이 있다.