### POST와 PUT의 차이

PUT은 POST와 달리 리소스의 전체적인 경로를 알고있다.

PUT은 클라이언트가 위치를 알고있어야하고, 리소스가 있다면 대체한다.

### PATCH

PATCH는 리소스를 부분만 수정하는 것이다.

PUT은 리소스 전체를 수정 또는 추가하는 것이다.

일부 PATCH를 지원하지 않는 서버가 있는데 이럴때는 POST를 사용하자.

- GET도 리소스를 포함했을때, 지원하지 않는 서버가 있다면 POST를 사용하자

## HTTP 메서드의 속성

![img_1.png](img_1.png)
### 안전 safe

- 호출해도 리소스를 변경하지 않는다.

### 멱등 idempotent

- f(xf(x)) = f(x)
- 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 똑같다.
- 멱등 메서드
    - GET : 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
    - PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    - DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
    - POST : 멱등이 아니다! 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
- 활용
    - 자동 복구 메커니즘
    - 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도 되는가? 판단 근거

### 캐시 가능 cacheable

- 응답 결과 리소스를 캐시해서 사용해도 되는가?
- GET, HEAD, POST, PATCH 캐시 가능
- 실제로는 GET, HEAD 정도만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음

## HTML Form 데이터 전송

- HTML Form submit시 POST 전송
- Content-Type : appplication/x-www-form-rulencoded 사용
    - form의 내용을 메시지 바디를 통해서 전송(key=value, 쿼리 파라미터 형식)
    - 전송 데이터를 url encoding 처리
- HTML Form은 GET 전송도 가능
- Content-Type : multipart/form-data
    - 파일 업로드 같은 바이너리 데이터 전송시 사용
    - 다른 종류의 여러 파일과 폼의 내용 함께 전송가능(그래서 이름이 multipart)
- 참고 : HTML Form 전송은 GET, POST만 지원

## HTTP API 데이터 전송

- 서버 to 서버
    - 백엔드 시스템 통신
- 앱 클라이언트
    - 아이폰, 안드로이드..
- 웹 클라이언트
    - HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
    - 예) React, VueJs 같은 웹 클라이언트와 API 통신
- POST, PUT, PATCH : 메시지 바디를 통해 데이터 전송
- GET : 조회, 쿼리 파라미터로 데이터 전달
- Content-Type : application/json을 주로 사용(사실상 표준)
    - TEXT, XML, JSON 등등

## 클라이언트에서 서버로 데이터 전송 4가지 상황

- 정적 데이터 조회
    - 이미지, 정적 텍스트 문서
- 동적 데이터 조회
    - 주로 검색, 게시판 목록에서 정렬 필터(검색어)
- HTML Form을 통한 데이터 전송
    - 회원 가입, 상품 주문, 데이터 변경
- HTTP API를 통한 데이터 전송
    - 회원가입, 상품 주문, 데이터 변경
    - 서버 to 서버, 앱 클라이언트, 웹 클라이언트(Ajax)

# 참고하면 좋은 URI 설계 개념

[REST API URI Naming Conventions and Best Practices](https://restfulapi.net/resource-naming/)

### 문서(ducument)

- 단일 개념(파일 하나, 객체 인스턴스, 데이터 베이스 row)
- 예) /members/100, /files/start.jpg

### 컬렉션(collection)

- 서버가 관리하는 리소스 디렉터리
- 서버가 리소스의 URI를 생성하고 관리
- 예) /members

### 스토어(stores)

- 클라이언트가 관리하는자원 저장소
- 클라이언트가 리소스의 URI를 알고 관리
- 예) /files

### 컨트롤러(controller), 컨트롤URI

- 문서, 컬레션, 스토어로 해결하기 어려운 추가 프로세스 실행
- 동사를 직접 사용
- 예) /members/{id}/delete

# 상태 코드

클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능

### 1xx (Informational) : 요청이 수신되어 처리중

### 2xx (Successful) : 요청 정상 처리

- 200 OK : 요청 성공
- 201 Created : 요청 성공해서 새로운 리소스가 생성됨
- 202 Accepted : 요청이 접수되었으나 처리가 완료되지 않았음
- 204 No Content : 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음(save버튼)

### 3xx (Redirection) : 요청을 완료하면 추가 행동이 필요.

- **리다이렉트 설명**
    - 웹 브라우저는 3xx응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동

  리다이렉트 종류

    - **영구 리다이렉션** : 특정 리소스의 URI가 영구적으로 이동
        - 리소스의 URI가 영구적으로 이동
        - 원래의 URL를 사용X, 검색 엔진 등에서도 변경 인지
        - 301, 308
        - 예) /members → /users
        - 예) /event → /new-event
    - **일시 리다이렉션** - 일시적인 변경
        - 리소스의 URI가 일시적으로 변경
        - 따라서 검색 엔진 등에서 URL을 변경하면 안됨
        - 302, 303, 307
        - 예) 주문 완료 후 주문 내역 화면으로 이동
        - PRG : Post/Redirect/Get
            - POST로 주문 후 브라우저를 새로고침하면? → 중복 주문이 될 수 있다.
            - 클라이언트 해결 방법 : POST로 주문 후에 주문 결과 화면을 GET메서드로 리다이렉트한다. 새로고침해도 결과화면을 GET으로 조회한다.
            - 백엔드 해결방법 : 주문 번호를 확인하여 방지

      ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e00b2a66-cb1a-4908-9dca-d58fe6200c6c/8b33ff46-fa3f-4fbb-aba8-f72537c2340e/image.png)

    - **특수 리다이렉션**
        - 결과 대신 캐시를 사용
- 300 Multiple Choices : 안쓴다.
- 301 Moved Permanently : 영구 리다이렉트, 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)
- 302 Found : 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음(MAY)

  **→ GET으로 변할 수 있음**

- 303 See Other : 리다이렉트시 요청 메서드가 GET으로 변경, 302와 기능은 같음
- 304 Not Modified : 캐시를 목적으로 사용, 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬PC에 저장된 캐시를 재사용한다.(캐시로 리다이렉트 한다.), **304응답은 응답에 메시지 바디를 포함하면 안된다.(로컬 캐시를 사용해야 하므로)**, 조건부 GET,HEAD 요청시 사용
- 307 Temporary Redirect : 리다이렉트시 요청 메서드와 본문 유지(**요청 메서드를 변경하면 안된다. MUST NOT**), 302와 기능은 같음

  **→ 메서드가 변하면 안됨**

- 308 Permanent Redirect : 영구 리다이렉트, 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST 유지), 301과 기능은 같음

  **→ 메서드가 GET으로 변경**


### 4xx (Client Error) : 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음

- 클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- **오류의 원인이 클라이언트에 있음**
- 중요! 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에, 똑같은 재시도가 실패함
- 400 Bad Request : 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
    - 요청 구문, 메시지 등등 오류
    - 클라이언트는 요청 내용을 다시 검토하고, 보내야함
    - 예) 요청 파라미터가 잘못되거나, API스펙이 맞지 않을 때
- 401 Unauthorized : 클라이언트가 해당 리소스에 대한 인증이 필요함
    - 인증(Authentication) 되지 않음
    - 401 오류 발생시 응답에 WWW-Authenticate 헤서와 함께 인증 방법을 설명
    - 참고
        - 인증(Authentication): 본인이 누구인지 확인, (로그인)
        - 인가(Authorization):권한부여(ADMIN 권한처럼 특정 리소스에 접근할 수 있는 권한, 인증이 있어야 인가가 있음)
        - 오류 메시지가 Unauthorized이지만 인증되지 않음(이름이 아쉬움)
- 403 Forbidden : 서버가 요청을 이해했지만 승인을 거부함
    - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
    - 예) 어드민 등급이 아닌 사용자가 로그인은 했지만 어드민 등급의 리소스에 접근하는 경우
- 404 Not Found : 요청 리소스를 찾을 수 없음
    - 요청 리소스가 서버에 없음
    - 또는 클라이언트가 원한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때

### 5xx (Server Error) : 서버 오류, 서버가 정상 요청을 처리하지 못함

- 비즈니스 로직에서 진짜 문제가 생겼을 때만 사용한다.
- 서버 문제로 오류 발생
- 서버에 문제가 있기 때문에 재시도 하면 성공할 수도 있음(복구가 되거나 등등)
- 500 Internal Server Error : 서버 문제로 오류 발생
    - 서버 냅 문제로 오류 발생
    - 애매하면 500 오류
- 503 Service Unavailable : 서비스 이용 불가
    - 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
    - Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수도 있음

# HTTP 헤더 - 일반 헤더

## 용도

- HTTP 전송에 필요한 모든 부가정보
- 예) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시관리 정보…
- 표준 헤더가 너무 많은
- 필요시 임의의 헤더 추가 기능
    - helloworld: hihi

## HTTP BODY

### message body - RFC7230(최신)

- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드(payload)
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 포현 데이터**를 해설할 수 있는 정보 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
- 참고 : 표현 헤더는 표현 메타데이터와, 페이로드 메시지를 구분해야 한다.

## 표현

- **표현 헤더는 전송, 응답 둘다 사용한다.**
- Content-Type : 표현 데이터의 형식 설명
    - **미디어 타입, 문자 인코딩**
    - 예)
        - text/html; charset=utf-8
        - application/json
        - image/png
- Content-Encoding : 표현 데이터의 압축 방식
    - **표현 데이터를 압축하기 위해 사용**
    - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
    - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
    - 예)
        - gzip
        - deflate
        - identity
- Content-Language : 표현 데이터의 자연 언어
    - **표현 데이터의 자연 언어를 표현**
    - 예)
        - ko
        - en
        - en-US
- Content-Length : 표현 데이터의 길이
    - 바이트 단위
    - **Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨**

## 협상(콘덴츠 네고시에이션)

### 클라이언트가 선호하는 표현 요청

- **협상헤더는 요청시에만 사용**
- Accept :  클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset : 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding : 클라이언트가 선호하는 압축 인코딩
- Accept-Language : 클라이언트가 선호하는 자연 언어(클라이언트가 원하는 언어로 전달)

### 협상과 우선순위1 Quality Values(q)

- Quality Values(q) 값 사용
- 0~1, 클수록 높은 우선순위
- 생략하면 1
- Accept-Leanguage: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    - 우선순위 1등 : ko-KR;q=1 (q생략)
    - 우선순위 2등 : ko;q=0.9
    - 우선순위 3등 : en-US;q=0.8
    - 우선순위 4등 : en;q=0.7

### 협상과 우선순위2 Quality Values(q)

- 구체적인 것이 우선이다.
- Accept: text/*, text/plain, text/plain;format=flowed, */*
    - 우선순위 1등 : text/plain;format=flowed
    - 우선순위 2등 : text/plain
    - 우선순위 3등 : text/*
    - 우선순위 4등 : */*

### 협상과 우선순위3 Quality Values(q)

- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
- Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5


    | Media Type | Quality |
    | --- | --- |
    | text/html;level=1 | 1 |
    | text/html | 0.7 |
    | text/plain | 0.3 |
    | image/jpeg | 0.5 |
    | text/html;level=2 | 0.4 |
    | text/html;level=3 | 0.7 |

## 전송 방식

- 단순 전송 Content-Length: 한번에 보내기
- 압축 전송 Content-Encoding: 인코딩해서 보내기
- 분할 전송 Transfer-Encoding: 분할해서 보내기
    - Content-Language 사용 금지
- 범위 전송 Range, Content-Range: 범위를 지정해서 보내기, 서버가 범위내의 값을 받으면 마지막까지 전송 요청 보냄

## 일반 정보

- From: 유저 에이전트의 이메일 정보
    - 일반적으로 잘 사용되지 않음
    - 검색 엔진같은 곳에서, 주로 사용
    - 요청에서 사용
- Referer: 이전 웹 페이지 주소
    - 현재 요청된 페이지의 이전 웹페이지 주소
    - A → B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
    - Referer를 사용해서 유입 경로 분석 가능
    - 요청에서 사용
    - 참고: referer는 단어 referrer의 오타
- User-Agent: 유저 에이전트 애플리케이션 정보
    - user-agent: Mozilla/5.0(…)…
    - 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
    - **통계 정보**
    - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
    - 요청에서 사용
- Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
    - ORIGIN 서버 : 요청이 도착하는 과정에 있는 프록시/캐시 서버 말고 마지막에 있는 서버
    - Server: Apache/2.2.22(Debian)
    - server: nginx
    - 응답에서 사용
- Date: 메시지가 생성된 날짜
    - Date: Tue, 15 Nov 1994 08:12:31 GMT
    - 응답에서 사용

## 특별한 정보

- Host: 요청한 호스트 정보(도메인)
    - 요청에서 사용
    - 필수
    - 하나의 서버가 여러 도메인을 처리해야 할 때 구분해줌
    - 하나의 IP주소에 여러 도메인이 적용되어 있을 때 구분해줌
- Location: 페이지 리다이렉션
    - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동(리다이렉트)
    - 응답코드 3xx에서 설명
    - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
    - 3xx (Redirection): Location 값은 요청을 자동으로 리다이렉션하기 위한 대상 리소스를 가리킴
- Alliow: 허용 가능한 HTTP 메서드
    - 405(Method Not Allowed)에서 응답에 포함해야 함
    - Allow : GET, GEAD, PUT
- Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
    - 503 (Service Unavailable) : 서비스가 언제까지 불가능인지 알려줄 수 있음
    - Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
    - Retry-After: 120 (초단위 표기)

## 인증

- Authorization : 클라이언트 인증 정보를 서버에 전달
    - Authorization: Basic xxxxxxxxxxxxxxxx
- WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의
    - 리소스 접근시 필요한 인증 방법 정의
    - 401 Unauthorized 응답과 함께 사용
    - WWW-Authenticate: Newauth realm=”apps”, type=1, title=”Login to \”apps\””,Basic realm=”simple”

## 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
    - 예) set-cookie: sessionId=abcd1234; expires=Sat, 26-Dec-2020 00:00:00 GMT; path=/; domain=.goolge.com;Secure
        - 쿠키 - 생명주기 Expires, max-age
            - 만료일이 되면 쿠키 학제
            - 0이나 음수를 지정하면 쿠키 삭제
            - 세션쿠키 : 만료 날짜를 생략하면 브라우저 종료시까지만 유지
            - 영속 쿠키 : 만료 날짜를 입력하면 해당 날짜까지 유지
        - 쿠키 - 도메인 Domain
            - 예) domain=example.org
            - **명시: 명시한 문서 기준 도메인 + 서브 도메인 포함**
            - doamin=example.org를 지정해서 쿠키 생성
                - example.org는 물론이고
                - dev.example.org도 쿠키 접근
            - **생락: 현재 문서 기준 도메인만 적용**
                - example.org 에서 쿠키를 생성하고 domain지정을 생략
                    - dev.example.org는 쿠키 미접근
        - 쿠키 - 경로 Path
            - 예) path=/home
            - **이 경로를 포함한 하위 경로 페이지만 쿠키 접근**
            - **일반적으로 path=/ 루트로 지정**
            - 예)
                - **path=/home 지정**
                - /home → 가능
                - /home/level1 → 가능
                - /home/level1/level2 → 가능
                - /hello → 불가능
        - 쿠키 - 보안 Secure, HttpOnly, SameSite
            - Secure
                - 쿠키는 http, https를 구분하지 않고 전송
                - Secure를 적용하면 https인 경우에만 전송
            - HttpOnly
                - XSS 공격 방지
                - 자바스크립트에서 접근 불가(document.cookie)
                - HTTP 전송에만 사용
            - SameSite
                - XSRF공격 방지
                - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
    - 사용처
        - 사용자 로그인 세션 관리
        - 광고 정보 트래킹
    - 크키 정보는 항상 서버에 전송됨
        - 네트워크 트래픽 추가 유발
        - 최소한의 정보만 사용(세션 id, 인증 토큰)
        - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지(localStorage, sessionStorage) 참고
    - 주의!
        - 보안에 민감한 데이터는 저장하면 안됨(주민번호, 신용카드 번호 등등)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달

# HTTP 헤더2 - 캐시와 조건부 요청

## 캐시 기본 동작

### 캐시가 없을 때

- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리가.
- 느린 사용자 경험

### 캐시 시간 초과

- 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다.
- 이때 다시 네트워크 다운로드가 발생한다.
- 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두가지 상황이 나타난다.
    1. 서버에서 기존 데이터를 변경함
    2. 서버에서 기존 데이터를 변경하지 않음

## 검증 헤더와 조건부 요청

- 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면
- 304 Not Modified + 헤더 메타 정보만 응답(바디X)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
- 클라이언트는 캐시에 저장되어 있는 데이터 재활용
- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드한다.
- 매우 실용적인 해결책
- 검증 헤더
    - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
    - Last-Modified, ETag

### 조건부 요청 헤더

- 검증 헤더로  조건에 따른 분기
- If-Modified-Since: Last-Modified 사용
- If-Modified-Since: ETag 사용
- 조건이 만족하면 200 OK
- 조건이 만족하지 않으면 304 Not Modified
- Last-Modified, If-Modified-Since 단점
    - 1초 미만(0.x초) 단위로 캐시 조정이 불가능
    - 날짜 기반의 로직 사용
    - 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
    - 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
        - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우
- ETag, If-None-Match
    - ETag(Entity Tag)
    - 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
        - 예)ETag: “c1.0”, ETag: “a2jiodwjekjl3”
    - 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
        - 예) ETag: “aaaaa” → ETag: “bbbbb”
    - 진짜 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기!
- ETag, If-None-Match 정리
    - 진짜 단순하게 ETag만 서버에 보내서 같으면 유지, 다르면 다시 받기
    - **캐시 제어 로직을 서버에서 완전히 관리**
    - 클라이언트는 단순히 이 값을 서버에 제공(클라이언트는 캐시 메커니즘을 모름)
    - 예)
        - 서버는 베타 오픈기간인 3일 동안 파일이 변경되어도 ETag를 동일하게 유지
        - 애플리케이션 배포 주기에 맞추어 ETag 모두 갱신