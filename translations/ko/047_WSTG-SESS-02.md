## WSTG-SESS-02 — 쿠키 속성 테스트 (Testing for Cookies Attributes)

세션 관리(Session Management)는 종종 상태를 유지하기 위해 HTTP 쿠키(HTTP Cookies)에 의존하므로, 이러한 쿠키의 보안 속성(Security attributes)은 공격자의 주요 목표가 됩니다. `HttpOnly`, `Secure`, `SameSite`와 같은 속성을 누락하거나 잘못 설정할 경우, 애플리케이션은 교차 사이트 스크립팅(Cross-Site Scripting, XSS), 암호화되지 않은 채널을 통한 가로채기 또는 교차 사이트 요청 위조(Cross-Site Request Forgery, CSRF) 공격을 통해 세션 토큰이 탈취될 위험에 노출됩니다. 모의해킹 전문가(Pentesters)는 이러한 플래그를 검토하여 공격자가 브라우저의 문서 객체 모델(Document Object Model, DOM)에서 세션 식별자를 유출하거나 브라우저가 교차 출처 요청(Cross-origin requests) 시 세션 식별자를 강제로 전송하게 할 수 있는지 확인합니다. 적절한 속성 설정은 민감한 토큰이 승인된 보안 컨텍스트 내에만 유지되도록 보장하는 기본적인 심층 방어(Defense-in-depth) 조치입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### 쿠키 플래그 구현 분석 (Analysis of Cookie Flag Implementation)
| 속성 | 적용됨 | 누락됨 |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### 민감한 세션 쿠키에 `HttpOnly` 플래그가 적용되어 있습니까?
- [ ] 예 — **모든** 민감한 세션 쿠키에 적용됨 *(가장 안전)*  
- [ ] 예 — 일부 세션 쿠키에만 적용됨  
- [ ] 아니요 — 플래그가 **적용되지 않아**, 클라이언트 측 스크립트를 통한 접근이 허용됨 *(치명적)*  

### HTTPS를 통해 전송되는 쿠키에 `Secure` 플래그가 강제 적용됩니까?
- [ ] 예 — 전송 계층 보안(TLS)을 통해 전송되는 **모든** 쿠키에 적용됨  
- [ ] 아니요 — 플래그가 **적용되지 않아**, 평문 HTTP를 통해 쿠키가 전송될 수 있음  

### CSRF 위험을 완화하기 위해 `SameSite` 속성이 사용됩니까?
- [ ] 예 — 세션 쿠키에 대해 `Strict` 또는 `Lax`로 설정됨  
- [ ] 예 — `None`으로 설정되었으나 `Secure` 플래그가 존재함  
- [ ] 아니요 — 속성이 **누락**되었거나 `Secure` 플래그 **없이** `None`으로 설정됨  

### `Domain` 및 `Path` 속성이 필요한 최소 범위로 제한되어 있습니까?
- [ ] 예 — **특정** 서브도메인 및 애플리케이션 경로로 범위가 제한됨  
- [ ] 아니요 — 범위가 지나치게 **넓게** 설정되어 (예: 상위 도메인 또는 루트 `/` 설정), 공격 표면이 증가함  

### `Expires` 또는 `Max-Age` 속성을 통해 민감한 세션 쿠키가 적절히 만료됩니까?
- [ ] 예 — 적절한 만료 날짜가 설정됨  
- [ ] 아니요 — 쿠키 수명이 과도하게 **길게** 설정되어 지속됨  
- [ ] 아니요 — 쿠키가 세션 전용이지만 서버 측 타임아웃 강제가 **누락**됨  

---