## 🌱 **스프링 컨테이너(Spring Container)란?**
스프링 컨테이너는 **스프링 프레임워크에서 객체의 생성과 생명 주기를 관리하는 핵심 요소**입니다.  
즉, 개발자가 직접 객체를 생성하고 관리하는 것이 아니라 **스프링 컨테이너가 대신 객체를 생성하고 관리**해 줍니다.

---

## 🔹 **1. 왜 스프링 컨테이너를 사용할까?**
스프링 컨테이너를 사용하는 이유는 다음과 같습니다.

✅ **제어의 역전(Inversion of Control, IoC)**
- 객체의 생성 및 관리를 개발자가 직접 하는 것이 아니라 **스프링이 대신 해줌**
- 코드의 의존성을 낮추고, 유지보수성을 높임

✅ **의존성 주입(Dependency Injection, DI)**
- 필요한 객체를 **자동으로 주입**하여 결합도를 낮추고 확장성을 높임

✅ **생명주기 관리**
- 객체의 **생성 → 초기화 → 사용 → 소멸** 과정을 컨테이너가 관리

✅ **AOP(Aspect-Oriented Programming) 지원**
- 공통 기능(트랜잭션, 로깅 등)을 모듈화하여 **비즈니스 로직과 분리** 가능

✅ **빈(Bean) 관리**
- 스프링 컨테이너는 **싱글톤 패턴**을 기본으로 객체를 관리하여 성능을 최적화

---

## 🔹 **2. 스프링 컨테이너의 종류**
스프링 컨테이너는 크게 **BeanFactory**와 **ApplicationContext**로 나뉩니다.

| 컨테이너 종류 | 설명 | 특징 |
|--------------|---------------------|-----------------------------|
| `BeanFactory` | 최소한의 기능을 가진 컨테이너 | 가벼운 컨테이너, 지연 초기화 지원 |
| `ApplicationContext` | BeanFactory + 부가 기능 | DI, AOP, 이벤트 리스너 등 지원 |

> 일반적으로 **ApplicationContext를 사용**하며, 대표적인 구현체는 다음과 같습니다.
> - **AnnotationConfigApplicationContext** : 자바 기반의 설정 파일 사용
> - **ClassPathXmlApplicationContext** : XML 기반의 설정 파일 사용
> - **GenericWebApplicationContext** : 웹 애플리케이션에서 사용

---

## 🔹 **3. 스프링 컨테이너의 동작 과정**
스프링 컨테이너는 다음과 같은 과정으로 동작합니다.

1️⃣ **설정 파일(자바 or XML)을 읽어들인다.**  
2️⃣ **객체(Bean) 생성**  
3️⃣ **의존성 주입(DI) 수행**  
4️⃣ **객체 초기화 (InitializingBean, @PostConstruct 등 활용 가능)**  
5️⃣ **객체 사용**  
6️⃣ **스프링 컨테이너 종료 시 객체 소멸 (DisposableBean, @PreDestroy 활용 가능)**

---

## 🔹 **4. 스프링 컨테이너 사용 예제**
### **🟢 자바 설정을 활용한 스프링 컨테이너 생성**
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MainApp {
    public static void main(String[] args) {
        // 스프링 컨테이너 생성
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        
        // 빈 가져오기
        MyService myService = context.getBean(MyService.class);
        
        myService.doSomething();
    }
}
```

### **🟢 설정 파일 (AppConfig.java)**
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {
    
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

### **🟢 빈 클래스 (MyService.java)**
```java
public class MyService {
    public void doSomething() {
        System.out.println("서비스 로직 실행 중...");
    }
}
```

---

## 🔹 **5. 스프링 컨테이너의 주요 개념**
| 개념 | 설명 |
|------|--------------------------------|
| `IoC (Inversion of Control)` | 객체의 생성과 관리를 스프링이 담당 |
| `DI (Dependency Injection)` | 의존성을 자동으로 주입 |
| `Bean` | 스프링이 관리하는 객체 |
| `Singleton` | 기본적으로 하나의 Bean 인스턴스를 공유 |
| `ApplicationContext` | BeanFactory를 확장한 컨테이너 |
| `AOP (Aspect-Oriented Programming)` | 공통 관심사를 분리하는 기법 |

---

## 🔹 **6. 스프링 컨테이너의 발전 과정**
| 시기 | 기술 |
|------|---------------------------------------------|
| **초기** | 객체를 직접 생성 및 관리 (new 연산자 사용) |
| **스프링 등장** | BeanFactory 기반의 IoC 컨테이너 제공 |
| **스프링 2.x** | ApplicationContext 도입 |
| **스프링 3.x** | Java 기반 설정 (@Configuration, @Bean) 도입 |
| **스프링 4.x~5.x** | 어노테이션 기반 DI(@Component, @Autowired) 강화 |

---

## 🔹 **7. 스프링 컨테이너가 없던 시절에는?**
스프링 컨테이너가 없던 시절에는 개발자가 객체를 직접 생성하고 관리해야 했습니다.

🔸 **문제점**
- 객체 생성 및 의존성 주입을 직접 관리해야 해서 **유지보수 어려움**
- 싱글톤 패턴을 직접 구현해야 해서 **코드 복잡도 증가**
- 객체 간 결합도가 높아 **확장성 부족**

🔹 **해결책**
- 스프링 컨테이너 도입으로 **객체의 생명주기를 자동으로 관리**
- **의존성 주입(DI)** 을 통해 **결합도 낮춤**
- **AOP 지원** 으로 **중복 코드 제거 및 성능 최적화**

---

## 🔥 **결론**
스프링 컨테이너는 객체의 생성과 생명주기를 관리하는 핵심 요소로,  
**IoC, DI, AOP 등을 지원하여 유지보수성과 확장성을 높이는 역할**을 합니다.  
기술면접에서는 **"스프링 컨테이너의 역할과 동작 과정"** 을 구체적으로 설명할 수 있도록 준비하면 좋습니다. 💪✨