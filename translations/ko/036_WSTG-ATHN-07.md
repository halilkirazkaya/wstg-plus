## WSTG-ATHN-07 — 취약한 비밀번호 정책 테스트 (Testing for Weak Password Policy)

취약한 비밀번호 정책 테스트는 계정 생성 및 자격 증명(Credential) 업데이트 중에 애플리케이션이 강제하는 복잡성, 길이 및 엔트로피(Entropy) 요구 사항을 평가하는 것을 포함합니다. 불충분한 정책은 사용자 계정을 사전 공격(Dictionary Attack), 무차별 대입 공격(Brute-force), 그리고 자격 증명 스터핑(Credential Stuffing)에 취약하게 만들어 기밀성을 직접적으로 침해합니다. 이러한 취약점은 일반적으로 등록 엔드포인트(Registration Endpoint), 비밀번호 재설정 워크플로우 및 관리자 사용자 관리 패널에 존재합니다. 공격자 관점에서 관대한 정책은 일반적인 워드리스트(Wordlist)나 패턴 기반 크래킹(Pattern-based Cracking)을 사용하여 자격 증명을 성공적으로 탈취하는 데 필요한 계산 비용과 시간을 크게 줄여줍니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **테스트 상태** | Not Performed |
| **심각도** | Medium / Low* |

> *심각도는 애플리케이션에 자동화된 추측을 방지하기 위한 계정 잠금(Account Lockout) 또는 속도 제한(Rate Limiting) 메커니즘이 부족한 경우 High가 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**도구:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### 애플리케이션에서 최소 비밀번호 길이를 강제합니까?
- [ ] 예 — 최소 길이는 12자 이상입니다 *(가장 안전함)*  
- [ ] 예 — 최소 길이는 8자에서 11자 사이입니다  
- [ ] 예 — 최소 길이는 8자 **미만**입니다  
- [ ] 아니요 — 최소 길이가 강제되지 않습니다  

### 복잡성 요구 사항(대문자, 소문자, 숫자, 특수문자)이 강제됩니까?
- [ ] 예 — 여러 문자 클래스가 필수이며 우회(Bypass)가 **불가능**합니다  
- [ ] 예 — 문자 클래스가 권장되지만 강제되지는 **않습니다**  
- [ ] 아니요 — 복잡성 요구 사항이 적용되지 않습니다  

### 애플리케이션이 흔하거나 취약한 비밀번호 또는 사용자 관련 데이터가 포함된 비밀번호를 차단합니까?
- [ ] 예 — 알려진 취약한 비밀번호(예: "password123") 및 사용자 이름 기반 비밀번호를 사용할 수 **없습니다**  
- [ ] 예 — 흔한 비밀번호는 차단되지만, 사용자 관련 데이터(예: 사용자 이름, 이메일)는 사용될 수 **있습니다**  
- [ ] 아니요 — 길이/복잡성 요구 사항을 충족하는 모든 문자열이 허용됩니다  

### 클라이언트 측(Client-side) 수정을 통해 복잡성 또는 길이 요구 사항을 우회할 수 있습니까?
- [ ] 아니요 — 정책이 **서버 측(Server-side)**에서 엄격하게 강제됩니다  
- [ ] 예 — 정책이 JavaScript를 통해 강제되며, 요청을 가로채서 우회할 수 **있습니다**  

### 구현 세부 정보의 유출 없이 비밀번호 정책이 사용자에게 명확하게 전달됩니까?
- [ ] 예 — 입력 중에 정책이 표시되며 일반적인 실패 메시지를 제공합니다  
- [ ] 예 — 정책이 표시되지만 실패 메시지에 특정 정규 표현식(Regex) 또는 로직의 허점이 드러납니다  
- [ ] 아니요 — 정책이 표시되지 않아 시행착오를 통해 찾아내야 합니다  

---