## WSTG-CONF-05 — 인프라 및 애플리케이션 관리 인터페이스 열거 (Enumerate Infrastructure and Application Admin Interfaces)

관리 인터페이스는 애플리케이션 및 기본 인프라의 관리 제어 평면(Management Control Plane)을 나타내며, 종종 시스템 구성, 사용자 데이터 및 서버 측 작업에 대한 권한 있는 액세스(Privileged Access)를 부여합니다. 공격자는 디렉토리 브루트 포싱(Directory Brute-forcing), `robots.txt` 분석 또는 예측 가능한 URL 패턴 관찰을 통해 이러한 인터페이스를 열거하는 것을 우선순위로 두어, 강력한 인증이 부족하거나 기본 자격 증명(Default Credentials)에 의존할 수 있는 진입점을 찾습니다. 노출된 관리자 패널의 발견은 전체 시스템 침해(System Compromise), 무단 데이터 수정 또는 서비스 중단의 위험을 크게 증가시킵니다. 이러한 인터페이스는 비표준 포트나 서브도메인에서 빈번하게 발견되어 자동화된 스캐너와 수동 정찰(Reconnaissance)의 주요 목표가 됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간(Medium) / 높음(High)* |

> *인터페이스가 다요소 인증(MFA) 없이 시스템 전반의 구성 변경, 사용자 관리 또는 데이터 유출을 허용하는 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### 디렉토리 퍼징(Directory Fuzzing) 또는 정찰을 통해 관리 인터페이스를 발견할 수 있습니까?
- [ ] 아니요 — 노출되었거나 발견 가능한 관리 인터페이스가 없습니다.
- [ ] 예 — 인터페이스를 발견할 수 있으나 액세스가 내부 IP 대역으로 제한됩니다.
- [ ] 예 — 인터페이스를 발견할 수 있으며 공개적으로 액세스 가능합니다.

### 발견된 관리 포털에 인증이 강제 적용됩니까?
- [ ] 예 — 다요소 인증(MFA)이 강제 적용됩니다.
- [ ] 예 — 단일 요소 인증(Single-factor Authentication)이 강제 적용됩니다.
- [ ] 아니요 — 인터페이스 액세스에 인증이 필요하지 않습니다 *(위험: Critical)*.

### 발견을 방지하기 위해 관리 경로가 숨겨져 있거나 비표준적입니까?
- [ ] 예 — 경로가 비표준적이고, 무작위화되었거나 예측 불가능한 명명 규칙을 사용합니다.
- [ ] 아니요 — 경로가 일반적인 명명 규칙(예: `/admin`, `/manager`, `/console`)을 사용하며 쉽게 추측할 수 있습니다.

### 인터페이스가 민감한 시스템 또는 애플리케이션 기능에 대한 액세스를 제공합니까?
- [ ] 아니요 — 인터페이스가 영향이 미미한 "읽기 전용" 상태 대시보드입니다.
- [ ] 예 — 인터페이스가 애플리케이션 구성 또는 사용자 데이터를 수정할 수 있습니다.
- [ ] 예 — 인터페이스가 시스템 수준의 명령을 실행하거나 데이터베이스 관리(Database Management)를 수행할 수 있습니다.

---