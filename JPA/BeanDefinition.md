## **BeanDefinition이란?**
Spring에서 `BeanDefinition`은 **스프링 컨테이너가 관리하는 빈(Bean)에 대한 메타데이터를 담고 있는 객체**이다.  
즉, 스프링 컨테이너는 `BeanDefinition`을 통해 **빈의 생성 방법, 라이프사이클, 속성, 의존성 주입 방식** 등을 정의하고 관리한다.

### **1. BeanDefinition의 주요 정보**
`BeanDefinition`이 담고 있는 주요 정보는 다음과 같다:

1. **빈 클래스 정보 (`beanClassName`)**
    - 해당 빈이 어떤 클래스를 기반으로 생성되는지 지정
    - 예) `com.example.MyService`

2. **스코프 (`scope`)**
    - 빈의 라이프사이클 범위를 지정
    - 예) `singleton` (기본값), `prototype`, `request`, `session`, `application`

3. **생성 방식 (`factoryMethodName` / `factoryBeanName`)**
    - 빈을 직접 생성할지, 특정 팩토리 메서드를 통해 생성할지 지정
    - 예) `factoryMethodName`이 `createInstance`라면 `MyService.createInstance()`를 호출하여 빈을 생성

4. **의존 관계 (`propertyValues`)**
    - 빈이 가질 프로퍼티 값 또는 DI(Dependency Injection) 정보
    - 예) `dataSource` 필드에 `DataSource` 빈 주입

5. **라이프사이클 메서드 (`initMethodName`, `destroyMethodName`)**
    - 빈이 초기화되거나 소멸될 때 호출할 메서드 지정
    - 예) `initMethodName="initService"` → 빈이 생성될 때 `initService()` 호출

---

## **2. BeanDefinition 관련 개념**
### **(1) ApplicationContext와 BeanFactory**
Spring 컨테이너에는 **`BeanDefinition`을 저장하고 빈을 생성하는 두 가지 주요 인터페이스**가 있다.

| 컨테이너 | 특징 |
|----------|---------------------------|
| **BeanFactory** | 기본적인 DI 컨테이너로, 필요할 때만 빈을 생성하는 **Lazy Initialization** 방식 |
| **ApplicationContext** | `BeanFactory`를 확장한 기능으로, **이벤트 처리, 국제화, AOP, 환경 설정 지원** 등 추가 기능 제공 |

📌 **ApplicationContext는 내부적으로 BeanFactory를 사용하여 빈을 관리하며, 빈 정의를 등록하고 조회할 수 있음**  
📌 **ApplicationContext를 사용하면 자동으로 BeanDefinition을 읽어와 빈을 생성함**

### **(2) BeanDefinition 등록 과정**
1. **스프링이 설정 파일(XML, Java Config, Annotation)을 읽음**
2. **각 빈을 `BeanDefinition` 형태로 저장** (`BeanDefinitionRegistry` 사용)
3. **필요할 때 `BeanFactory`가 `BeanDefinition`을 참고하여 빈을 생성**
4. **의존성 주입 수행 후, 초기화 메서드 실행**

---

## **3. BeanDefinition 활용 예제**
스프링에서 `BeanDefinition`을 직접 조작할 일은 거의 없지만, **동적으로 빈을 등록할 때 활용**할 수 있다.

### **(1) BeanDefinition 수동 등록**
```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();

// BeanDefinition 생성
BeanDefinition beanDefinition = BeanDefinitionBuilder
    .genericBeanDefinition(MyService.class)
    .setScope(BeanDefinition.SCOPE_SINGLETON)
    .getBeanDefinition();

// BeanDefinitionRegistry에 등록
context.registerBeanDefinition("myService", beanDefinition);

// 컨테이너 초기화
context.refresh();

// 빈 사용
MyService myService = context.getBean(MyService.class);
```
✔ **이렇게 동적으로 `BeanDefinition`을 생성하고 등록할 수 있다.**

---

### **4. BeanDefinition이 중요한 이유**
1. **Spring 컨테이너가 빈을 관리하는 핵심 원리**
    - XML, Java Config, Annotation 기반 설정이 모두 `BeanDefinition`으로 변환되어 관리됨

2. **동적 빈 등록이 필요할 때 활용**
    - 런타임에 새로운 빈을 추가하거나 변경 가능 (`context.registerBeanDefinition()`)

3. **Spring 내부 동작을 이해하는 데 필수**
    - `@ComponentScan`, `@Bean`, `@Service` 등의 어노테이션도 결국 `BeanDefinition`으로 변환됨

📌 **결론적으로, `BeanDefinition`은 스프링 프레임워크가 빈을 정의하고 관리하는 핵심적인 메타데이터 구조이다!**