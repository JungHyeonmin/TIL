# `pageContext.request.contextPath`란?

## ✅ 개념 정리

- `pageContext.request.contextPath`는 **현재 HTTP 요청이 발생한 웹 애플리케이션의 컨텍스트 경로(Context Path)**를 반환한다.
- JSP에서 자주 사용되며, 웹 애플리케이션 내의 **링크나 리소스 경로(URL)를 안정적으로 작성하기 위해 사용**된다.

---

## ✅ 용어 분석

| 구성 요소 | 설명 |
|-----------|------|
| `pageContext` | JSP 내장 객체로, 현재 JSP 페이지에 대한 정보와 기능을 제공함 |
| `request` | 현재 클라이언트의 HTTP 요청 정보를 담고 있는 객체 |
| `contextPath` | 현재 요청이 발생한 웹 애플리케이션의 루트 경로(식별자) |

---

## ✅ 예시

> 웹 애플리케이션 이름: `myApp`  
> 서버 주소: `http://localhost:8080`

이 경우 브라우저에서 접속할 때 전체 URL은:

```
http://localhost:8080/myApp
```

여기서 **`/myApp`** 이 바로 **Context Path**이다.

---

## ✅ JSP에서의 사용 예시

```jsp
<a href="${pageContext.request.contextPath}/member/loginForm.jsp">
    로그인
</a>
```

### 🔍 결과
`http://localhost:8080/myApp/member/loginForm.jsp`와 같이 동작하게 된다.

---

## ✅ 왜 이렇게 쓰는가?

| 이유 | 설명 |
|------|------|
| ✔ 유지보수성 | 애플리케이션 이름이 바뀌거나 경로가 변경되더라도 JSP 코드 수정이 필요 없음 |
| ✔ 유연성 | WAR 이름 또는 배포 설정에 따라 달라지는 Context Path를 자동으로 추적 |
| ✔ 확장성 | 여러 웹 앱이 하나의 톰캣 서버에서 동시에 동작할 수 있음 (Context로 구분) |

---

## ✅ 하드코딩과의 비교

```jsp
<!-- ❌ 하드코딩 (비추천) -->
<a href="/myApp/member/loginForm.jsp">로그인</a>
```

- 애플리케이션 이름 변경 시 모든 코드 수정 필요 → **비효율적**

```jsp
<!-- ✅ 동적 Context Path 사용 -->
<a href="${pageContext.request.contextPath}/member/loginForm.jsp">로그인</a>
```

- **Context Path 자동 적용** → 코드 재사용성과 유지보수에 유리

---

## ✅ 과거에는 어떻게 했나?

Servlet이나 JSP에서 `request.getContextPath()`를 직접 호출하거나 하드코딩 방식 사용
```jsp
<%
String path = request.getContextPath();
%>
<a href="<%=path%>/member/loginForm.jsp">로그인</a>
```

→ 유지보수 어려움, 코드 보기 복잡

---

## ✅ 스프링에서는?

JSP에서 JSTL 사용 시, `<c:url>` 태그를 활용해 Context Path 자동 처리 가능:

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:url value="/member/loginForm.jsp" var="loginUrl" />
<a href="${loginUrl}">로그인</a>
```

---

## ✅ JSP의 위치와 한계

- JSP는 **서버 측에서 HTML을 생성하는 기술**
- MVC 패턴에서 **View 역할**
- 서버에서 실행되고, 클라이언트에는 HTML만 전달 → **보안상 유리**
- 하지만 단점도 많음:
  - 자바 코드와 HTML이 섞여 **가독성/유지보수성↓**
  - 스프링 + 템플릿 엔진(Thymeleaf, FreeMarker) 혹은 SPA 프레임워크(React 등)에 밀림

---

## ✅ 결론

- `pageContext.request.contextPath`는 JSP에서 웹 애플리케이션의 상대 경로를 안정적으로 가져오는 핵심 도구
- 하드코딩보다 훨씬 유연하고 유지보수에 강함
- 웹 앱 구조가 바뀌더라도 자동 대응 가능
- **JSP 기반 프로젝트나 전자정부프레임워크처럼 레거시 환경에서 필수적으로 사용되는 개념**