# 📥 Git Master Guide 

### 0. 🚨 시작 전 절대 주의사항
명령어를 입력하기 전, 터미널의 현재 경로가 **프로젝트 전용 폴더**인지 반드시 확인하세요.

 🔹 현재 경로 확인
```bash
# "Print Working Directory" - 현재 내가 서 있는 폴더 위치를 터미널에 출력합니다.
pwd
```
⚠️ 경고: C:\Users\사용자명 같은 루트 폴더에서 git init을 수행하면 안 됩니다.

만약 잘못 생성했다면 즉시 아래 명령어로 해제하세요.

🔹 잘못된 Git 연결 해제 (Bash)
```bash
# .git 폴더를 강제로(-f) 삭제하여 해당 폴더의 모든 Git 추적 기록을 지웁니다.
rm -rf .git
```

### 1. ⚙️ 초기 환경 설정 (Config & Terminal)
새 환경 세팅 시 가장 먼저 수행합니다.

### 🔹 유저 정보 등록
```bash
# 본인 정보 등록
git config --global user.name "[사용자명]"
git config --global user.email "[이메일주소]"

# 환경 설정 (줄바꿈/대소문자)
git config --global core.autocrlf true
git config --global core.ignorecase false
```

🔹터미널 및 계정 인증
터미널 설정:

Ctrl+Shift+P → Terminal: Select Default Profile → Git Bash 선택.

계정 연결: VSCode 왼쪽 하단 [사람 아이콘] → Sign in with GitHub 클릭하여 브라우저 인증.


### 2. 📥 시작하기 & 동기화 (Clone/Pull)
🔹 리포지토리 가져오기
```bash
git clone [https://github.com/](https://github.com/)[사용자명]/[리포지토리명].git
```
🔹 원격 저장소 확인
 ```bash
git remote -v
```

🔹 원격 주소 변경 (리포지토리 이사 갈 때)
```bash
git remote set-url origin [https://github.com/](https://github.com/)[사용자명]/[새_리포지토리명].git
```
🔹 최신 내용 당겨오기 (작업 전 필수!)
```bash
git pull origin main
```
### 3. 💾 저장 및 업로드 루틴 (Routine)
 🔹 1단계: 변경사항 확인 (Diff)
```bash
# 파일 목록 확인
git status

# 변경된 코드 상세 비교 (Q키로 종료)
git diff
```

 🔹 2단계: 파일 담기 (Staging)
```bash
# 모든 변경사항 담기
git add .
```

 🔹 3단계: 커밋 (Commit)
```bash
git commit -m "[작업 내용 요약]"
```

 🔹 4단계: 업로드 (Push)
```bash
git push origin main
```

## 4. 💥 충돌 해결 (Conflict)
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

## 5. 🙈 파일 무시 (.gitignore)
보안 파일(.env)이나 용량이 큰 폴더(node_modules)를 Git에 올리지 않도록 설정합니다.

🔹 캐시 삭제 (이미 올라간 파일 지우기)
`.gitignore`에 추가했는데도 계속 Git에 뜰 때 사용합니다.
```bash
git rm -r --cached .
git add .
git commit -m "fix: gitignore 적용"
```

## 6. 🔥 강제 업데이트 (Force Update)
서버의 기록을 무시하고 내 로컬 코드로 강제 덮어쓰기를 수행합니다.

🔹 [Case A] 메인 브랜치 최초 연결 및 강제 푸시
로컬과 서버를 처음 연결하거나 기록 세탁 후 사용합니다.

-u 옵션은 로컬과 서버의 브랜치를 '단짝'으로 묶어주어 이후 git push만 쳐도 작동하게 만듭니다.

```bash
# 첫 역사 기록 생성
git add .
git commit -m "Initial commit: Professional Architecture"

# 브랜치 고정 및 강제 업로드 (-u로 업스트림 설정)
git branch -M main
git push -u origin main --force
```
🔹 [Case B] 특정 브랜치 강제 수정
작업 중인 특정 브랜치의 기록이 꼬였을 때 해당 브랜치만 덮어씁니다.

```bash
git push origin [브랜치명] --force
```

### 7. ↩️ 실수 수습 & 되돌리기 (Undo)
🔹 수정 사항 버리기 & 임시 저장 (Stash)

```bash
# 수정 사항 다 버리고 파일 원상복구 (주의!)
git checkout .

# 작업 중인 내용 임시 저장
git stash
# 임시 저장 목록 불러오기 및 삭제
git stash pop
```
🔹 방금 한 커밋 세탁 (Amend)
reset으로 커밋을 지우기엔 번거로울 때, 즉시 덮어쓰는 용도입니다.

```bash
# 1. 커밋 메시지만 수정하고 싶을 때
git commit --amend -m "새로운 커밋 메시지"

# 2. 깜빡한 파일을 방금 커밋에 합치고 싶을 때
git add [누락파일]
git commit --amend --no-edit  # --no-edit: 메시지는 유지함
```

🔹 커밋 취소 및 서버 상태 복구
```bash
# 방금 한 커밋 취소 (수정 내용은 유지)
git reset --soft HEAD~1

# 서버 상태로 강제 복구 (가장 확실한 방법)
git fetch origin
git reset --hard origin/main
```

### 8. 🌿 가지치기 & 협업 (Branch)
🔹 브랜치 관리
```bash
# 목록 확인
git branch -a

# 새 브랜치 생성 및 이동
git checkout -b [새_브랜치명]
```
🔹 병합 (Merge) 및 삭제
```bash
# 1. 메인 브랜치로 이동 후 합치기
git checkout main
git merge [작업한_브랜치명]

# 2. 로컬 브랜치 삭제
git branch -d [삭제할_브랜치명]
```

### 9. 🏷️ 버전 태그 (Tag)
```bash
# 태그 생성 (예: v1.0.0)
git tag [태그명]

# 태그 업로드
git push origin [태그명]
```
### 10. 🔌 초기화 & 연결 해제 (Danger Zone)
    
🔹 커밋 기록 완전 초기화 (Orphan)
기존 설정을 유지한 채 히스토리를 완전히 비우고 새 역사를 만들 때 사용합니다.

```bash
git checkout --orphan latest_branch
git add -A
git commit -m "Initial commit"
git branch -D main
git branch -m main
git push -f origin main
```
🔹 Git 설정 완전 삭제 (연결 끊기)
```bash
rm -rf .git
```
