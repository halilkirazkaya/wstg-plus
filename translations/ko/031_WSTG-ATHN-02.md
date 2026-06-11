## WSTG-ATHN-02 — 기본 자격 증명 점검 (Testing for Default Credentials)

기본 자격 증명 점검(Testing for Default Credentials)은 공장 출고 시 설정되거나 벤더가 제공한 사용자 이름 및 비밀번호를 여전히 사용 중인 관리자 인터페이스, 서비스 또는 애플리케이션 구성 요소를 식별하는 작업을 포함합니다. 이 취약점은 공격자에게 최소한의 노력으로 시스템 전체 장악, 관리자 권한 획득 또는 원격 코드 실행(Remote Code Execution, RCE)으로 이어지는 즉각적인 경로를 제공하기 때문에 우선순위가 높은 대상입니다. 이는 일반적으로 상용 소프트웨어(Off-the-shelf software), CMS 플랫폼, 데이터베이스 관리 도구, 그리고 IoT 기기나 네트워크 어플라이언스(Network appliances)와 같은 통합 하드웨어에서 발생합니다. 취약점 공격(Exploitation)은 보통 초기 정찰(Reconnaissance) 및 공격 단계에서 식별된 소프트웨어 버전을 공개된 기본 비밀번호 데이터베이스와 대조하거나 자동화된 자격 증명 스터핑(Credential stuffing) 도구를 사용하여 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각(Critical) / 높음(High) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### 관리자 또는 관리 인터페이스가 인터넷이나 신뢰할 수 없는 네트워크 세그먼트에 노출되어 있습니까?
- [ ] 아니요 — 관리 인터페이스에 접근할 수 없습니다  
- [ ] 예 — 인터페이스가 존재하지만 네트워크 수준의 제어(예: VPN, IP 허용 목록(Allowlist))에 의해 제한됩니다  
- [ ] 예 — 인터페이스가 공개적으로 접근 가능하며 인증이 필요합니다  

### 식별된 모든 구성 요소에 대해 벤더가 제공한 기본 자격 증명이 변경되었습니까?
- [ ] 예 — 모든 구성 요소에서 모든 기본 자격 증명이 변경되었습니다 *(가장 안전)*  
- [ ] 예 — 주요 애플리케이션의 자격 증명은 변경되었으나, 보조 구성 요소(예: CMS, DB)에는 기본값이 남아 있습니다  
- [ ] 아니요 — 하나 이상의 식별 가능한 인터페이스에서 기본 자격 증명이 활성화되어 있습니다 *(심각)*  

### 애플리케이션이 신규 또는 관리자 계정의 첫 로그인 시 비밀번호 변경을 강제합니까?
- [ ] 예 — 어떠한 관리 작업도 수행하기 전에 비밀번호 변경이 필수적으로 요구됩니다  
- [ ] 아니요 — 애플리케이션이 기본 자격 증명을 무기한으로 사용할 수 있도록 허용합니다  

### 자동화된 기본 자격 증명 테스트를 방지하기 위한 계정 잠금 메커니즘이나 속도 제한(Rate Limiting) 제어가 활성화되어 있습니까?
- [ ] 예 — 강력한 속도 제한 또는 계정 잠금이 적용됩니다  
- [ ] 예 — 제어 장치가 존재하지만 헤더 조작이나 IP 순환(Rotation)을 통해 우회할 수 있습니다  
- [ ] 아니요 — 속도 제한이나 잠금 메커니즘이 활성화되어 있지 않습니다  

### 타사 플러그인, 미들웨어 또는 관리 도구(예: phpMyAdmin, Tomcat Manager)가 고유한 자격 증명을 사용합니까?
- [ ] 아니요 — 환경에 타사 구성 요소가 존재하지 않습니다  
- [ ] 예 — 모든 타사 구성 요소가 기본값이 아닌 고유한 자격 증명을 사용합니다  
- [ ] 아니요 — 적어도 하나의 타사 구성 요소를 알려진 기본 자격 증명을 통해 접근할 수 있습니다  

---