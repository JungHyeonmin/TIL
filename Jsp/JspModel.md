## ✅ JSP Model이란?

**JSP 기반 웹 애플리케이션에서 로직 처리와 화면(View)을 어떻게 나누는지에 대한 구조적 방식**을 뜻함.
**Model 1 / Model 2**로 구분되며, 특히 **Model 2는 MVC 패턴**을 따름.

---

## 🧱 JSP Model 1

### 구조

* **JSP 파일 하나가 모든 역할을 처리함 (View + Controller + Model까지)**
* 사용자의 요청을 JSP가 직접 받고, 내부에서 로직 처리하고, 결과 출력까지 담당

### 특징

* 빠르게 만들 수 있고 구조가 단순
* 로직과 뷰가 혼합되어 있어 유지보수 어려움
* 규모가 커지면 코드가 난잡해짐

### 예시

```jsp
<%@ page import="java.sql.*" %>
<%
    String id = request.getParameter("id");
    Connection conn = DriverManager.getConnection(...);
    PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id=?");
    stmt.setString(1, id);
    ResultSet rs = stmt.executeQuery();
%>
<html>
<body>
    <h1>사용자 정보</h1>
    <%= rs.getString("name") %>
</body>
</html>
```

→ JSP 안에서 DB 연결, 쿼리, HTML까지 다 처리함

---

## 🧱 JSP Model 2 (MVC 패턴)

### 구조

* **Model, View, Controller**가 분리됨
* JSP는 **View**만 담당
* \*\*서블릿(Servlet)\*\*이 Controller 역할을 함 → 요청 처리, Model 호출, View로 포워딩
* 비즈니스 로직(Model)은 자바 클래스(Service, DAO 등)로 따로 분리

### 특징

* 유지보수에 강하고, 코드 분리가 잘 되어 있음
* 대규모 개발, 협업에 적합
* Spring MVC의 기반 구조

### 흐름

```
[사용자 요청]
      ↓
[Controller(Servlet)]
      ↓ (데이터 처리, Model 호출)
[Model(Service/DAO)]
      ↓ (결과 저장)
[Controller → View(JSP)]
```

### 예시

```java
// Controller (Servlet)
String id = request.getParameter("id");
User user = userService.findUser(id);
request.setAttribute("user", user);
RequestDispatcher rd = request.getRequestDispatcher("/userView.jsp");
rd.forward(request, response);
```

```jsp
<!-- View (userView.jsp) -->
<h1>${user.name}님 환영합니다</h1>
```

---

## ✅ Model 1 vs Model 2 비교표

| 항목     | Model 1         | Model 2 (MVC)         |
| ------ | --------------- | --------------------- |
| 구조     | JSP 단독          | JSP + Servlet + Model |
| 역할 분리  | 없음 (JSP가 모든 역할) | 명확히 분리됨 (MVC 구조)      |
| 유지보수   | 어려움             | 쉬움                    |
| 재사용성   | 낮음              | 높음                    |
| 개발 난이도 | 쉬움 (작은 규모에 적합)  | 다소 복잡 (대규모에 적합)       |

---

## ✅ 왜 Model 2 (MVC)를 써야 할까?

* **협업이 편해짐 (디자이너 - 개발자 역할 분리)**
* **로직 재사용 가능 (Model은 다른 View에서도 재활용)**
* **유지보수 및 테스트 용이**
* 현대 프레임워크(Spring, Struts 등)의 기반이 됨

---

## ✅ 만약 JSP Model이 없었을 때는?

* JSP에 모든 코드가 뒤섞여 유지보수가 힘들고, 협업이 어려웠음
* HTML + Java 코드가 섞이면서 복잡한 비즈니스 로직 처리가 어려웠음

---

## ✅ 면접용 핵심 요약

> JSP Model이란, 웹 애플리케이션에서 요청 처리와 화면 출력을 어떻게 나누는지를 설명하는 구조 방식입니다.
> Model 1은 JSP가 모든 역할을 하고, Model 2는 MVC 패턴에 따라 역할을 나누어 유지보수가 쉬운 구조입니다.
> Model 2는 오늘날 대부분의 웹 프레임워크(Spring MVC 등)에서 표준적으로 사용됩니다.
