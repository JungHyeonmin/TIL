## **1. 제어의 역전(IoC)란**

제어의 역전(IoC)이란, 소프트웨어 설계 원칙 중 하나로 프로그래밍에 있어 객체의 생성 및 관리 책임을 개발자에서 전체 애플리케이션 또는 프레임워크에 위임하는 디자인 원칙을 일컫는다.
프레임워크 없이 개발을 진행할 때는 개발자가 객체의 생성 및 관리 등의 흐름을 직접 제어한다. 객체의 생성, 설정, 초기화, 메소드 호출, 소멸을 프로그래머가 직접 제어하고 관리하는 것이다. 그러나 IoC를
적용하면 이러한 "제어의 흐름"을 "역전"시켜 객체의 생성 및 관리를 외부 컨테이너 또는 프레임워크가 담당하도록 한다. "제어의 역전(IOC)"이 라는 말 뜻 그대로 제어에 대한 권한이 프로그래머에서 외부 환경으로
역전 되는 것이라고 할 수 있다. loC의 한 형태 로 의존성 주입(Dl; Dependency Injection)을 들 수 있는데 이는 객체가 필요로 하는 의존성을 외부에서 주입받는 방식을 사용한다. 객체는 자신의
의존성을 직접 생성하거나 관리하지 않고 외부에서 해당 의존성을 제공 받는 것이다.

### loC는 "어떻게" 가 아니라 "누가"
loC의 핵심 아이디어는 "어떻게"가 아니라 누가" 객체의 생성 및 관리 책임을 가지느냐에 있다. 즉, 우리는 어떻게 객 체들이 생성되고 관리되는지 보다 누구에 의해서 생성되고 관리되는지에 초점을 맞출 필요가 있다.
앞에서도 언급했 지만, loC는 객체의 생명 주기를 모두 프레임워크에 위임하는 것이라고 생각하면 된다.
전통적인 프로그래밍(프레임워크 없이 개발하는 방식)에서 제어의 주체에 대해 얘기해보면 다음과 같다.

1. 개발자(또는 특정 코드)가 직접 객체를 생성한다.
2. 해당 객체의 생명 주기 및 다른 관련 동작들을 명시적으로 제어한다.
3. 객체 간의 관계나 의존성은 코드 내에 직접 정의되어 있다. (코드의 구체적인 부분에서 명시적으로 표현)
   하지만 loC, 제어의 역전을 도입하는 순간 다음과 같이 변화한다.
1. 프레임워크가 객체의 생성 및 생명 주기를 관리한다.
2. 개발자는 단지 필요한 인터페이스나 규약을 따르며 그것을 구현한다.
3. 객체 간의 의존성은 외부에서 정의되며 프레임워크가 이러한 의존성을 주입하여 연결한다.
   개발자가 직접 객체의 생성, 생명 주기 및 제어 흐름을 관리하던 것을 IoC를 통해 프레임워크가 그 역할을 대신 수행 하게 된다. 즉, 프로그램의 제어 흐름이 개발자가 아닌 프레임워크에 의해 결정되고 관리되는
   것이다. 이것을 우리는
   "loC, 제어의 역전"이라고 한다. 그리고 이런 loC 원칙을 실현하기 위한 여러 디자인 패턴 중 하나가 바로 "Dl, 의존성 주입"이다.


```
제어의 역전이라는 건, 간단히 프로그램의 제어 흐름 구조가 뒤바뀌는 것이라고 설명할 수 있다. 일반적으로 프로그램의 흐름은 main() 메소드와 같이 프로그램이 시작되는 지점에서 다음에 사용할 오브젝트를 결정하고,
결정한 오브젝트를 생성하고, 만들어진 오브젝트에 있는 메소드를 호출하고, 그 오브젝트 메소드 안에서 다음에 사용할 것을 결정하고 호출하는 식의 작업이 반복된다. 이 런 프로그램 구조에서 각 오브젝트는 프로그램 흐름을
결정하거나 사용할 오브젝트를 구성하는 작업에 능동적으로 참여한다.
(중략)
제어의 역전이란 이런 제어 흐름의 개념을 꺼꾸로 뒤집는 것이다. 제어의 역전에서는 오브젝트가 자신이 사용할 오브젝트를 스스로 선택하지 않는다. 당연히 생성하지도 않는다. 또 자신도 어떻게 만들어지고 어디서
사용되는지를 알 수 없다. 모든 제어 권한을 자 신이 아닌 다른 대상에게 위임하기 때문이다. 프로그램의 시작을 담당하는 main() 과 같은 엔트리 포인트를 제외하면 모든 오브젝 트는 이렇게 위임받은 제어 권한을
갖는 특별한 오브젝트에 의해 결정되고 만들어진다.
(중략)
프레임워크도 제어의 역전 개념이 적용된 대표적인 기술이다. 프레임워크는 라이브러리의 다른 이름이 아니다. 프레임워크는 단지 미리 만들어둔 반제품이나, 확장해서 사용할 수 있도록 준비된 추상 라이브러리의 집합이
아니다. 프레임워크가 어떤 것인지 이해 하려면 라이브러리와 프레임워크가 어떻게 다른지 알아야 한다. 라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름 을 직접 제어한다. 단지 동작하는 중에 필요한 기능이
있을 때 능동적으로 라이브러리를 사용할 뿐이다. 반면에 프레임워크는 거꾸 로 애플리케이션 코드가 프레임워크에 의해 사용된다. 보통 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 주 도하는 중에
개발자가 만든 애플리케이션 코드를 사용하도록 만드는 방식이다. 최근에는 툴킷, 엔진, 라이브러리 등도 유행을 따라 서 무작정 프레임워크라고 부르기도 하는데 이는 잘못된 것이다. 프레임워크에는 분명한 제어의 역전
개념이 적용되어 있어야 한 다. 애플리케이션 코드는 프레임워크가 짜놓은 틀에서 수동적으로 동작해야 한다.

- 토비의 스프링 3.11권. 92쪽. -
```

---

## **2. 제어의 역전(IoC)이 나오게 된 배경**

과거에는 객체를 생성하고 관리하는 책임이 애플리케이션 코드 내부에 직접 존재했다. 하지만 프로젝트가 커지고 복잡성이 증가하면서, **객체 간의 강한 결합(tight coupling)** 문제가 발생했다. 이로 인해
코드의 재사용성과 유지보수성이 낮아졌고, 이를 해결하기 위해 **제어의 역전(IoC)** 개념이 등장하게 되었다.

### **1) 전통적인 프로그래밍 방식의 문제점**

기존의 객체 지향 프로그래밍에서는 클래스 간의 의존성을 직접 코드에서 정의하는 방식이 일반적이었다.

#### **(1) 직접 객체를 생성하는 방식**

```java
class Repository {
    public void getData() {
        System.out.println("데이터 가져오기");
    }
}

class Service {
    private Repository repository = new Repository(); // 직접 객체 생성

    public void execute() {
        repository.getData();
    }
}
```

📌 **문제점:**

- `Service` 클래스가 `Repository` 클래스와 강하게 결합되어 있음
- `Repository` 클래스가 변경되면 `Service`도 함께 수정해야 함 → **유지보수성 저하**
- `Repository`의 구현체가 여러 개일 경우(예: `MySQLRepository`, `OracleRepository`) 매번 `Service`에서 이를 수정해야 함 → **확장성 저하**

### **2) 디자인 패턴을 통한 문제 해결 시도**

객체 간의 강한 결합을 줄이기 위해 **팩토리 패턴(Factory Pattern)** 같은 기법이 사용되었다.

#### **(2) 팩토리 패턴 적용**

```java
class RepositoryFactory {
    public static Repository createRepository() {
        return new Repository(); // 변경 시 이 부분만 수정하면 됨
    }
}

class Service {
    private Repository repository = RepositoryFactory.createRepository();

    public void execute() {
        repository.getData();
    }
}
```

📌 **개선된 점:**

- `Service`에서 직접 `Repository`를 생성하지 않음
- `RepositoryFactory`를 통해 객체를 생성하므로, `Repository`의 변경이 필요하면 Factory에서만 수정하면 됨

📌 **하지만 여전히 문제:**

- `Service`는 여전히 `RepositoryFactory`에 의존하고 있음
- `RepositoryFactory`가 복잡해질수록 유지보수하기 어려움

---

## **3. IoC의 등장: 객체의 생성을 외부에서 관리**

IoC는 **객체의 생성 및 관리를 개발자가 아닌 외부 컨테이너(프레임워크)가 담당**하도록 하는 개념이다. 이를 통해 코드의 결합도를 낮추고 유지보수성과 확장성을 높일 수 있다.

### **1) IoC 적용 전후 비교**

| 구분         | IoC 적용 전               | IoC 적용 후                |
|------------|------------------------|-------------------------|
| **객체 생성**  | 개발자가 직접 객체 생성          | 프레임워크(Spring 등)가 객체 생성  |
| **의존성 관리** | 클래스 내부에서 직접 의존성 할당     | 외부에서 의존성을 주입            |
| **결합도**    | 강한 결합(Tightly Coupled) | 느슨한 결합(Loosely Coupled) |

---

## **4. IoC를 실현하는 방식: 의존성 주입(DI, Dependency Injection)**

IoC의 대표적인 구현 방식이 **의존성 주입(DI, Dependency Injection)** 이다. DI를 사용하면 **객체가 직접 의존성을 생성하는 것이 아니라, 외부에서 필요한 의존성을 주입받도록 설계할 수
있다.**

### **(1) 생성자 주입 방식 (Constructor Injection)**

```java
class Service {
    private Repository repository;

    // 생성자를 통해 의존성을 외부에서 주입받음
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void execute() {
        repository.getData();
    }
}

// 외부에서 객체를 생성하고 주입
public class Main {
    public static void main(String[] args) {
        Repository repository = new Repository();
        Service service = new Service(repository); // 객체 주입
        service.execute();
    }
}
```

✅ **개선된 점:**

- `Service`가 `Repository`를 직접 생성하지 않음
- `Repository`의 구현체를 쉽게 변경할 수 있음
- **테스트 코드 작성이 용이함** (Mock 객체를 쉽게 주입 가능)

---

## **5. IoC 컨테이너의 등장 (Spring 프레임워크 활용)**

스프링 프레임워크에서는 IoC 컨테이너가 객체의 생성을 관리하고 DI를 수행한다.

### **(1) 스프링에서 IoC 컨테이너를 활용한 DI**

```java

@Component // 스프링이 객체를 자동 관리
class Repository {
    public void getData() {
        System.out.println("데이터 가져오기");
    }
}

@Service
class Service {
    private final Repository repository;

    @Autowired // 스프링이 Repository를 자동 주입
    public Service(Repository repository) {
        this.repository = repository;
    }

    public void execute() {
        repository.getData();
    }
}

@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(Main.class, args);
        Service service = context.getBean(Service.class);
        service.execute();
    }
}
```

✅ **IoC 컨테이너(Spring)가 하는 역할:**

1. `@Component`, `@Service` 등의 애노테이션이 붙은 클래스를 자동으로 감지하여 객체 생성
2. `@Autowired`를 통해 필요한 객체를 자동으로 주입
3. 개발자는 객체 생성 및 관리를 신경 쓰지 않고 비즈니스 로직에 집중할 수 있음

---

## **6. IoC가 주는 이점**

### ✅ **1) 결합도 감소 (Loose Coupling)**

- IoC를 통해 객체 간의 의존성을 줄이고, 유지보수가 쉬워짐

### ✅ **2) 코드의 재사용성 증가**

- `Repository`의 구현체만 변경하면 여러 `Service`에서 쉽게 활용 가능

### ✅ **3) 테스트 용이성 증가**

- Mock 객체를 쉽게 주입할 수 있어 단위 테스트 작성이 쉬워짐

### ✅ **4) 유지보수 및 확장성 증가**

- 새로운 의존성이 필요해도 코드 변경 없이 설정만으로 적용 가능

---

## **7. 정리**

- **IoC(Inversion of Control)**: 객체의 생성 및 관리 책임을 개발자가 아닌 프레임워크(Spring 등)가 담당하는 원칙
- **IoC가 등장한 배경**: 객체 간 결합도를 줄이고 유지보수를 쉽게 하기 위해 도입됨
- **IoC를 실현하는 대표적인 방식 → DI (Dependency Injection)**
    - **생성자 주입(Constructor Injection)**
    - **Setter 주입(Setter Injection)**
    - **필드 주입(Field Injection, 비추천)**
- **스프링 IoC 컨테이너 사용 시 장점**
    - 개발자가 객체 생성을 신경 쓸 필요 없이 자동 관리됨
    - 코드의 재사용성과 유지보수성이 향상됨

---

🚀 **즉, IoC를 사용하면 개발자는 객체 생성 및 관리를 신경 쓰지 않고, 핵심 비즈니스 로직 개발에 집중할 수 있다!** 🚀