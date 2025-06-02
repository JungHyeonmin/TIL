## ✅ moment.js

### 1. 개념

* moment.js는 JavaScript의 날짜 및 시간 처리를 쉽게 하기 위한 라이브러리다.
* 날짜 포맷팅, 파싱, 연산, 비교, 타임존 처리 기능을 제공한다.
* JavaScript의 Date 객체의 불편함을 해소하기 위해 등장했다.

---

### 2. 사용하는 이유

* Date 객체는 월이 0부터 시작하는 등 직관적이지 않다.
* 시간 덧셈/뺄셈은 ms 단위로 계산해야 해서 번거롭다.
* 다양한 포맷 출력을 수동으로 처리해야 한다.
* moment.js는 이 모든 작업을 간단한 API로 처리한다.

---

### 3. 설치 방법

```bash
npm install moment
```

또는 CDN으로 포함한다.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.29.4/moment.min.js"></script>
```

---

### 4. 주요 기능 사용법

```javascript
const moment = require('moment'); // Node.js 기준

// 현재 시간 가져오기
moment();  

// 포맷 지정 출력
moment().format("YYYY-MM-DD HH:mm:ss");  

// 문자열 파싱
moment("2025-12-25", "YYYY-MM-DD");

// 날짜 덧셈
moment().add(3, 'days');

// 날짜 뺄셈
moment().subtract(1, 'month');

// 날짜 비교
moment("2025-06-01").isBefore("2025-06-02");
```

---

### 5. 자주 사용하는 포맷

| 형식     | 설명       | 예시   |
| ------ | -------- | ---- |
| `YYYY` | 연도       | 2025 |
| `MM`   | 월 (2자리)  | 06   |
| `DD`   | 일 (2자리)  | 02   |
| `HH`   | 시 (24시간) | 13   |
| `mm`   | 분        | 45   |
| `ss`   | 초        | 07   |

---

### 6. 많이 사용하는 곳

* 프론트엔드에서 날짜 입력/출력 시
* 백엔드 로그 생성 시 타임스탬프 포맷 지정
* 예약 시스템, 마감일 계산 기능
* DB 조회 조건에서 날짜 계산

---

### 7. 발전 과정

* moment.js는 2011년에 등장했다.
* 개발 당시에는 Date 객체가 매우 불편했기 때문에 큰 인기를 얻었다.
* 시간이 지나며 다음과 같은 단점이 드러났다:

    * 파일 크기가 크다.
    * immutable이 아니다.
    * 모듈화가 부족하다.
* 현재는 유지보수만 하고 있다.
* 공식 문서에서도 신규 프로젝트에는 다른 라이브러리 사용을 권장한다.

---

### 8. moment.js가 없을 때 처리 방식

```javascript
const date = new Date();
const year = date.getFullYear();
const month = date.getMonth() + 1; // 0부터 시작하므로 +1
const day = date.getDate();
const hours = date.getHours();
const minutes = date.getMinutes();
const seconds = date.getSeconds();
console.log(`${year}-${month}-${day} ${hours}:${minutes}:${seconds}`);
```

* 날짜 연산은 `getTime()`으로 밀리초를 직접 계산해야 한다.
* 포맷 출력도 수작업으로 처리해야 한다.
* 복잡한 연산이나 타임존 처리는 직접 구현해야 했다.

---

### 9. moment.js 대체 라이브러리

| 라이브러리        | 특징                                         |
| ------------ | ------------------------------------------ |
| **dayjs**    | moment와 API 유사, 매우 가볍다 (2KB), immutable 지원 |
| **date-fns** | 함수 단위 모듈화, 트리 쉐이킹에 강함                      |
| **luxon**    | moment 개발진이 만든 후속작, 국제화 및 타임존 강력           |

---

### 10. 예시: dayjs 대체 코드

```javascript
import dayjs from 'dayjs';

dayjs().format("YYYY-MM-DD HH:mm:ss"); // moment와 동일한 문법
```
