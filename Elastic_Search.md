## **Elasticsearch란?**
Elasticsearch는 분산형 오픈소스 검색 및 분석 엔진으로, **Apache Lucene**을 기반으로 구축되었습니다. JSON 문서를 기반으로 데이터를 저장하며, 대량의 데이터를 빠르게 검색하고 분석할 수 있도록 설계되었습니다. 주로 **검색 엔진, 로깅 및 모니터링, 데이터 분석** 등에 활용됩니다.

---

## **1. Elasticsearch의 핵심 개념**
### **(1) Index (인덱스)**
- 데이터를 저장하는 **논리적인 단위**로, 관계형 데이터베이스(RDB)의 **데이터베이스(Database)**와 유사합니다.
- 하나의 인덱스는 여러 개의 문서(Document)를 포함합니다.

### **(2) Document (문서)**
- Elasticsearch에서 데이터를 저장하는 **기본 단위**입니다.
- **JSON 형식**으로 저장되며, RDB에서의 **레코드(Row)**와 비슷한 개념입니다.

### **(3) Shard (샤드)**
- 인덱스를 여러 개의 부분으로 나눈 조각입니다.
- 대용량 데이터를 분산 저장하고 병렬 처리를 통해 **빠른 검색 성능을 제공**합니다.

### **(4) Replica (복제본)**
- 샤드의 복사본으로, **데이터 안정성 및 성능 향상**을 위해 사용됩니다.
- 하나의 샤드가 손상되더라도 복제본이 있기 때문에 **고가용성(High Availability)**을 보장합니다.

---

## **2. Elasticsearch의 주요 특징**
### **(1) 분산형 아키텍처**
- 여러 개의 서버(Node)로 이루어진 **클러스터(Cluster)**에서 동작합니다.
- **샤드(Shard)와 레플리카(Replica)를 통해 데이터가 여러 노드에 분산 저장**되므로 **확장성(Scalability)과 안정성(Reliability)**이 뛰어납니다.

### **(2) JSON 기반 문서 저장**
- Elasticsearch는 NoSQL 데이터베이스처럼 **JSON 형식의 문서를 저장하고 검색**합니다.
- SQL처럼 관계형 데이터베이스 스키마를 강제하지 않으며, **유연한 데이터 모델**을 제공합니다.

### **(3) 강력한 검색 기능**
- **역색인(Inverted Index)**을 활용한 검색 최적화로 매우 빠른 검색 성능을 보장합니다.
- **풀 텍스트 검색(Full-Text Search)** 기능을 제공하여 텍스트 데이터를 효율적으로 검색할 수 있습니다.

### **(4) 실시간 데이터 처리**
- Elasticsearch는 데이터가 입력되면 **즉시 검색 가능**한 상태가 됩니다.
- 이는 실시간 로그 분석, 모니터링 시스템 등에 유용하게 사용됩니다.

### **(5) Kibana와 연동 가능**
- Elasticsearch의 시각화 도구인 **Kibana**를 사용하면 데이터를 그래프, 차트, 대시보드 등의 형태로 쉽게 분석할 수 있습니다.

---

## **3. Elasticsearch의 활용 사례**
### **(1) 검색 엔진**
- 전자상거래(예: **Amazon, eBay**)에서 상품 검색 기능 구현
- 뉴스 포털(예: **The Guardian**)에서 기사 검색

### **(2) 로그 및 모니터링 시스템**
- **ELK Stack(Elasticsearch + Logstash + Kibana)**을 활용한 서버 및 애플리케이션 로그 분석
- **APM(Application Performance Monitoring)**을 통한 서비스 성능 분석

### **(3) 데이터 분석**
- 대량의 데이터를 분석하고 시각화하여 의사결정 지원 시스템 구축
- 머신러닝(ML) 및 AI 모델의 데이터 처리 백엔드로 활용

### **(4) 추천 시스템**
- 사용자 검색 이력을 기반으로 맞춤형 콘텐츠 추천
- 영화, 음악, 뉴스 추천 시스템에 활용

---

## **4. Elasticsearch의 데이터 저장 및 검색 방식**
### **(1) 데이터 저장 (Indexing)**
Elasticsearch는 JSON 데이터를 저장하는데, 다음과 같은 방식으로 문서를 저장할 수 있습니다.

#### **예제: 사용자 정보 저장**
```json
PUT my_index/_doc/1
{
  "name": "John Doe",
  "age": 30,
  "email": "johndoe@example.com",
  "skills": ["Java", "Elasticsearch", "Spring"]
}
```
위의 요청은 `my_index`라는 인덱스에 ID가 `1`인 문서를 저장하는 것입니다.

---

### **(2) 데이터 검색 (Querying)**
Elasticsearch는 강력한 검색 기능을 제공하며, 기본적으로 `Match Query`, `Term Query` 등을 사용할 수 있습니다.

#### **예제: 이름이 "John"인 사용자 검색**
```json
GET my_index/_search
{
  "query": {
    "match": {
      "name": "John"
    }
  }
}
```
위의 요청은 `my_index`에서 `name` 필드에 "John"이 포함된 문서를 검색하는 것입니다.

---

### **(3) 필터링 (Filtering)**
검색 시 특정 조건을 추가하여 데이터를 필터링할 수도 있습니다.

#### **예제: 25세 이상인 사용자 검색**
```json
GET my_index/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 25
      }
    }
  }
}
```
위의 요청은 `age` 필드가 **25 이상**인 문서를 검색합니다.

---

## **5. Elasticsearch가 등장하기 전에는?**
Elasticsearch가 등장하기 전에는 대량의 데이터를 검색하고 분석하는 데 다음과 같은 방법이 사용되었습니다.

### **(1) 관계형 데이터베이스(RDB)**
- MySQL, PostgreSQL 등에서 `LIKE` 연산자를 사용하여 검색
- 대용량 데이터 검색 시 속도가 매우 느리고 성능이 저하됨

### **(2) Apache Solr**
- Elasticsearch 이전의 대표적인 검색 엔진
- **Lucene 기반**이지만 확장성이 부족하고 설정이 복잡함

Elasticsearch는 **분산 처리, JSON 문서 기반 저장, 강력한 검색 성능**을 제공하여 기존 방법의 한계를 극복하였습니다.

---

## **6. Elasticsearch의 발전 과정**
1. **2010년:** Elasticsearch 첫 번째 버전 출시
2. **2015년:** Elasticsearch 2.0 출시 (안정성 및 성능 개선)
3. **2017년:** Elasticsearch 5.x → 6.x로 발전 (샤드 관리 최적화)
4. **2019년:** Elasticsearch 7.x 출시 (쿼리 속도 및 집계 성능 개선)
5. **2023년:** 최신 버전 8.x 출시 (머신러닝 기능 강화, 보안 기능 향상)

---

## **7. Elasticsearch를 활용한 프로젝트 예제**
### **(1) Spring Boot와 Elasticsearch 연동**
Elasticsearch를 백엔드 애플리케이션에서 사용하려면 Spring Data Elasticsearch를 활용할 수 있습니다.

#### **예제: Spring Boot에서 Elasticsearch 저장 및 검색**
```java
@Document(indexName = "users")
public class User {
    @Id
    private String id;
    private String name;
    private int age;
    private List<String> skills;
}
```