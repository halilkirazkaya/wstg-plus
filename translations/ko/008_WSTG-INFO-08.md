## WSTG-INFO-08 — 웹 애플리케이션 프레임워크 핑거프린팅 (Fingerprint Web Application Framework)

웹 애플리케이션 프레임워크 핑거프린팅(Fingerprinting)은 애플리케이션을 구축하고 서비스하는 데 사용된 특정 기술 스택과 소프트웨어 버전을 식별하는 작업을 포함합니다. 공격자는 HTTP 응답 헤더, 프레임워크별 쿠키(Cookies), 예측 가능한 파일 구조, 고유한 자바스크립트 아티팩트(JavaScript Artifacts)를 분석하여 대상이 React, Angular, Django 또는 Spring과 같은 플랫폼에서 실행 중인지 판단합니다. 이 정찰 단계(Reconnaissance Phase)는 테스터가 공격 표면(Attack Surface)을 해당 프레임워크에 특화된 알려진 취약점(CVE) 및 일반적인 설정 약점으로 좁힐 수 있게 해주므로 매우 중요합니다. 기본 아키텍처를 이해함으로써 공격자는 일반적인 테스트에서 고도로 타겟팅된 익스플로잇(Exploit) 전략으로 전환할 수 있습니다.

| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **테스트 상태** | Not Performed |
| **위험도** | Informational |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP 응답 헤더가 프레임워크 이름이나 버전을 노출하고 있습니까?
- [ ] 아니요 — `X-Powered-By` 또는 `Server`와 같은 헤더가 존재하지 않거나 정제(Sanitized)되어 있습니다.
- [ ] 예 — 프레임워크 이름이 존재하지만 버전은 숨겨져 있습니다.
- [ ] 예 — 프레임워크 이름과 버전이 모두 노출되어 있습니다.

### 프레임워크 고유의 쿠키 또는 디렉터리 구조를 식별할 수 있습니까?
- [ ] 아니요 — 일반적인 프레임워크 아티팩트(예: `.js` 경로, 쿠키 이름)가 난독화되거나 이름이 변경되었습니다.
- [ ] 예 — 프레임워크 고유의 쿠키(예: `JSESSIONID`, `PHPSESSID`, `csrftoken`)를 식별할 수 있습니다.
- [ ] 예 — 디렉터리 구조(예: `/wp-content/`, `_next/static/`)가 보이며 프레임워크를 확인해 줍니다.

### 오류 메시지나 소스 코드 주석이 프레임워크 세부 정보를 공개합니까?
- [ ] 아니요 — 오류 처리가 일반적이며 주석이 제거되었습니다.
- [ ] 예 — 프레임워크 전용 스택 트레이스(Stack traces)가 활성화되어 응답에서 확인 가능합니다.
- [ ] 예 — HTML 소스 코드에 프레임워크를 식별하는 주석이나 메타데이터 태그가 포함되어 있습니다.

### 식별된 프레임워크 버전과 관련된 알려진 취약점이 있습니까?
- [ ] 아니요 — 식별된 프레임워크가 최신 버전이며 알려진 공개 취약점이 없습니다.
- [ ] 예 — 프레임워크 버전이 오래되었으며 특정 CVE를 식별할 수 있습니다.

---