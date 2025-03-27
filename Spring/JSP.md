# JSP (Java Server Pages) 및 관련 개념 정리

## 1. JSP (Java Server Pages)란?
**JSP(Java Server Pages)**는 Java를 기반으로 동적인 웹 페이지를 생성하는 기술입니다. **HTML, CSS, JavaScript 등의 정적인 웹 요소에 Java 코드를 삽입하여 동적인 웹 페이지를 만들 수 있는 서버 측 기술**입니다. JSP는 **서블릿(Servlet)**을 보다 쉽게 사용할 수 있도록 도와주는 기술로 볼 수 있습니다.

---

## 2. JSP의 특징
1. **서버 사이드 기술**
    - JSP는 클라이언트가 요청하면 서버에서 실행되어 HTML을 생성하고, 이를 클라이언트(웹 브라우저)에 반환합니다.

2. **HTML 내에 Java 코드 삽입 가능**
    - JSP 페이지 안에서 `<% %>` 태그를 사용하여 Java 코드를 직접 작성할 수 있습니다.

3. **서블릿과의 관계**
    - JSP는 내부적으로 **서블릿(Servlet)**으로 변환되어 실행됩니다.
    - 개발자는 서블릿보다 더 직관적으로 HTML과 Java를 혼합하여 작성할 수 있습니다.

4. **MVC 패턴에서 주로 View 역할 수행**
    - JSP는 웹 애플리케이션의 **뷰(View)**를 담당하며, 비즈니스 로직 처리는 보통 **서블릿이나 서비스(Service) 계층에서 담당**합니다.

---

## 3. JSP 동작 원리
### 📌 JSP 요청 처리 과정
1. 클라이언트가 **JSP 페이지를 요청**함.
2. 웹 컨테이너(Tomcat 등)가 JSP 파일을 **서블릿( `.java` 파일 )으로 변환**.
3. 변환된 서블릿을 **컴파일하여 클래스 파일(`.class`)** 생성.
4. 서블릿이 실행되어 **HTML을 생성**한 후 클라이언트에게 응답.
5. 클라이언트는 최종적으로 HTML을 받아 웹 브라우저에서 출력.

---

## 4. JSP의 주요 문법 및 태그
### 4.1 스크립트 요소 (Java 코드 삽입)
JSP에서 Java 코드를 삽입하는 방법은 크게 3가지가 있습니다.

#### (1) **스크립트릿 (Scriptlet)**
```jsp
<% 
    int count = 10;
    out.println("현재 카운트: " + count);
%>
```
- `<% %>` 내부에 Java 코드를 삽입할 수 있음.
- 비추천 방식 (MVC 패턴 위반 가능성이 높음).

#### (2) **표현식 (Expression)**
```jsp
현재 시간: <%= new java.util.Date() %>
```
- `<%= %>` 안의 Java 코드의 결과값이 HTML에 직접 출력됨.

#### (3) **선언문 (Declaration)**
```jsp
<%! int sum(int a, int b) { return a + b; } %>
```
- `<%! %>` 안의 코드는 멤버 변수나 메서드를 선언하는 데 사용됨.

---

### 4.2 디렉티브 태그 (Directive)
**JSP 페이지에 대한 설정을 정의하는 태그**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
```
- `language="java"` : JSP에서 사용할 언어 지정
- `contentType="text/html; charset=UTF-8"` : 응답 데이터 타입 및 문자 인코딩 설정

---

### 4.3 액션 태그 (JSP 기본 기능 제공)
JSP의 기능을 수행하는 태그들입니다.

#### (1) `jsp:include` (다른 JSP 파일 포함)
```jsp
<jsp:include page="header.jsp" />
```
- 다른 JSP 페이지를 포함 (HTML 조각을 재사용할 때 유용).

#### (2) `jsp:forward` (페이지 이동)
```jsp
<jsp:forward page="nextPage.jsp" />
```
- 현재 페이지에서 `nextPage.jsp`로 요청을 전달.

#### (3) `jsp:param` (파라미터 전달)
```jsp
<jsp:include page="header.jsp">
    <jsp:param name="title" value="홈페이지" />
</jsp:include>
```
- 포함된 페이지에 추가 파라미터 전달.

---

## 5. JSP의 주요 내장 객체 (Implicit Objects)
JSP에는 기본적으로 제공되는 객체들이 있습니다.

| 내장 객체 | 설명 |
|-----------|-------------|
| `request` | HTTP 요청 정보 (`HttpServletRequest`) |
| `response` | HTTP 응답 정보 (`HttpServletResponse`) |
| `session` | 사용자 세션 정보 (`HttpSession`) |
| `application` | 애플리케이션 범위 데이터 (`ServletContext`) |
| `out` | 출력 스트림 (`JspWriter`) |
| `page` | 현재 JSP 페이지 객체 |
| `config` | Servlet 설정 정보 (`ServletConfig`) |
| `pageContext` | JSP 페이지 컨텍스트 |

### 예제: `request` 객체 사용
```jsp
<%
    String name = request.getParameter("name");
    out.println("이름: " + name);
%>
```
- 클라이언트가 URL에 `name=홍길동`을 포함하면 `홍길동`이 출력됨.

---

## 6. JSP 관련 개념 (JSP와 함께 사용되는 기술들)

### 6.1 서블릿(Servlet)
JSP의 핵심 개념 중 하나로, **JSP는 내부적으로 서블릿으로 변환**됩니다.  
서블릿은 Java 코드만으로 클라이언트 요청을 처리하는 웹 기술입니다.

**서블릿과 JSP의 차이점**
| 항목 | 서블릿 (Servlet) | JSP (Java Server Pages) |
|------|------------------|------------------|
| 작성 방식 | Java 코드로 작성 | HTML에 Java 코드 포함 가능 |
| 유지보수 | 어렵다 (HTML 삽입이 복잡함) | 쉽다 (HTML 중심) |
| 역할 | 주로 컨트롤러 역할 (요청 처리) | 주로 뷰 역할 (응답 생성) |

---

### 6.2 EL (Expression Language)
EL은 JSP에서 간결하게 값을 출력하는 방법을 제공합니다.
```jsp
이름: ${param.name}
```
- `param.name`은 `request.getParameter("name")`과 동일한 역할을 함.

---

### 6.3 JSTL (JSP Standard Tag Library)
JSP에서 자주 사용되는 기능(반복문, 조건문, 출력 등)을 태그로 제공하는 라이브러리입니다.

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:if test="${user == 'admin'}">
    <p>관리자 계정입니다.</p>
</c:if>

<c:forEach var="i" begin="1" end="5">
    <p>번호: ${i}</p>
</c:forEach>
```
- 조건문과 반복문을 사용할 수 있어 **JSP 코드의 가독성이 향상됨**.

---

## 7. JSP의 한계점과 대안
### ✅ JSP의 한계점
1. **비즈니스 로직과 뷰(View) 코드가 섞이기 쉬움**
    - 유지보수 및 확장성이 떨어짐.
2. **서블릿 변환 과정으로 인해 실행 속도가 다소 느릴 수 있음**.
3. **HTML과 Java 코드 혼합이 가독성을 해침**.

### ✅ JSP의 대안
- **Spring MVC** : 뷰(View)와 로직을 분리하여 구조적인 웹 개발 가능.
- **Thymeleaf** : HTML 기반 템플릿 엔진, JSP 대체 가능.

---

## 8. 정리
| 개념 | 설명 |
|------|------|
| JSP | 서버에서 실행되는 Java 기반 웹 페이지 기술 |
| 서블릿 | Java 코드로 클라이언트 요청을 처리하는 웹 기술 |
| EL | JSP에서 데이터를 쉽게 출력할 수 있는 표현 언어 |
| JSTL | JSP에서 조건문, 반복문 등을 쉽게 사용하게 해주는 라이브러리 |