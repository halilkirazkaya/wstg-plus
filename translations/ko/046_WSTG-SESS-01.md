## WSTG-SESS-01 — 세션 관리 스키마 테스트 (Testing for Session Management Schema)

세션 관리 스키마 테스트는 애플리케이션이 세션 식별자(Session Identifiers)의 생명 주기와 구조를 어떻게 처리하는지 분석하여, 예측 및 하이재킹(Hijacking) 시도에 대해 충분히 견고한지 확인하는 데 중점을 둡니다. 공격자는 통계적 분석을 수행하기 위해 여러 세션 토큰(Session Tokens)을 수집하며, 이를 통해 패턴, 낮은 엔트로피(Entropy), 또는 예측 가능한 증가치를 찾아 다른 사용자의 유효한 세션을 위조할 수 있습니다. 스키마의 취약점은 주로 세션 고정(Session Fixation) 취약점이나 충분히 무작위적이지 않은 값의 사용으로 나타나며, 이는 일반적으로 HTTP 쿠키, URL 파라미터 또는 사용자 정의 헤더 구현에서 발견됩니다. 세션 스키마를 탈취하는 것은 공격자가 피해자의 기본 인증 정보(Credentials) 없이도 인증된 상태에 완전히 접근할 수 있게 하는 영향력이 큰 공격입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### 세션 식별자가 충분히 복잡하고 무작위적입니까?
- [ ] 예 — 토큰이 통계적 무작위성 테스트를 통과하며(높은 엔트로피), 우회가 **불가능합니다**  
- [ ] 예 — 토큰의 길이는 길지만 예측 가능한 패턴이나 정적인 세그먼트를 포함하고 있습니다  
- [ ] 아니요 — 토큰이 순차적이거나 예측 가능한 타임스탬프(Timestamps)를 기반으로 하며, 엔트로피가 **심각하게 낮습니다** *(Critical)*  

### 세션 식별자가 어디에서 전송되고 저장됩니까?
- [ ] 식별자가 `Secure` 및 `HttpOnly` 쿠키에 저장됩니다 *(가장 안전함)*  
- [ ] 식별자가 쿠키에 저장되지만 `Secure` 또는 `HttpOnly` 플래그가 누락되었습니다  
- [ ] 식별자가 URL 파라미터 또는 `GET` 요청을 통해서만 전송됩니다 *(높은 위험)*  

### 애플리케이션이 인증 성공 시 새로운 세션 식별자를 발급합니까?
- [ ] 예 — 로그인 직후 새로운 고유 세션 ID가 **생성됩니다**  
- [ ] 아니요 — 로그인 전후의 세션 ID가 동일하게 유지됩니다 *(세션 고정 가능)*  

### 재사용 방지를 위해 세션 식별자가 특정 IP 주소나 User-Agent에 결합되어 있습니까?
- [ ] 예 — 클라이언트의 핑거프린트(Fingerprint)가 크게 변경되면 세션이 무효화됩니다  
- [ ] 아니요 — 추가 검증 없이 서로 다른 IP 주소나 기기에서 세션을 재사용할 **수 있습니다**  

### 로그아웃 또는 타임아웃 후에 서버 측에서 세션 식별자가 적절히 무효화됩니까?
- [ ] 예 — 로그아웃 시 서버 측 세션 상태가 **완전히 파기됩니다**  
- [ ] 아니요 — 클라이언트 측 쿠키가 삭제되더라도 서버에서 세션이 유효하게 유지됩니다  
- [ ] 아니요 — 세션이 **만료되지 않거나** 타임아웃(Timeout) 기간이 지나치게 깁니다  

---