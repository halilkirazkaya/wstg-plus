## WSTG-CLNT-12 — 브라우저 스토리지 테스트 (Test Browser Storage)

브라우저 스토리지(Browser Storage) 테스트는 애플리케이션이 데이터를 유지하기 위해 로컬 스토리지(LocalStorage), 세션 스토리지(SessionStorage), IndexedDB 및 WebSQL과 같은 클라이언트 측 메커니즘을 어떻게 활용하는지 분석하는 과정을 포함합니다. 개인식별정보(PII), 인증 토큰(Authentication Tokens) 또는 세션 식별자(Session Identifiers)를 포함하여 안전하지 않게 저장된 민감한 정보는 공격자가 교차 사이트 스크립팅(XSS)을 수행하거나 사용자의 장치에 물리적으로 접근할 경우 노출될 수 있습니다. 모의해킹 전문가(Pentester)는 데이터가 평문(Cleartext)으로 저장되는지, 로그아웃 후에도 접근 가능한지, 그리고 데이터 유출을 방지하기 위해 스토리지 생명주기가 적절히 관리되는지 확인해야 합니다. 공격자 관점에서 이러한 스토리지 메커니즘은 세션 하이재킹(Session Hijacking)이나 장기적인 계정 지속성(Account Persistence)을 가능하게 하는 상태 유지 정보를 탈취(Exfiltrating)하기 위한 매력적인 대상입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High)* |

> *세션 토큰이나 민감한 PII가 LocalStorage에 저장되어 있고 XSS를 통해 접근 가능한 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### 애플리케이션이 브라우저 스토리지에 민감한 정보를 저장합니까?
- [ ] 아니오 — 애플리케이션이 브라우저 스토리지를 사용하지 않거나 민감하지 않은 UI 상태만 저장함  
- [ ] 예 — 민감한 데이터가 저장되지만 강력한 클라이언트 측 암호화 기술을 사용하여 암호화됨  
- [ ] 예 — 민감한 데이터(토큰, PII, 비밀 정보)가 **평문(Cleartext)**으로 저장됨 *(중간)*  

### 저장된 데이터가 권한이 없는 스크립트에서 접근 가능합니까 (XSS 위험)?
- [ ] 아니오 — 데이터가 HttpOnly 쿠키(브라우저 스토리지가 아님)에 저장되거나 스토리지가 사용되지 않음  
- [ ] 예 — LocalStorage/SessionStorage가 사용되어, 임의의 XSS 페이로드(Payload)를 통해 데이터에 **완전한 접근**이 가능함 *(높음)*  

### 사용자 로그아웃 시 브라우저 스토리지가 적절히 삭제됩니까?
- [ ] 예 — 로그아웃 과정에서 모든 애플리케이션 관련 스토리지가 **명시적으로 삭제(Cleared)**됨  
- [ ] 아니오 — 로그아웃 후에도 스토리지가 유지되지만, 민감한 세션 데이터를 포함하지 않음  
- [ ] 아니오 — 세션이 종료된 후에도 인증 토큰이나 PII가 스토리지에 **유지**됨 *(중간)*  

### 애플리케이션이 대규모/민감한 데이터 세트를 위해 IndexedDB 또는 WebSQL을 사용합니까?
- [ ] 아니오 — 이러한 스토리지 메커니즘이 사용되지 않음  
- [ ] 예 — 데이터가 저장되며 DOM에 렌더링되기 전에 **적절히 새니타이징(Sanitized)**됨  
- [ ] 예 — 민감한 데이터 세트가 IndexedDB/WebSQL 구조 내에 **평문**으로 저장됨  

### 저장된 데이터의 무결성(Integrity)을 조작하여 애플리케이션 로직에 영향을 미칠 수 있습니까?
- [ ] 아니오 — 애플리케이션이 스토리지를 처리하기 전에 데이터를 검증하거나 서명함  
- [ ] 예 — 스토리지 값(예: 사용자 역할, 플래그)을 수정하는 것이 **가능**하며, 그 결과 애플리케이션 동작이 변경됨  

---