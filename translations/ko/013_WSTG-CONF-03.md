## WSTG-CONF-03 — 민감한 정보에 대한 파일 확장자 처리 (File Extension Handling) 테스트

파일 확장자 처리 테스트는 웹 서버나 애플리케이션 서버가 제한되거나 실행되어야 할 파일을 제공함으로써 민감한 정보를 노출하는지 여부를 식별하는 과정을 포함합니다. 공격자들은 특정 핸들러 매핑(Handler Mappings)의 부재로 인해 웹 루트(Web Root)에 남겨져 평문(Plain Text)으로 제공될 수 있는 백업 파일, 설정 파일, 소스 코드 조각(예: `.bak`, `.old`, `.env`, `.inc`)을 빈번하게 탐색합니다. 이러한 취약점은 데이터베이스 자격 증명(Credentials), API 키(API Keys), 내부 비즈니스 로직의 노출로 이어져 인프라에 대한 심층적인 공격을 용이하게 하므로 중요합니다. 이러한 노출은 주로 공용 디렉터리, 업로드 폴더 또는 서버의 잘못 설정된 MIME 타입(MIME-type) 설정을 통해 발생합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *자격 증명을 포함한 설정 파일이나 전체 소스 코드에 접근 가능한 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### 민감한 백업 또는 임시 파일 확장자(예: `.bak`, `.old`, `.swp`, `~`)에 접근 가능합니까?
- [ ] 아니오 — 서버가 일반적인 백업 확장자에 대해 403 Forbidden 또는 404 Not Found를 반환함  
- [ ] 예 — 서버가 백업 파일 다운로드를 허용하지만, 민감한 데이터를 포함하고 있지 않음  
- [ ] 예 — 서버가 민감한 데이터나 소스 코드가 포함된 백업 파일의 다운로드를 허용함 *(높음)*  

### 서버가 환경 설정 또는 시스템 구성 파일(예: `.env`, `.config`, `.yml`, `.ini`)을 노출합니까?
- [ ] 아니오 — 제한된 설정 파일에 접근할 수 없으며 403/404를 반환함  
- [ ] 예 — 민감한 설정 파일에 접근 가능하며 내부 비밀 정보나 자격 증명이 노출됨  

### 대체 확장자(예: `.php.txt`, `.jsp.old`, `.aspx.bak`)를 추가하여 소스 코드를 추출할 수 있습니까?
- [ ] 아니오 — 확장자 조작과 관계없이 애플리케이션 코드가 평문으로 렌더링되지 않음  
- [ ] 예 — 서버에 추가된 확장자에 대한 핸들러가 없어 소스 코드가 노출됨  

### 관리용 또는 메타데이터 디렉터리(예: `.git/`, `.svn/`, `.DS_Store`)가 외부 접근으로부터 차단되어 있습니까?
- [ ] 아니오 — 해당 디렉터리/파일이 웹 루트에 존재하지 않음  
- [ ] 예 — 서버 측 리라이트 규칙(Rewrite Rules)이나 권한 설정으로 인해 접근이 불가능함  
- [ ] 예 — 메타데이터 디렉터리에 접근 가능하며 이를 통해 전체 소스 코드 복원이 가능함  

### 서버가 다중 확장자(예: `file.php.jpg`)를 가진 파일에 대한 요청을 어떻게 처리합니까?
- [ ] 아니오 — 서버가 내부 확장자의 보안을 올바르게 우선시하거나 요청을 차단함  
- [ ] 예 — 서버가 파일을 첫 번째 확장자로 처리하지만, 실행을 위한 우회(Bypass)는 불가능함  
- [ ] 예 — 서버가 최종 확장자를 무시하여 코드 실행이나 소스 노출이 가능함  

---