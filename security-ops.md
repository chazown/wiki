# 🛡️ Security Operations & Logs

## 1. 🚧 Network Security (Firewall)
서버의 1차 방어선인 방화벽 설정 및 관리 매뉴얼입니다.

### 🔹 Ubuntu / Debian 방화벽 (UFW)
# 상태 확인 (verbose로 상세 정보 확인)
sudo ufw status verbose

# 방화벽 활성화 (부팅 시 자동 실행됨)
sudo ufw enable

# 방화벽 비활성화 (긴급 시)
sudo ufw disable

### 🔹 UFW 정책 허용 (Allow)
# 기본 포트 허용 (SSH, HTTP, HTTPS)
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# 특정 포트 허용
sudo ufw allow [포트번호]/tcp

# 특정 IP에서의 접속만 허용 (예: 관리자 PC, 사무실 IP)
sudo ufw allow from [허용할_IP주소]

# 특정 IP가 특정 포트로 들어오는 것만 허용 (보안 강도 높음)
sudo ufw allow from [허용할_IP주소] to any port [포트번호]

### 🔹 UFW 정책 차단 (Deny) 및 삭제
# 특정 IP 차단 (공격자 IP)
sudo ufw deny from [차단할_IP주소]

# 기존에 설정한 정책 삭제 (예: 8080 포트 허용 룰 삭제)
sudo ufw delete allow 8080/tcp

# 정책 번호로 삭제 (규칙이 많을 때 유용)
sudo ufw status numbered
sudo ufw delete [번호]

### 🔹 RHEL / CentOS 방화벽 (Firewall-cmd)
RedHat 계열(CentOS, Rocky Linux)에서 주로 사용하는 명령어입니다.
# 상태 확인
sudo firewall-cmd --state

# 영구적으로 포트 허용 (예: 443 포트)
sudo firewall-cmd --zone=public --add-port=[포트번호]/tcp --permanent

# [추가] 영구적으로 포트 삭제 (예: 443 포트)
sudo firewall-cmd --zone=public --remove-port=[포트번호]/tcp --permanent

# 변경사항 적용 (reload)
sudo firewall-cmd --reload

# 영구 설정 목록 확인
sudo firewall-cmd --list-all

---

## 2. 🕵️ Log Analysis & Monitoring
침해 사고 발생 시 로그를 분석하여 원인을 파악합니다.

### 🔹 실시간 로그 감시 (Live Tail)
# 인증 로그 (SSH 접속 시도, sudo 권한 사용 등)
tail -f /var/log/auth.log

# 시스템 전체 로그 (Syslog - Ubuntu/Debian)
tail -f /var/log/syslog

# 웹 서버 로그 (Nginx 예시)
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log

### 🔹 침해 흔적 검색 (Grep Analysis)
# SSH 로그인 실패 기록 검색 ('Failed password' 문구)
grep "Failed password" /var/log/auth.log

# 특정 IP의 활동 내역 검색
grep "[IP주소]" /var/log/access.log

# 공격자 IP 통계 추출 (접속 실패가 많은 상위 10개 IP)
grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr | head -10

---

## 3. 🌐 Connection & Port Check
현재 서버와 연결된 네트워크 상태를 점검합니다.

### 🔹 열린 포트 확인
# 현재 리스팅 중인(Listen) 포트와 프로그램 확인
sudo netstat -tulpn

### 🔹 특정 포트 사용자 확인
# 포트 번호로 프로세스 찾기
sudo lsof -i :[포트번호]

---

## 4. 🚨 Service Management
보안 장비 및 로깅 데몬 상태를 점검하고 제어합니다.

### 🔹 서비스 상태 점검
# 서비스 상태 확인 (Active/Inactive 확인)
systemctl status [서비스명]
# 예: ufw, sshd, fail2ban, elasticsearch, logstash 등

### 🔹 서비스 제어
# 서비스 재시작 (설정 변경 후 적용 시)
sudo systemctl restart [서비스명]

# 서비스 중지
sudo systemctl stop [서비스명]

# 부팅 시 자동 시작 등록
sudo systemctl enable [서비스명]

---
## 5. 🤖 Advanced Security (Automation)
주인님 전용 보안 자동화 도구 활용 가이드입니다.

### 🔹 취약점 자동 진단 (Vuln-Scan)
- `vuln-auto-scan` 리포지토리를 활용하여 주요정보통신기반시설 기술적 점검 수행.
- 점검 후 결과값 DB 저장 및 보고서 자동화 연동.

### 🔹 보안 운영 자동화 (SOAR)
- `soar` 프로젝트의 Ansible 플레이북을 활용하여 반복적인 침해 사고 대응 프로세스 자동화.
