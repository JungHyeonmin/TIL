## **Java Reflection이란?**
Java Reflection(리플렉션)은 **런타임(Runtime)**에 클래스, 인터페이스, 메소드, 필드 등에 대한 정보를 얻거나 조작할 수 있도록 해주는 Java API이다. 이를 통해 컴파일 시점이 아니라 실행 중에 클래스의 구조를 동적으로 분석하고 수정할 수 있다.

---

## **1. 왜 Reflection을 사용할까?**
### **✅ 주요 사용 목적**
1. **프레임워크 개발**
    - 스프링(Spring) 같은 프레임워크에서는 Bean을 동적으로 주입할 때 Reflection을 활용한다.
2. **의존성을 줄이기 위한 동적 로딩**
    - 특정 클래스의 존재 여부를 체크한 후 동적으로 객체를 생성하고 실행할 수 있다.
3. **어노테이션 기반 프로그래밍**
    - 리플렉션을 이용하여 어노테이션을 분석하고, 실행 시 특정 동작을 수행할 수 있다.
4. **디버깅 및 테스트**
    - Private 필드나 메소드를 강제로 호출하여 테스트할 수 있다.

---

## **2. Reflection의 주요 기능**
Java의 `java.lang.reflect` 패키지를 이용하여 다양한 Reflection 기능을 수행할 수 있다.

### **1️⃣ 클래스 정보 얻기**
```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

class Person {
    private String name;
    public int age;
    
    public Person() {}
    public Person(String name) { this.name = name; }
    
    private void sayHello() {
        System.out.println("Hello, my name is " + name);
    }
}

public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        // 클래스 정보를 가져오는 방법
        Class<?> clazz = Person.class; // 방법 1: 클래스 타입 직접 사용
        Class<?> clazz2 = Class.forName("Person"); // 방법 2: 문자열을 이용하여 로드
        
        System.out.println("클래스 이름: " + clazz.getName());

        // 필드 정보 출력
        Field[] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("필드 이름: " + field.getName() + ", 타입: " + field.getType());
        }

        // 생성자 정보 출력
        Constructor<?>[] constructors = clazz.getDeclaredConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("생성자: " + constructor);
        }

        // 메소드 정보 출력
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("메소드 이름: " + method.getName());
        }
    }
}
```
**🔹 실행 결과**
```
클래스 이름: Person
필드 이름: name, 타입: class java.lang.String
필드 이름: age, 타입: int
생성자: public Person()
생성자: public Person(java.lang.String)
메소드 이름: sayHello
```

---

### **2️⃣ 필드 값 변경하기**
```java
Field field = clazz.getDeclaredField("name");
field.setAccessible(true); // private 필드 접근 가능하게 설정

Person person = new Person();
field.set(person, "John"); // 필드 값 변경

System.out.println(field.get(person)); // John 출력
```

---

### **3️⃣ 메소드 실행하기**
```java
Method method = clazz.getDeclaredMethod("sayHello");
method.setAccessible(true); // private 메소드 접근 가능하게 설정

Person person = new Person("Alice");
method.invoke(person); // "Hello, my name is Alice" 출력
```

---

### **4️⃣ 객체 동적 생성**
```java
Constructor<?> constructor = clazz.getDeclaredConstructor(String.class);
Person person = (Person) constructor.newInstance("Bob");

System.out.println("생성된 객체의 이름: " + field.get(person)); // Bob 출력
```

---

## **3. Reflection의 단점**
### **🚨 성능 저하**
Reflection은 일반적인 코드보다 **느리다.**
- 이유: JVM이 최적화할 수 없고, 보안 검사와 추가적인 검증이 필요하기 때문.

### **🚨 보안 문제**
- private 필드나 메서드까지 접근이 가능하기 때문에 보안적으로 위험할 수 있음.
- **SecurityManager**를 통해 제한 가능.

### **🚨 유지보수 어려움**
- 컴파일 시점이 아닌 런타임에서 오류가 발생하기 때문에, 디버깅이 어렵다.
- 리팩토링 시에도 문제가 생길 가능성이 높음.

---

## **4. Reflection이 자주 사용되는 곳**
| 사용처 | 설명 |
|------|------|
| **Spring Framework** | 의존성 주입(DI), AOP 등에서 빈(Bean) 생성을 위해 사용 |
| **JUnit** | 테스트 프레임워크에서 private 메서드를 호출할 때 사용 |
| **ORM (Hibernate, JPA)** | 엔티티의 필드 정보를 가져와 SQL을 동적으로 생성 |
| **JSON 파싱 (Jackson, Gson)** | 클래스 필드를 기반으로 JSON 데이터를 매핑 |

---

## **5. Reflection을 대체할 방법**
성능 저하와 유지보수 문제 때문에 **다음과 같은 방법**을 고려할 수 있다.

1. **어노테이션 프로세싱**
    - 컴파일 타임에 코드 자동 생성 (`@AnnotationProcessor`)
2. **런타임 다이나믹 프록시**
    - `java.lang.reflect.Proxy`를 사용하여 인터페이스 기반 프록시 활용
3. **MethodHandle (Java 7+)**
    - `java.lang.invoke.MethodHandles`를 이용하여 보다 빠르게 메소드 호출 가능

---

## **📌 정리**
| 장점 | 단점 |
|------|------|
| 동적 객체 생성 가능 | 성능 저하 |
| private 메서드, 필드 접근 가능 | 보안 문제 발생 가능 |
| 프레임워크 개발에 필수적 | 유지보수 어려움 |

Reflection은 **프레임워크**나 **라이브러리 개발**에서 필수적인 기능이지만,  
일반적인 애플리케이션 개발에서는 **남용하면 성능과 보안에 문제가 생길 수 있다.**  
따라서 꼭 필요한 경우에만 사용하고, 대체 방법이 있다면 고려하는 것이 좋다! 🚀