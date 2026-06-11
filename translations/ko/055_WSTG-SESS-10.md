## WSTG-SESS-10 — JSON 웹 토큰(JWT) 테스트

JSON 웹 토큰 (JSON Web Tokens, JWT)은 무상태 세션 관리(Stateless session management) 및 ID 전파(Identity propagation)를 위해 흔히 사용되지만, 그 보안성은 서명 알고리즘(Signing algorithms)의 올바른 구현과 엄격한 클레임 검증(Claim verification)에 전적으로 의존합니다. 공격자들은 "none" 알고리즘 결함을 악용하거나, 취약한 HS256 비밀키를 브루트포스(Brute-force)하거나, 비대칭 공개키(Asymmetric public key)를 대칭 HMAC 비밀키(Symmetric HMAC secret)로 사용하는 알고리즘 전환 공격(Algorithm-switching attacks)을 통해 인증 우회를 자주 시도합니다. 이러한 취약점은 주로 세션 쿠키나 인증(Authorization) 헤더에서 나타나며, 공격자가 토큰의 암호학적 무결성(Cryptographic integrity)을 손상시키지 않으면서 `role` 또는 `user_id`와 같은 클레임을 수정하여 권한을 상승(Escalate privileges)시키거나 다른 사용자를 사칭(Impersonate)할 수 있게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **테스트 상태** | Not Performed |
| **심각도** | High / Critical |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**도구:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### 서버가 "none" 알고리즘을 허용합니까?
- [ ] 아니요 — 서버가 헤더에서 `alg: none`을 사용하는 토큰을 **거부함**  
- [ ] 예 — 서버가 `alg: none` 및 빈 서명이 포함된 토큰을 **허용함** *(Critical)*  

### JWT 서명이 어떻게 검증되며 브루트포스 공격으로부터 보호됩니까?
- [ ] 예 — 강력한 비대칭 키(RS256/ES256) 또는 높은 엔트로피(Entropy)를 가진 HMAC 비밀키가 사용됨  
- [ ] 예 — HS256이 사용되지만 낮은 엔트로피 또는 기본값으로 인해 비밀키가 **브루트포스될 수 있음**  
- [ ] 아니요 — 서명 검증이 **적용되지 않거나** 알고리즘 전환(RS256에서 HS256으로)을 통해 **우회될 수 있음**  

### 백엔드에서 표준 클레임(exp, nbf, iat)이 엄격하게 강제됩니까?
- [ ] 예 — `exp`(만료)가 존재하며 우회할 수 **없음**  
- [ ] 예 — `exp`가 존재하지만 서버가 검증 중에 이를 강제하지 **않음**  
- [ ] 아니요 — 만료 클레임이 **누락되었거나** 무시되어 무기한 세션 재전송(Replay)이 가능함  

### 구현 시 헤더 기반 키 주입(kid, jku, x5u)을 방지합니까?
- [ ] 예 — 헤더가 살균(Sanitize)되며 오직 신뢰할 수 있는 내부 키/소스만 **허용됨**  
- [ ] 아니요 — `kid`(Key ID) 헤더가 디렉토리 탐색(Directory traversal) 또는 SQL Injection에 취약하여 알려진 파일을 가리킬 수 있음  
- [ ] 아니요 — `jku` 또는 `x5u` 헤더가 공격자가 제어하는 URL을 가리켜 악성 키를 로드할 수 **있음**  

---