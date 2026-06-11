## WSTG-CLNT-07 — 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)

교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)는 서버가 다른 출처(Origin)로부터의 리소스 접근을 명시적으로 허용하여 동일 출처 정책 (Same-Origin Policy, SOP)을 완화하는 브라우저 기반 메커니즘입니다. 애플리케이션이 임의의 출처를 과도하게 신뢰할 때 설정 오류가 발생하며, 이는 주로 응답 헤더의 `Access-Control-Allow-Origin`에 `Origin` 헤더를 그대로 반영(Reflect)하거나 잘못 구현된 정규식(Regex) 화이트리스트(Whitelist)를 사용할 때 발생합니다. 공격자의 관점에서, 허용적인 CORS 정책은 공격자가 제어하는 도메인에 악성 스크립트를 호스팅하고 취약한 애플리케이션에 인증된 요청을 보내도록 유도함으로써 악용될 수 있습니다. 이를 통해 공격자는 응답 본문에서 직접 CSRF 토큰이나 개인정보 (PII)와 같은 민감한 데이터를 탈취할 수 있습니다. 이 취약점은 인증된 세션을 처리하고 비공개 정보를 반환하는 API 엔드포인트에서 가장 치명적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**도구:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### 애플리케이션이 인증된 엔드포인트에 CORS 헤더를 구현하고 있습니까?
- [ ] 아니요 — CORS가 **활성화되지 않음**; 브라우저는 엄격한 동일 출처 정책 (SOP)을 기본으로 적용함  
- [ ] 예 — CORS 헤더가 존재하며 엄격한 **화이트리스트**로 제한됨  
- [ ] 예 — CORS 헤더가 존재하지만 **허용적(Permissive)**으로 설정됨  

### 서버가 `Access-Control-Allow-Origin` (ACAO) 헤더를 어떻게 처리합니까?
- [ ] ACAO가 특정 **신뢰된** 정적 도메인으로 설정됨  
- [ ] ACAO가 `*` (와일드카드)로 설정되었으며 자격 증명(Credentials)을 **보낼 수 없음**  
- [ ] ACAO가 `*` (와일드카드)로 설정되었으며 자격 증명을 **보낼 수 있음** *(참고: 대부분의 브라우저는 이를 차단함)*  
- [ ] ACAO가 요청의 `Origin` 헤더 값을 **동적으로 반영함**  

### `Access-Control-Allow-Credentials` (ACAC) 헤더가 `true`로 설정되어 있습니까?
- [ ] 아니요 — ACAC가 **없거나** `false`로 설정됨  
- [ ] 예 — ACAC가 `true`로 설정되었으나, ACAO가 신뢰된 출처로 **제한됨**  
- [ ] 예 — ACAC가 `true`로 설정되었으며 ACAO가 임의의 출처를 **반영함** *(치명적)*  

### 서브도메인 또는 null 출처 조작을 통해 출처 화이트리스트를 우회할 수 있습니까?
- [ ] 아니요 — 화이트리스트가 엄격하게 강제되며 **우회할 수 없음**  
- [ ] 예 — 화이트리스트가 타겟의 **모든** 서브도메인을 허용함 (예: `attacker.target.com`)  
- [ ] 예 — 화이트리스트가 샌드박스 처리된 iframe을 통한 `null` 출처를 허용함  
- [ ] 예 — 화이트리스트가 정규식(Regex) 우회에 취약함 (예: `target.com.attacker.com` 또는 `target.com-safe.com`)  

### CORS 설정이 민감한 데이터 탈취(Exfiltration)를 허용합니까?
- [ ] 아니요 — 노출된 엔드포인트에 민감한 정보나 세션 특정 데이터가 **포함되지 않음**  
- [ ] 예 — 인증된 엔드포인트가 권한이 없는 출처에서 읽을 수 있는 개인정보 (PII), 자격 증명 또는 CSRF 토큰을 반환함  

---