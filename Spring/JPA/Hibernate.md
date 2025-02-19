# Hibernate란?

## 1. Hibernate 개요
Hibernate는 **자바 기반의 ORM(Object-Relational Mapping) 프레임워크**로, 관계형 데이터베이스를 객체지향적으로 다룰 수 있도록 도와줍니다.

## 2. Hibernate의 주요 특징
- **ORM(Object-Relational Mapping)**: 객체와 데이터베이스 테이블을 매핑하여 SQL을 직접 작성하지 않아도 데이터 조작 가능
- **자동화된 SQL 생성 및 실행**: 개발자가 SQL을 작성하지 않아도 데이터베이스에 대한 CRUD 연산 수행 가능
- **DB 독립성 보장**: 특정 DBMS에 의존적이지 않고 다양한 데이터베이스를 지원
- **Lazy Loading 지원**: 필요한 시점에 데이터를 가져오는 방식으로 성능 최적화 가능
- **트랜잭션 관리**: Hibernate 자체적으로 트랜잭션을 관리하여 안정적인 데이터 조작 가능

## 3. Hibernate와 JPA의 관계
- Hibernate는 **JPA(Java Persistence API)의 구현체 중 하나**
- JPA는 인터페이스이고, Hibernate는 이를 구현한 프레임워크
- JPA 표준을 따르면서 Hibernate만의 추가 기능도 제공

## 4. Hibernate의 주요 기능
### 4.1 엔티티 매핑(Entity Mapping)
- `@Entity`, `@Table`, `@Id`, `@Column` 등의 애너테이션을 사용하여 테이블과 클래스 매핑
- 객체를 데이터베이스 테이블과 연결하여 쉽게 데이터 조작 가능

### 4.2 CRUD 자동화
- `save()`, `find()`, `update()`, `delete()` 등의 메서드를 통해 기본적인 데이터 조작 가능

### 4.3 HQL (Hibernate Query Language)
- SQL과 유사하지만, 엔티티 객체를 대상으로 쿼리 작성 가능
- 특정 DBMS에 종속되지 않는 장점

### 4.4 캐싱 (1차 캐시, 2차 캐시)
- **1차 캐시**: 같은 트랜잭션 내에서 동일한 엔티티 조회 시 캐싱
- **2차 캐시**: 애플리케이션 레벨에서 캐싱하여 성능 최적화

### 4.5 Lazy & Eager Loading
- **Lazy Loading**: 실제 사용할 때 데이터를 가져와 불필요한 쿼리 최소화
- **Eager Loading**: 연관된 데이터를 한 번에 가져와 성능 최적화

## 5. Hibernate 설정 예제 (Spring Boot 기반)
### 5.1 build.gradle 설정
```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.hibernate:hibernate-core:5.6.15.Final'
    runtimeOnly 'org.postgresql:postgresql' // 사용하는 DB에 맞게 변경
}
```

### 5.2 application.yml 설정
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: myuser
    password: mypassword
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: true
```

### 5.3 엔티티(Entity) 클래스 예제
```java
import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "users")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true)
    private String username;

    @Column(nullable = false)
    private String password;
}
```

### 5.4 Repository 사용 예제
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```

## 6. 결론
Hibernate는 **데이터베이스와 객체 간의 매핑을 쉽게 해주는 ORM 프레임워크**로, JPA의 구현체 중 하나입니다. SQL을 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있으며, 다양한 성능 최적화 기능을 제공합니다.

### 📌 Hibernate를 사용하면?
- 반복적인 SQL 작성이 줄어들고 유지보수성이 향상됨
- DBMS에 종속적이지 않으므로 이식성이 뛰어남
- 캐싱, Lazy Loading 등을 통해 성능 최적화 가능