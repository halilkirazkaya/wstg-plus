## WSTG-CONF-14 — 기타 HTTP 보안 헤더 설정 오류 테스트

HTTP 보안 헤더 설정 오류(HTTP security header misconfigurations)에 대한 테스트는 일반적인 웹 기반 공격 벡터에 대해 필수적인 보호를 제공하는 방어 헤더의 존재 여부와 올바른 구현을 검증하는 작업을 포함합니다. 이러한 헤더들이 근본적인 코드 취약점을 해결하지는 못하지만, 헤더가 누락될 경우 중간자 공격(Man-in-the-Middle, MITM), MIME 스니핑(MIME-sniffing) 공격, 그리고 리퍼러(Referrers)를 통한 의도치 않은 교차 출처 데이터 유출(Cross-origin data leakage)의 위험이 크게 증가합니다. 공격자의 관점에서 보안 헤더의 누락은 보안 설정이 미흡한 환경임을 나타내는 가시성 높은 지표이며, 이는 종종 세션 하이재킹(Session hijacking)이나 정보 유출(Information exfiltration)과 같은 더 복잡한 익스플로잇(Exploit) 체인을 용이하게 합니다. 이러한 설정은 일반적으로 서버 수준에서 평가되며 API 엔드포인트(API endpoints) 및 정적 리소스(Static resources)를 포함한 모든 응답 유형에 일관되게 적용되어야 합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### 보안 헤더 존재 여부 감사
| 헤더 | 존재 | 누락 | 권장 설정 |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` 또는 `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (기능별 설정, 예: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### 애플리케이션 범위 전체에서 보안 헤더가 어떻게 강제되고 있습니까?
- [ ] 예 — 헤더가 중앙 로드 밸런서(Load balancer) 또는 리버스 프록시(Reverse proxy)를 통해 전역적으로 적용되며 재정의할 수 없음  
- [ ] 예 — 헤더가 주요 문서 응답에는 적용되지만, API 응답 또는 정적 자산에서는 누락됨  
- [ ] 아니요 — 헤더가 일관성 없이 적용되거나 전체 애플리케이션 도메인에서 누락됨  

### Strict-Transport-Security (HSTS) 헤더가 올바르게 설정되어 있습니까?
- [ ] 예 — `max-age`가 긴 기간(예: 2년)으로 설정되어 있고 `includeSubDomains`가 활성화됨  
- [ ] 예 — `max-age`는 설정되어 있으나 `includeSubDomains`가 비활성화되어 서브도메인이 취약한 상태임  
- [ ] 아니요 — HSTS 헤더가 존재하지 않거나 `max-age`가 0으로 설정되어 프로토콜 다운그레이드 공격(Protocol downgrade attacks)이 가능함  

### Referrer-Policy가 민감한 URL 파라미터의 유출을 방지합니까?
- [ ] 예 — 정책이 `no-referrer` 또는 `same-origin`으로 설정됨 (가장 안전)  
- [ ] 예 — 정책이 `strict-origin-when-cross-origin`으로 설정됨  
- [ ] 아니요 — 정책이 설정되지 않았거나 `unsafe-url`로 설정되어, 리퍼러 헤더를 통해 토큰이나 민감한 개인 식별 정보(Personally Identifiable Information, PII)가 유출될 가능성이 있음  

### X-Content-Type-Options 헤더가 MIME 스니핑을 방지하고 있습니까?
- [ ] 예 — `nosniff` 지시문이 존재하며 올바르게 구현됨  
- [ ] 아니요 — 헤더가 누락되어 브라우저가 실행 불가능한 파일(예: 이미지)을 스크립트로 실행할 가능성이 있음  

---