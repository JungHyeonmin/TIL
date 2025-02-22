## GitHub Actions란?
GitHub Actions는 CI/CD(Continuous Integration/Continuous Deployment) 파이프라인을 자동화할 수 있는 GitHub의 기능입니다. 코드를 푸시하거나 PR을 생성할 때 테스트, 빌드, 배포 등의 작업을 자동으로 수행할 수 있도록 설정할 수 있습니다.

---

## 🔹 GitHub Actions의 구조

GitHub Actions의 설정은 `.github/workflows/` 디렉터리에 YAML 파일로 정의되며, 핵심 구성 요소는 다음과 같습니다.

### 1️⃣ **Workflow (워크플로우)**
워크플로우는 GitHub Actions에서 실행되는 자동화된 프로세스입니다.
- 하나 이상의 **Job(작업)**을 포함합니다.
- 특정 이벤트(코드 푸시, PR 생성, 일정 등)에 의해 실행됩니다.

**예시**
```yaml
name: CI Pipeline  # 워크플로우 이름

on: 
  push:
    branches: [main]  # main 브랜치로 push 될 때 실행
  pull_request:
    branches: [main]  # main 브랜치로 PR이 생성될 때 실행

jobs:
  build:
    runs-on: ubuntu-latest  # 실행 환경 지정
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3  # 코드 체크아웃

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build with Gradle
        run: ./gradlew build  # Gradle 빌드 실행
```

---

### 2️⃣ **Event (이벤트)**
워크플로우를 트리거하는 조건입니다.
- `push`: 특정 브랜치에 푸시될 때 실행
- `pull_request`: PR이 생성되거나 업데이트될 때 실행
- `schedule`: 특정 시간마다 실행 (cron)
- `workflow_dispatch`: 수동 실행

```yaml
on:
  push:
    branches:
      - main
  schedule:
    - cron: '0 0 * * *'  # 매일 자정 실행
  workflow_dispatch:  # 수동 실행 가능
```

---

### 3️⃣ **Job (작업)**
하나의 워크플로우 안에서 실행되는 개별 작업 단위입니다.
- 여러 개의 Job을 가질 수 있으며, 병렬 혹은 순차 실행이 가능합니다.
- `needs` 키워드를 사용하여 실행 순서를 제어할 수 있습니다.

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Run Unit Tests
        run: ./gradlew test

  deploy:
    runs-on: ubuntu-latest
    needs: test  # test 작업이 끝난 후 실행됨
    steps:
      - name: Deploy Application
        run: echo "Deploying to production..."
```

---

### 4️⃣ **Runner (러너)**
GitHub Actions의 Job을 실행하는 환경입니다.
- **GitHub 호스팅 러너**: `ubuntu-latest`, `windows-latest`, `macos-latest`
- **Self-hosted 러너**: 자체 서버에 설치하여 실행 가능

```yaml
jobs:
  my-job:
    runs-on: ubuntu-latest
```

---

### 5️⃣ **Step (스텝)**
Job 내부에서 실행되는 개별 작업 단위입니다.
- `run` 키워드를 사용하여 직접 명령을 실행할 수 있습니다.
- `uses` 키워드를 사용하여 외부 액션을 실행할 수 있습니다.

```yaml
steps:
  - name: Run a shell command
    run: echo "Hello, GitHub Actions!"
```

---

### 6️⃣ **Actions (액션)**
GitHub Actions에서 미리 만들어진 재사용 가능한 스크립트입니다.
- 공식 및 커뮤니티 액션을 사용할 수 있습니다.
- `uses: actions/checkout@v3` → Git 저장소를 가져오는 액션
- `uses: actions/setup-java@v3` → Java 환경을 설정하는 액션

```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v3
  - name: Set up Node.js
    uses: actions/setup-node@v3
    with:
      node-version: '18'
```

---

## 🛠 **GitHub Actions의 활용 예시**
### ✅ **CI/CD 자동화**
- 코드 푸시 시 자동으로 빌드 및 테스트 실행
- PR이 생성되면 코드 리뷰 전에 자동 테스트 실행
- 성공하면 자동으로 배포

```yaml
name: Java CI/CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up JDK 17
        uses: actions/setup-java@v3
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Build project
        run: ./gradlew build

      - name: Run Tests
        run: ./gradlew test

  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to Server
        run: echo "Deploying to production..."
```

---

## 🚀 **정리**
| 구성 요소 | 설명 | 예제 |
|-----------|-------------------|----------------------|
| Workflow | 자동화 프로세스 | `.github/workflows/*.yaml` |
| Event | 실행 조건 | `push`, `pull_request`, `schedule` |
| Job | 개별 실행 단위 | `jobs: build:`, `jobs: deploy:` |
| Runner | 실행 환경 | `runs-on: ubuntu-latest` |
| Step | Job 내 작업 | `run: echo "Hello"` |
| Action | 재사용 가능한 스크립트 | `uses: actions/setup-java@v3` |

GitHub Actions를 사용하면 CI/CD 파이프라인을 쉽게 설정할 수 있으며, 백엔드 개발에서 테스트 자동화, 코드 품질 유지, 배포 자동화 등에 활용할 수 있습니다. 🚀