## ✅ 2. `ViewResolver`

### 🔍 개념

* 컨트롤러에서 반환한 논리적인 뷰 이름(예: "home")을 실제 **JSP, Thymeleaf, HTML 파일 경로**로 변환해주는 역할을 해.

### 📌 사용 이유

* View 이름과 물리 경로를 **분리**할 수 있어서, 뷰 구조가 바뀌어도 컨트롤러는 변경할 필요 없어.
* 다양한 뷰 기술과 연동 가능 (JSP, Thymeleaf, PDF, Excel 등)

### 🧠 어떻게 동작?

```text
[Controller return "home"] → ViewResolver → "/WEB-INF/views/home.jsp"
```

### ⚙️ 설정 예시 (Spring XML)

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".jsp" />
</bean>
```