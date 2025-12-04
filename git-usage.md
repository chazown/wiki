# 📥 Git Master Guide

## 0. ⚙️ 초기 환경 설정 (Config)
새 컴퓨터 세팅 시 가장 먼저 수행합니다.

### 🔹 유저 정보 등록
```bash
# 본인 이름(ID) 등록
git config --global user.name "[사용자명]"

# 본인 이메일 등록 (GitHub 가입 이메일)
git config --global user.email "[이메일주소]"
```

### 🔹 필수 설정 (줄바꿈/대소문자 이슈 방지)
```bash
# 윈도우/맥 협업 시 줄바꿈 문자 자동 변환
git config --global core.autocrlf true

# 파일명 대소문자 구분 무시 방지 (false로 설정해야 정확함)
git config --global core.ignorecase false
```

---

## 1. 📥 시작하기 & 동기화 (Clone/Pull)

### 🔹 리포지토리 가져오기
```bash
git clone [https://github.com/](https://github.com/)[사용자명]/[리포지토리명].git
```

### 🔹 원격 저장소 연결 확인 및 변경
```bash
# 연결된 주소 확인
git remote -v

# 원격 주소 변경 (리포지토리 이사 갈 때)
git remote set-url origin [https://github.com/](https://github.com/)[사용자명]/[새_리포지토리명].git
```

### 🔹 최신 내용 당겨오기 (작업 전 필수!)
```bash
git pull origin main
```

---

## 2. 💾 저장 및 업로드 루틴 (Routine)

### 🔹 1단계: 변경사항 확인 (Diff)
```bash
# 파일 목록 확인
git status

# 변경된 코드 상세 비교 (Q키로 종료)
git diff
```

### 🔹 2단계: 파일 담기 (Staging)
```bash
# 모든 변경사항 담기
git add .
```

### 🔹 3단계: 커밋 (Commit)
```bash
git commit -m "[작업 내용 요약]"
```

### 🔹 4단계: 업로드 (Push)
```bash
git push origin main
```

---

## 3. 💥 충돌 해결 (Conflict)
`git pull`이나 `git merge` 시 코드가 겹쳐서 에러가 날 때 대처법입니다.

### 🔹 해결 순서
1. `git status`로 충돌난 파일 확인 ("Both modified"라고 뜸).
2. VSCode에서 해당 파일을 열면 `<<<<<<< HEAD` 와 같은 표시가 보임.
3. 원하는 코드만 남기고 특수기호(`<<<<`, `====`, `>>>>`)를 모두 지움.
4. 저장 후 다시 커밋 진행.

```bash
git add .
git commit -m "fix: 충돌 해결"
git push origin main
```

---

## 4. 🙈 파일 무시 (.gitignore)
보안 파일(.env)이나 용량이 큰 폴더(node_modules)를 Git에 올리지 않도록 설정합니다.

### 🔹 캐시 삭제 (이미 올라간 파일 지우기)
`.gitignore`에 추가했는데도 계속 Git에 뜰 때 사용합니다.
```bash
git rm -r --cached .
git add .
git commit -m "fix: gitignore 적용"
```

---

## 5. ↩️ 실수 수습 & 되돌리기 (Undo)

### 🔹 방금 한 커밋 취소하기 (파일 수정 내용은 유지)
```bash
git reset --soft HEAD~1
```

### 🔹 특정 커밋의 내용을 반대로 실행 (Revert)
이미 `push` 해버린 커밋을 안전하게 취소할 때 사용합니다. (기록이 남음)
```bash
git revert [커밋ID]
```

### 🔹 수정 사항 다 버리고 파일 원상복구 (주의!)
```bash
git checkout .
```

### 🔹 임시 저장 (Stash)
```bash
# 임시 저장
git stash

# 불러오기 및 삭제
git stash pop
```

---

## 6. 🌿 가지치기 & 협업 (Branch)

### 🔹 브랜치 관리
```bash
# 브랜치 목록 확인
git branch -a

# 새 브랜치 생성 및 이동
git checkout -b [새_브랜치명]
```

### 🔹 병합 (Merge)
```bash
# 1. 메인 브랜치로 이동
git checkout main

# 2. 작업 브랜치 합치기
git merge [작업한_브랜치명]
```

### 🔹 브랜치 삭제
```bash
# 로컬 브랜치 삭제
git branch -d [삭제할_브랜치명]
```

---

## 7. 🏷️ 버전 태그 (Tag)

### 🔹 태그 생성 및 업로드
```bash
# 태그 생성 (예: v1.0.0)
git tag [태그명]

# 태그 업로드
git push origin [태그명]
```

---

## 8. 🔥 초기화 & 연결 해제 (Danger Zone)

### 🔹 커밋 기록 완전 초기화
```bash
git checkout --orphan latest_branch
git add -A
git commit -m "Initial commit"
git branch -D main
git branch -m main
git push -f origin main
```

### 🔹 Git 폴더 삭제 (연결 끊기)
```powershell
# Windows PowerShell
Remove-Item -Recurse -Force .git
```