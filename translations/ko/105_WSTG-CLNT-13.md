## WSTG-CLNT-13 — 사이트 간 스크립트 포함(Cross-Site Script Inclusion) 테스트

사이트 간 스크립트 포함(Cross-Site Script Inclusion, XSSI)은 웹 애플리케이션이 동적으로 생성된 JavaScript 파일이나 JSONP 응답 내에 민감한 데이터를 포함하여 내보낼 때 발생하며, 공격자가 제어하는 사이트에서 `<script>` 태그를 통해 이를 포함할 수 있습니다. 스크립트 포함은 동일 출처 정책(Same-Origin Policy, SOP)을 우회하므로, 공격자는 자신의 컨텍스트에서 스크립트를 실행하고 전역 객체 또는 변수를 오버라이드(Override)하여 민감한 정보를 탈취(Exfiltration)할 수 있습니다. 이 취약점은 주로 이메일 주소, API 키(API keys) 또는 세션 메타데이터와 같은 사용자 특정 데이터를 스크립트 형식으로 반환하는 인증된 엔드포인트에서 나타납니다. 공격자 관점에서 익스플로잇(Exploit)은 취약한 스크립트를 참조하고 그 결과로 발생하는 전역 변수 변경이나 함수 호출을 캡처하는 악성 페이지를 제작하는 과정을 포함합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 보통 / 높음 |

> *유출된 데이터에 CSRF 토큰, 세션 식별자 또는 매우 민감한 개인 식별 정보(Personally Identifiable Information, PII)가 포함된 경우 심각도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### 인증된 엔드포인트가 민감한 정보를 포함하는 동적 JavaScript 또는 JSONP를 반환합니까?
- [ ] 아니요 — 모든 스크립트가 정적이며 사용자 특정 데이터를 포함할 수 없습니다.
- [ ] 예 — 동적 스크립트가 존재하지만 민감한 정보는 포함되어 있지 않습니다.
- [ ] 예 — 동적 스크립트에 PII 또는 토큰이 포함되어 있으며 출처 간(Cross-Origin) 접근이 가능합니다.

### 민감한 스크립트 응답에 `X-Content-Type-Options: nosniff` 헤더가 구현되어 있습니까?
- [ ] 예 — 헤더가 적용되어 MIME 타입 스니핑(MIME-type sniffing)을 방지합니다.
- [ ] 아니요 — 헤더가 누락되어 스크립트가 아닌 콘텐츠가 스크립트로 실행될 가능성이 있습니다.

### 민감한 스크립트가 실행 불가능한 접두사(Non-executable prefixes) 또는 사용자 정의 헤더와 같은 anti-XSSI 메커니즘으로 보호됩니까?
- [ ] 예 — 스크립트 호출 시 표준 `<script>` 태그를 통해 전송할 수 없는 사용자 정의 헤더 또는 토큰이 필요합니다.
- [ ] 예 — 스크립트가 쉽게 우회할 수 없는 실행 불가능한 접두사(예: `)]}'`)를 사용합니다.
- [ ] 아니요 — 앰비언트 자격 증명(Ambient credentials)을 사용하는 GET 요청을 통해 스크립트에 직접 접근할 수 있습니다. *(중대함)*

### 전역 변수나 프로토타입(Prototypes)을 오버라이드하여 민감한 데이터를 탈취하는 것이 가능합니까?
- [ ] 아니요 — 민감한 데이터의 범위가 로컬로 제한되거나 현대적인 브라우저 기능을 통해 보호됩니다.
- [ ] 예 — 민감한 데이터가 전역 변수에 할당되어 캡처될 수 있습니다.
- [ ] 예 — 인터셉트(Intercept) 가능한 JSONP 콜백을 통해 데이터가 유출됩니다.

---