## WSTG-SESS-04 — 노출된 세션 변수 테스트 (Testing for Exposed Session Variables)

노출된 세션 변수는 민감한 세션 식별자(Session Identifier)나 상태 관련 데이터가 URL 쿼리 스트링(Query String), Referer 헤더 또는 시스템 로그와 같은 비보안 채널을 통해 전송될 때 발생합니다. 이러한 노출은 식별자가 브라우저 기록에 캐시되거나 중간 프록시에 로깅될 수 있으며, Referer 헤더를 통해 제3자 사이트로 유출될 수 있기 때문에 세션 하이재킹(Session Hijacking)의 위험을 크게 증가시킵니다. 모의해킹 전문가(Pentesters)는 모든 아웃바운드 요청을 모니터링하고 애플리케이션 로그나 서버 설정을 검사하여 의도치 않은 데이터 유출이 있는지 확인함으로써 이러한 노출을 식별합니다. 실제 상황에서 공격자는 공용 PC에서 유효한 세션 토큰(Session Token)을 수집하거나 트래픽 패턴을 분석하여 인증을 우회하고 사용자 계정에 대한 무단 액세스 권한을 획득함으로써 이러한 유출을 악용합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **테스트 상태** | 수행되지 않음 |
| **위험도(Severity)** | 중간 / 높음* |

> *노출된 변수가 즉각적인 계정 탈취를 허용하는 세션 토큰인 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### 세션 식별자가 URL 쿼리 스트링으로 전송됩니까?
- [ ] 아니요 — URL 쿼리 스트링에 세션 식별자가 존재하지 않습니다.
- [ ] 예 — 식별자가 존재하지만 세션의 수명이 짧거나 위험도가 낮습니다.
- [ ] 예 — 고유한 세션 ID가 URL 쿼리 스트링에 존재합니다 *(Critical)*.

### 애플리케이션이 Referer 헤더를 통해 세션 토큰을 유출합니까?
- [ ] 아니요 — `Referrer-Policy`가 유출을 방지하거나 제3자 링크가 존재하지 않습니다.
- [ ] 예 — 세션 토큰이 Referer 헤더를 통해 제3자 도메인으로 전송됩니다.

### 세션 변수가 서버 측 또는 애플리케이션 로그에 저장됩니까?
- [ ] 아니요 — 세션 변수가 마스킹 처리되거나 로그에서 제외됩니다.
- [ ] 예 — 세션 변수가 애플리케이션 또는 웹 서버 로그에 평문(Plain Text)으로 기록됩니다.

### 애플리케이션이 상태 변경 작업에 GET 요청을 사용합니까?
- [ ] 아니요 — 애플리케이션이 민감한 작업에 `POST` 또는 기타 비멱등(Non-idempotent) 메서드를 사용합니다.
- [ ] 예 — `GET` 메서드가 사용되어 세션 데이터가 브라우저 기록 및 중간 프록시에 캐시됩니다.

### 세션 관련 데이터의 캐싱을 방지하도록 `Cache-Control` 헤더가 설정되어 있습니까?
- [ ] 예 — 민감한 응답에 `Cache-Control: no-store`(또는 이와 유사한 설정)가 적용되어 있습니다.
- [ ] 예 — 제어 항목이 적용되어 있으나 브라우저 예외 사례를 통해 우회가 가능합니다.
- [ ] 아니요 — 세션 데이터를 포함하는 민감한 응답이 브라우저에 의해 캐시될 수 있습니다.

---