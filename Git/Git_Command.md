## **1. Git 설정 관련 명령어**
| 명령어 | 설명 |
|--------|------|
| `git config --global user.name "사용자이름"` | Git 사용자 이름 설정 |
| `git config --global user.email "이메일주소"` | Git 사용자 이메일 설정 |
| `git config --list` | 설정된 Git 환경 변수 확인 |
| `git config --global core.editor "vim"` | 기본 텍스트 편집기 변경 |
| `git config --global merge.tool "vimdiff"` | 기본 병합 도구 설정 |
| `git config --global alias.<별칭> "<명령어>"` | Git 명령어 별칭 생성 (`git config --global alias.co checkout` → `git co` 사용 가능) |

---

## **2. Git 저장소 초기화 및 클론**
| 명령어 | 설명 |
|--------|------|
| `git init` | 현재 디렉터리에 새로운 Git 저장소 초기화 |
| `git clone <원격저장소URL>` | 원격 저장소 복제 |
| `git clone --depth=1 <원격저장소URL>` | 최근 커밋만 가져와 저장소를 가볍게 복제 |

---

## **3. 스테이징 및 커밋**
| 명령어 | 설명 |
|--------|------|
| `git add <파일명>` | 특정 파일을 스테이징 |
| `git add .` | 모든 변경 사항을 스테이징 |
| `git commit -m "커밋 메시지"` | 스테이징된 변경 사항을 커밋 |
| `git commit --amend -m "새 메시지"` | 마지막 커밋 메시지 수정 |
| `git commit --amend --no-edit` | 마지막 커밋 내용 수정 없이 그대로 유지 |

---

## **4. 브랜치 관련 명령어**
| 명령어 | 설명 |
|--------|------|
| `git branch` | 현재 존재하는 브랜치 목록 출력 |
| `git branch <브랜치명>` | 새로운 브랜치 생성 |
| `git checkout <브랜치명>` | 특정 브랜치로 이동 |
| `git checkout -b <브랜치명>` | 새 브랜치를 만들고 이동 |
| `git merge <브랜치명>` | 현재 브랜치에서 특정 브랜치를 병합 |
| `git branch -d <브랜치명>` | 로컬 브랜치 삭제 |
| `git branch -D <브랜치명>` | 강제로 브랜치 삭제 |
| `git switch <브랜치명>` | 최신 Git 버전에서 브랜치 이동 (checkout 대체) |
| `git switch -c <브랜치명>` | 최신 Git에서 새로운 브랜치 생성 및 이동 |

---

## **5. 원격 저장소 관련 명령어**
| 명령어 | 설명 |
|--------|------|
| `git remote -v` | 원격 저장소 목록 확인 |
| `git remote add origin <URL>` | 원격 저장소 추가 |
| `git push origin <브랜치명>` | 원격 저장소로 코드 푸시 |
| `git push -u origin <브랜치명>` | 기본 원격 브랜치 설정 후 푸시 |
| `git pull origin <브랜치명>` | 원격 저장소에서 최신 코드 가져오기 |
| `git fetch` | 원격 저장소의 변경 사항 가져오기 |
| `git remote rm origin` | 원격 저장소 삭제 |

---

## **6. 되돌리기 및 리셋**
| 명령어 | 설명 |
|--------|------|
| `git checkout -- <파일명>` | 변경 사항을 마지막 커밋 상태로 되돌리기 |
| `git revert <커밋ID>` | 특정 커밋을 되돌리고 새로운 커밋 생성 |
| `git reset --soft <커밋ID>` | 해당 커밋까지 되돌리되, 변경 사항은 유지 |
| `git reset --mixed <커밋ID>` | 해당 커밋까지 되돌리며, 스테이징 영역 초기화 |
| `git reset --hard <커밋ID>` | 해당 커밋까지 강제 리셋 (변경 사항 삭제) |

---

## **7. Git Stash (임시 저장)**
| 명령어 | 설명 |
|--------|------|
| `git stash` | 현재 변경 사항을 임시 저장 |
| `git stash list` | 저장된 stash 목록 확인 |
| `git stash pop` | 가장 최근 stash 적용 후 삭제 |
| `git stash apply` | 가장 최근 stash 적용 (삭제 안 함) |
| `git stash drop` | 가장 최근 stash 삭제 |
| `git stash clear` | 모든 stash 삭제 |

---

## **8. 태그(Tag) 관리**
| 명령어 | 설명 |
|--------|------|
| `git tag` | 태그 목록 확인 |
| `git tag <태그명>` | 태그 생성 |
| `git tag -a <태그명> -m "태그 설명"` | 주석이 포함된 태그 생성 |
| `git tag -d <태그명>` | 태그 삭제 |
| `git push origin <태그명>` | 특정 태그 푸시 |
| `git push origin --tags` | 모든 태그 푸시 |

---

## **9. Git Log (기록 확인)**
| 명령어 | 설명 |
|--------|------|
| `git log` | 커밋 로그 확인 |
| `git log --oneline` | 한 줄로 요약된 커밋 로그 확인 |
| `git log --graph --oneline --all` | 브랜치 구조를 그래프로 확인 |
| `git reflog` | HEAD 이동 내역 확인 |

---

## **10. Git Blame & Diff (변경 사항 추적)**
| 명령어 | 설명 |
|--------|------|
| `git blame <파일명>` | 파일의 각 줄을 수정한 커밋 및 사용자 확인 |
| `git diff` | 변경 사항 비교 |
| `git diff --staged` | 스테이징된 변경 사항 비교 |
| `git diff <브랜치명>..<브랜치명>` | 두 브랜치 간 변경 사항 비교 |

---
좋아! Git의 **rebase** 관련 명령어도 추가해 줄게.

---

## **11. Git Rebase (리베이스)**
**Rebase**는 브랜치를 다른 브랜치의 최신 커밋 위로 재정렬하는 명령어야.  
이걸 사용하면 브랜치 히스토리를 더 깔끔하게 정리할 수 있어.

| 명령어 | 설명 |
|--------|------|
| `git rebase <브랜치명>` | 현재 브랜치를 지정한 브랜치의 최신 커밋 위로 이동 |
| `git rebase -i <커밋ID>` | **인터랙티브 모드**로 지정한 커밋 이후의 커밋들을 수정 |
| `git rebase --abort` | 진행 중인 리베이스를 중단하고 원래 상태로 되돌림 |
| `git rebase --continue` | 충돌 해결 후 리베이스 계속 진행 |
| `git rebase --skip` | 충돌이 발생한 커밋을 건너뛰고 리베이스 진행 |

---

### **📌 Git Rebase vs Merge 차이**
| | Rebase | Merge |
|------|--------|------|
| **히스토리** | 깔끔한 직선형 히스토리 유지 | 병합 커밋이 남아 히스토리가 복잡해질 수 있음 |
| **충돌 처리** | 하나씩 순차적으로 처리 | 병합 시 한 번에 처리 |
| **사용 목적** | 히스토리를 정리하고 싶은 경우 | 브랜치를 병합하고 협업할 때 |
