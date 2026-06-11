## WSTG-INFO-02 — 웹 서버 핑거프린팅 (Fingerprint Web Server)

웹 서버 핑거프린팅 (Fingerprinting)은 대상 웹 서버의 특정 소프트웨어 유형, 버전 및 기반 운영 체제를 식별하는 프로세스입니다. 공격자는 Apache, Nginx, IIS 또는 기타 서버 기술의 특정 버전에 고유한 패치되지 않은 CVE 또는 구성 취약점과 같이 알려진 취약점을 식별하기 위해 이러한 정찰 활동을 수행합니다. 이는 일반적으로 HTTP 응답 헤더 (HTTP response headers, 예: `Server`, `X-Powered-By`)를 분석하거나, 고유한 프로토콜 동작을 조사하거나, 기본 에러 페이지 및 파일을 통해 유출된 정보를 관찰함으로써 수행됩니다. 성공적인 핑거프린팅을 통해 공격자는 일반적인 스캔 방식에서 벗어나 알려진 아키텍처 결함에 대한 높은 확률의 공격으로 익스플로잇 (Exploitation) 전략을 맞춤화할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 정보성 (Informational) / 낮음 (Low)* |

> *위험도는 핑거프린팅을 통해 알려진 치명적인 취약점이 있는 구식 또는 수명 종료 (End-of-life, EOL) 서버 버전이 발견될 경우 높음 (High)으로 격상됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**도구:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### HTTP 응답 헤더에 식별 문자열이 존재합니까?
- [ ] 아니요 — `Server` 및 `X-Powered-By` 헤더가 **존재하지 않거나** 일반적인 값을 포함하고 있음  
- [ ] 예 — 헤더가 서버 소프트웨어를 노출하지만 특정 버전 번호는 **포함하지 않음**  
- [ ] 예 — 헤더가 특정 서버 소프트웨어와 **정확한** 버전 번호를 노출함  

### 기본 에러 페이지 또는 시스템 생성 응답이 서버 세부 정보를 유출합니까?
- [ ] 아니요 — 사용자 정의 에러 페이지가 사용되며 서버 정보를 **노출하지 않음**  
- [ ] 예 — 기본 에러 페이지가 서버 소프트웨어 이름 및/또는 버전을 유출함  
- [ ] 예 — 에러 페이지가 서버 세부 정보, 버전 및 **기반 운영 체제**를 유출함  

### 기본 파일 또는 디렉터리 구조를 통해 서버 버전이 노출됩니까?
- [ ] 아니요 — 기본 설치 파일, 매뉴얼 및 테스트 스크립트가 **제거됨**  
- [ ] 예 — 기본 파일(예: `info.php`, `manual/`, `test.html`)이 **존재하며** 버전 데이터를 노출함  

### 고유한 동작 분석을 통해 서버 유형을 정확하게 추론할 수 있습니까?
- [ ] 아니요 — 서버 응답이 정규화되어 있으며 동작 기반 핑거프린팅이 **불가능함**  
- [ ] 예 — 고유한 동작(예: 헤더 순서, 특정 에러 코드 또는 TCP/IP 스택 특이점)을 통해 서버 식별이 **가능함**  

### 식별된 서버 버전이 현재 지원되고 패치되었습니까?
- [ ] 예 — 식별된 버전이 최신이며 벤더 (Vendor)에 의해 **지원됨**  
- [ ] 아니요 — 식별된 버전이 **구식**이거나 **수명 종료 (EOL)** 상태이며 알려진 취약점이 존재함  

---