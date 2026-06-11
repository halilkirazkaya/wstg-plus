## WSTG-ATHZ-05 — OAuth 취약점 테스트 (Testing for OAuth Weaknesses)

OAuth 취약점은 OAuth 2.0 또는 OpenID Connect (OIDC) 프로토콜 구현 시 발생하는 다양한 결함을 포괄하며, 이는 흔히 완전한 계정 탈취 (Account Takeover)나 미인가 데이터 접근으로 이어집니다. 이러한 취약점은 일반적으로 인가 흐름 (Authorization Flow) 중에 나타나며, 특히 `redirect_uri`의 검증, `state` 파라미터의 엔트로피 (Entropy) 및 검증, 그리고 액세스 (Access) 또는 ID 토큰의 보안 처리에 관한 것입니다. 공격자는 인가 코드를 가로채거나, 오픈 리다이렉트 (Open Redirects)를 통해 공격자가 제어하는 도메인으로 토큰을 리다이렉트하거나, 사이트 간 요청 위조 (CSRF)를 수행하여 피해자의 세션을 공격자의 계정에 연결함으로써 이를 악용합니다. OAuth는 종종 주요 인증 메커니즘으로 사용되기 때문에, 단 하나의 설정 오류만으로도 전체 사용자 식별 시스템의 무결성이 손상될 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) / 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**도구:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### CSRF 방지를 위해 `state` 파라미터가 구현 및 검증되었는가?
- [ ] 예 — `state`가 필수이며, 세션당 고유하고 서버에서 엄격하게 검증됨  
- [ ] 예 — `state`가 존재하지만 여러 세션에 걸쳐 고정되어 있거나 예측 가능함  
- [ ] 예 — `state`가 존재하지만 애플리케이션이 콜백 시 이를 검증하지 않음  
- [ ] 아니요 — 인가 요청에서 `state` 파라미터가 사용되지 않음 *(심각)*  

### `redirect_uri`가 화이트리스트 (Whitelist)를 기준으로 엄격하게 검증되는가?
- [ ] 예 — 미리 등록된 화이트리스트와 정확히 일치하는 경우에만 허용됨  
- [ ] 예 — 검증이 적용되지만 경로 탐색 (Path Traversal) 또는 서브도메인 조작을 통해 우회가 가능함  
- [ ] 예 — 검증이 적용되지만 파라미터 오염 (Parameter Pollution) 또는 프래그먼트 주입 (Fragment Injection)을 통해 우회가 가능함  
- [ ] 아니요 — 애플리케이션이 `redirect_uri` 파라미터에 임의의 URL을 허용함 *(심각)*  

### 인가 흐름을 변경하기 위해 `response_type`을 조작할 수 있는가?
- [ ] 아니요 — 애플리케이션이 의도된 흐름(예: `code`)만 수락하고 나머지는 거부함  
- [ ] 예 — `code`에서 `token` (암시적 흐름, Implicit flow)으로 전환이 가능하며 URL에서 토큰이 유출됨  
- [ ] 예 — 하이브리드 흐름(Hybrid flows) 또는 허가되지 않은 `response_type` 값이 활성화되어 처리됨  

### Referer 헤더 또는 브라우저 기록을 통해 액세스 토큰이나 인가 코드가 유출되는가?
- [ ] 아니요 — 토큰/코드가 안전하게 처리되며 민감한 페이지에서 적절한 `Referrer-Policy`를 사용함  
- [ ] 예 — `Referer` 헤더를 통해 제3자 도메인으로 토큰/코드가 유출됨  
- [ ] 예 — 안전하지 않은 캐싱 또는 GET 요청으로 인해 브라우저 기록에서 토큰/코드가 확인됨  

### 애플리케이션이 OAuth 스코프 (Scopes)에 대해 "최소 권한 (Least Privilege)" 원칙을 강제하는가?
- [ ] 예 — 애플리케이션이 기능 수행에 필요한 최소한의 스코프만 요청함  
- [ ] 아니요 — 애플리케이션이 기본적으로 과도하거나 관리자 권한의 스코프를 요청함  
- [ ] 아니요 — 사용자가 동의 단계에서 특정 스코프를 검토하거나 제외할 수 없음  

---