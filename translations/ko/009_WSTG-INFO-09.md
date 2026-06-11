## WSTG-INFO-09 — 웹 애플리케이션 지문 분석 (Fingerprint Web Application)

웹 애플리케이션 지문 분석 (Fingerprinting)은 특정 프레임워크, 콘텐츠 관리 시스템 (CMS) 또는 기반 기술 스택 (Technology stack) 및 관련 버전을 식별하는 과정입니다. 이 정찰 단계는 식별된 구성 요소를 알려진 취약점 (CVE) 및 공개된 익스플로잇 (Exploit)에 매핑하여 공격 표면 (Attack surface)을 크게 좁힐 수 있게 해주므로 매우 중요합니다. 정보는 일반적으로 HTTP 응답 헤더, 고유한 파일 경로, 쿠키 이름, HTML 소스 코드 메타데이터 및 정적 자산의 암호화 해시를 통해 수집됩니다. 기술 스택을 정확하게 파악함으로써 공격자는 일반적인 자동 스캔에서 버전별 약점을 겨냥한 고도로 정밀한 공격으로 전환할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 낮음 / 정보성 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP 응답 헤더가 기술 스택이나 버전 번호를 노출하고 있습니까?
- [ ] 아니요 — 민감한 헤더가 제거되었거나 일반적인 값을 포함하고 있습니다.
- [ ] 예 — `X-Powered-By`, `Server`, 또는 `X-AspNet-Version`과 같은 기본 헤더가 존재합니다.
- [ ] 예 — 헤더가 웹 서버 또는 애플리케이션 프레임워크의 구체적이고 오래된 버전을 노출합니다.

### 고유한 파일 경로 또는 명명 규칙을 통해 애플리케이션 프레임워크나 CMS를 식별할 수 있습니까?
- [ ] 아니요 — 파일 경로가 난독화되어 있으며 기반 기술을 드러내지 않습니다.
- [ ] 예 — 일반적인 디렉터리 구조 (예: `/wp-content/`, `/_next/`, `/node_modules/`)가 보입니다.
- [ ] 예 — `robots.txt`, `humans.txt`, 또는 `sitemap.xml`과 같은 고유한 파일에 기술별 메타데이터가 포함되어 있습니다.

### HTML 소스 코드나 정적 자산 내에 특정 버전 번호가 노출되어 있습니까?
- [ ] 아니요 — 주석이나 자산 파라미터에서 버전 정보가 발견되지 않습니다.
- [ ] 예 — HTML 주석이나 메타 태그 (예: `<meta name="generator" content="...">`)가 존재합니다.
- [ ] 예 — CSS/JS 파일명에 버전 문자열이 추가되어 있으며 (예: `jquery.js?v=1.12.4`) 이를 비활성화할 수 없습니다.

### 기본 에러 페이지나 관리 인터페이스가 소프트웨어 환경을 노출합니까?
- [ ] 아니요 — 애플리케이션이 모든 상태 코드에 대해 사용자 정의된 일반 에러 페이지를 사용합니다.
- [ ] 예 — 웹 서버나 프레임워크의 기본 에러 페이지가 활성화되어 있으며 버전 데이터를 유출합니다.
- [ ] 예 — 관리자 로그인 포털 (예: `/wp-admin`, `/phpmyadmin`)에 접근 가능하며 식별할 수 있습니다.

### 쿠키가 세션 관리 프레임워크를 노출하고 있습니까?
- [ ] 아니요 — 세션 쿠키가 일반적이거나 사용자 정의된 이름을 사용합니다.
- [ ] 예 — 기술별 쿠키 이름 (예: `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`)이 사용됩니다.

---