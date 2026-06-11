## WSTG-CONF-12 — 콘텐츠 보안 정책 (Content Security Policy, CSP)

콘텐츠 보안 정책 (Content Security Policy, CSP)은 크로스 사이트 스크립팅 (Cross-Site Scripting, XSS), 클릭재킹 (Clickjacking) 및 데이터 주입 (Data Injection) 공격과 같은 위험을 완화하기 위해 HTTP 응답 헤더를 통해 구현되는 중요한 심층 방어 (Defense-in-depth) 메커니즘입니다. 허용되는 동적 리소스를 정의함으로써, 잘 구성된 CSP는 공격자가 승인되지 않은 스크립트를 실행하거나 민감한 데이터를 외부 도메인으로 유출 (Exfiltrate)하는 능력을 제한합니다. 모의해킹 전문가 (Pentester)는 과도하게 허용적인 지시문, `'unsafe-inline'`과 같은 안전하지 않은 키워드의 사용, 우회 (Bypass)를 용이하게 할 수 있는 안전하지 않은 CDN에 대한 의존성 등을 기준으로 정책을 평가합니다. 약하거나 누락된 CSP가 직접적으로 취약점을 만드는 것은 아니지만, 보조 보호 계층을 제공하지 못함으로써 주입 공격 성공 시의 영향도를 크게 증가시킵니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 낮음 (Low) / 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**도구:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### 애플리케이션 응답에 콘텐츠 보안 정책 (CSP) 헤더가 존재하는가?
- [ ] 예 — `Content-Security-Policy` 헤더가 **존재**하며 **적용(Enforced)** 중임  
- [ ] 예 — 테스트를 위해 `Content-Security-Policy-Report-Only`가 **존재**함  
- [ ] 아니요 — 응답에 CSP 헤더가 **존재하지 않음**  

### `script-src` 또는 `default-src` 지시문이 적절히 제한되어 있는가?
- [ ] 예 — 지시문이 엄격한 화이트리스트, 넌스 (Nonce) 또는 해시 (Hash)를 사용하며 우회가 **불가능함**  
- [ ] 예 — 지시문이 **적절히 구성**되었으나, 화이트리스트에 우회 가능한 것으로 알려진 CDN이 포함되어 있음  
- [ ] 아니요 — 지시문에 와일드카드 (`*`) 또는 `data:` 스킴을 사용하여 우회가 **가능함**  

### 정책이 안전하지 않은 인라인 스크립트 또는 eval의 실행을 허용하는가?
- [ ] 아니요 — `'unsafe-inline'` 및 `'unsafe-eval'`이 **존재하지 않음**  
- [ ] 예 — `'unsafe-inline'`이 **존재**하지만 `nonce` 또는 `hash`에 의해 보호됨  
- [ ] 예 — `'unsafe-inline'` 또는 `'unsafe-eval'`이 추가적인 보호 조치 없이 **활성화**되어 있음 *(Critical)*  

### 애플리케이션이 `frame-ancestors` 지시문을 통해 클릭재킹으로부터 보호되는가?
- [ ] 예 — `frame-ancestors`가 `'none'` 또는 `'self'`로 설정됨  
- [ ] 예 — `frame-ancestors`가 **활성화**되어 있으며 신뢰할 수 있는 특정 제3자 도메인을 허용함  
- [ ] 아니요 — `frame-ancestors`가 **누락**되었으며, 레거시 `X-Frame-Options`에 의존하거나 보호 조치가 없음  

### CSP 위반 사항을 보고하는 메커니즘이 존재하는가?
- [ ] 예 — `report-uri` 또는 `report-to`가 **구성**되어 있으며 활성화 상태임  
- [ ] 아니요 — 위반 보고가 **비활성화**되어 있거나 **구성되지 않음**  

---