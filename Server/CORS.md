## **CORS (Cross-Origin Resource Sharing)란?**

CORS(교차 출처 리소스 공유, Cross-Origin Resource Sharing)는 웹 브라우저에서 **다른 출처(origin)의 리소스를 요청할 때 보안 정책을 적용하는 메커니즘**이다. 기본적으로 웹 브라우저는 **동일 출처 정책(Same-Origin Policy, SOP)** 을 따르기 때문에, 출처가 다른 리소스를 가져오는 것을 차단한다. 그러나 CORS를 설정하면 특정 출처의 요청을 허용할 수 있다.

---

## **1. 왜 CORS가 필요한가?**
웹 애플리케이션을 개발할 때, 클라이언트(브라우저)와 서버가 서로 다른 출처를 가질 수 있다. 예를 들어:

- 클라이언트: `https://frontend.example.com`
- 백엔드 API 서버: `https://api.example.com`

위와 같이 출처가 다르면, 브라우저는 보안 정책에 따라 기본적으로 API 요청을 차단한다. 이를 해결하기 위해 **CORS 정책을 적용하면 특정 출처의 요청을 허용할 수 있다.**

---

## **2. 출처(Origin)의 개념**
웹에서 출처(Origin)는 다음 **세 가지 요소**로 결정된다.

1. **프로토콜 (Protocol)** - `http://` 또는 `https://`
2. **도메인 (Domain)** - `example.com`
3. **포트 (Port)** - `:3000`, `:8080` 등

출처가 동일하려면 위 세 가지가 **완전히 동일**해야 한다. 예를 들어:
- ✅ `https://example.com:443` → `https://example.com:443` (같은 출처)
- ❌ `https://example.com` → `http://example.com` (프로토콜 다름)
- ❌ `https://example.com` → `https://api.example.com` (도메인 다름)
- ❌ `https://example.com` → `https://example.com:3000` (포트 다름)

---

## **3. CORS 동작 방식**
CORS는 서버가 특정 출처의 요청을 허용할 수 있도록 응답 헤더를 추가하는 방식으로 동작한다.

### **(1) 단순 요청 (Simple Request)**
- 요청이 다음 조건을 만족하면 **Preflight 요청 없이 바로 요청이 전송**된다.
    - `GET`, `POST`, `HEAD` 메서드만 사용
    - `Content-Type`이 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 중 하나
    - 사용자 지정 헤더(`Authorization`, `X-Custom-Header` 등)가 없음

**서버 응답 예시:**
```http
Access-Control-Allow-Origin: https://frontend.example.com
```

### **(2) Preflight 요청 (OPTIONS 요청)**
- 클라이언트가 **보안에 민감한 요청 (예: `PUT`, `DELETE`, `PATCH` 등)** 을 보낼 때, **브라우저는 먼저 OPTIONS 메서드를 사용해 서버가 요청을 허용하는지 확인**한다.
- 이 과정을 **Preflight 요청**이라고 한다.

**Preflight 요청 예시 (OPTIONS)**
```http
OPTIONS /data HTTP/1.1
Origin: https://frontend.example.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization
```

**서버 응답 예시**
```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://frontend.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Max-Age: 3600
```

---

## **4. CORS 설정 방법 (Spring Boot 예제)**

### **(1) Spring Boot에서 CORS 설정하는 방법**
#### **1. Controller 단위에서 설정**
```java
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@CrossOrigin(origins = "https://frontend.example.com") // 특정 Origin 허용
@RestController
@RequestMapping("/api")
public class TestController {
    
    @GetMapping("/test")
    public String test() {
        return "CORS 설정 완료!";
    }
}
```
- 특정 출처(`https://frontend.example.com`)의 요청만 허용함.

#### **2. Global 설정 (WebMvcConfigurer)**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig {
    
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**") // 모든 엔드포인트 적용
                        .allowedOrigins("https://frontend.example.com") // 허용할 출처
                        .allowedMethods("GET", "POST", "PUT", "DELETE") // 허용할 HTTP 메서드
                        .allowedHeaders("*") // 모든 헤더 허용
                        .allowCredentials(true); // 쿠키 포함 요청 허용
            }
        };
    }
}
```

#### **3. Spring Security와 함께 CORS 설정**
Spring Security를 사용하는 경우, `SecurityFilterChain`에서도 CORS를 허용해야 한다.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .cors() // CORS 설정 적용
            .and()
            .csrf().disable() // CSRF 보호 비활성화 (필요에 따라 설정)
            .authorizeHttpRequests(auth -> auth.anyRequest().permitAll());
        return http.build();
    }
}
```

---

## **5. CORS 관련 오류 및 해결 방법**
### **(1) `CORS policy: No 'Access-Control-Allow-Origin' header is present`**
#### 🔍 원인:
- 서버에서 CORS 설정이 누락됨.

#### ✅ 해결 방법:
- 서버 응답 헤더에 `Access-Control-Allow-Origin` 추가.

---

### **(2) `CORS policy: Method DELETE is not allowed by Access-Control-Allow-Methods`**
#### 🔍 원인:
- 서버가 `DELETE` 메서드를 허용하지 않음.

#### ✅ 해결 방법:
- `Access-Control-Allow-Methods` 설정에 `DELETE` 추가.

---

### **(3) `CORS policy: Response to preflight request doesn’t pass access control check`**
#### 🔍 원인:
- 서버가 `OPTIONS` 요청을 처리하지 않음.

#### ✅ 해결 방법:
- Preflight 요청(`OPTIONS`)을 허용하도록 설정 (`CorsConfig` 추가).

---

## **6. CORS 없이 다른 출처와 통신하는 방법**
CORS를 적용하지 않고 다른 출처의 리소스를 요청할 수 있는 방법:
1. **JSONP (JSON with Padding)**
    - `script` 태그를 사용해 데이터를 요청하는 방식 (현재는 거의 사용되지 않음).
2. **프록시 서버 사용**
    - 백엔드 서버가 대신 요청을 수행하고 결과를 클라이언트에 반환하는 방법.
    - 예제: `nginx`나 `Spring Boot`에서 프록시 설정을 추가.

---

## **7. 결론**
- CORS는 브라우저 보안을 위한 정책이지만, API 서버에서 적절히 설정하면 특정 출처의 요청을 허용할 수 있다.
- Spring Boot에서는 `@CrossOrigin`, `WebMvcConfigurer`, `SecurityFilterChain` 등을 활용하여 CORS 설정이 가능하다.
- CORS 오류가 발생하면 **서버 응답 헤더를 점검**하고 **Preflight 요청을 확인**해야 한다.