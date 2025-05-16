# 🌐 서블릿(Servlet) 완전 정복

---

## ✅ 1. 개념 (What)

> **서블릿**은 자바를 사용해서 **웹 요청을 처리**하고, **응답을 생성**하는 서버 사이드 컴포넌트다.
> `javax.servlet.http.HttpServlet` 클래스를 상속해서 HTTP 요청/응답을 제어할 수 있어.

### 핵심 키워드

* 자바 기반 서버 기술
* 요청(Request), 응답(Response)
* Tomcat 같은 서블릿 컨테이너에서 실행

---

## ✅ 2. 왜 쓰는가? (Why)

| 목적            | 설명                                  |
| ------------- | ----------------------------------- |
| ✅ 동적 웹 페이지 처리 | 클라이언트의 요청에 따라 HTML, JSON 등 동적 응답 생성 |
| ✅ HTTP 제어     | 파라미터, 세션, 쿠키, 헤더 등을 직접 제어 가능        |
| ✅ 플랫폼 독립성     | 자바 기반이라 OS, WAS에 독립적                |
| ✅ 구조적 웹 프로그래밍 | 요청 흐름과 로직을 구조화 가능 (JSP보다 우수)        |

---

## ✅ 3. 어떻게 사용하는가? (How)

### 기본 흐름

```java
@WebServlet("/hello")
public class HelloServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
        
        String name = req.getParameter("name"); // 요청 파라미터 받기
        resp.setContentType("text/html"); // 응답 형식 설정
        PrintWriter out = resp.getWriter();
        
        out.println("<html><body>");
        out.println("<h1>Hello, " + name + "</h1>");
        out.println("</body></html>");
    }
}
```

### 생명주기

1. `init()` – 최초 1번만 실행 (초기화)
2. `service()` – 요청마다 실행
3. `doGet()`, `doPost()` – HTTP 방식에 따라 분기
4. `destroy()` – 종료 시 1번 실행

---

## ✅ 4. 발전 과정 (History)

| 단계                     | 설명                                      |
| ---------------------- | --------------------------------------- |
| 🔹 CGI                 | 요청마다 프로세스 생성 (비효율적)                     |
| 🔹 Servlet (1세대)       | Java 코드로 요청/응답 처리                       |
| 🔹 JSP (2세대)           | HTML 안에 Java 코드 사용                      |
| 🔹 Servlet + JSP + MVC | Controller와 View 분리                     |
| 🔹 Spring MVC          | DispatcherServlet, HandlerMapping 등 추상화 |
| 🔹 Spring Boot         | 설정 자동화 + REST API 중심 개발                 |

---

## ✅ 5. 없었을 때는? (Before Servlet)

### 🔻 CGI (Common Gateway Interface)

```bash
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><body>Hello</body></html>";
```

* 요청마다 별도의 OS 프로세스를 생성 → 느리고 자원 낭비
* 보안, 성능, 구조화 모두 부족

👉 그래서 JVM 기반으로 성능, 구조, 재사용성을 갖춘 **서블릿**이 등장하게 됨

---

## ✅ 6. 장점 / 단점 (Pros / Cons)

| 장점                        | 단점                           |
| ------------------------- | ---------------------------- |
| ✅ 자바 기반 → 강력한 언어 기능       | ❌ HTML 출력 불편 (`PrintWriter`) |
| ✅ HTTP 세부 제어 가능           | ❌ 코드가 길고 지저분함                |
| ✅ 객체 재사용 (스레드 기반) → 성능 우수 | ❌ MVC 분리가 안 됨                |
| ✅ WAS에서 관리되므로 안정성 ↑       | ❌ 생산성 ↓ → Spring MVC 등으로 해결  |

---

## ✅ 7. 비슷한 기술 / 대체 기술 (Alternative)

| 기술                     | 설명                 | Servlet과의 관계                   |
| ---------------------- | ------------------ | ------------------------------ |
| JSP                    | HTML 안에 Java 코드 작성 | Servlet으로 변환되어 실행됨             |
| Spring MVC             | 컨트롤러 중심의 구조화된 웹 개발 | 내부에서 서블릿 사용                    |
| REST API (Spring Boot) | JSON 기반 API 응답 중심  | 서블릿보다 추상화되고 간단함                |
| JSF, Struts            | 예전 Java 기반 웹 프레임워크 | Servlet 위에서 동작                 |
| Node.js                | JS 기반 서버           | Servlet과 완전히 다른 플랫폼            |
| ASP.NET, PHP           | MS, LAMP 기반 서버     | Servlet과 유사한 위치 (서버 사이드 로직 처리) |

---

## ✅ 8. Servlet의 현재 위치

오늘날에도 `Servlet`은 여전히 **Spring, Spring Boot** 등의 **기초 인프라로 동작**해.
→ 너가 `@RestController`를 만들 때도, 내부에서는 DispatcherServlet → 서블릿 → HttpServlet 기반으로 작동하고 있어.

즉, 우리는 서블릿을 "직접 쓰지 않아도 **그 위에서 편하게 개발**하는 것"이야.

---

## ✅ 마무리 요약

| 항목    | 내용                               |
| ----- | -------------------------------- |
| 기술명   | Servlet (javax.servlet)          |
| 목적    | HTTP 요청/응답 처리                    |
| 특징    | Java 기반, 스레드 재사용, WAS 기반         |
| 단점    | HTML/Java 혼합, 복잡                 |
| 발전    | JSP → MVC → Spring MVC → Boot    |
| 대체 기술 | Spring, JSP, REST API, Node.js 등 |
