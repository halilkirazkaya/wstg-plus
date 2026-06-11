## WSTG-ATHN-03 — 취약한 계정 잠금 메커니즘 테스트 (Testing for Weak Lock Out Mechanism)

계정 잠금 메커니즘(Account Lockout Mechanism)은 지정된 횟수의 인증 시도 실패 후 계정을 비활성화함으로써 무차별 대입(Brute Force) 및 크리덴셜 스터핑(Credential Stuffing) 공격을 완화하도록 설계된 중요한 심층 방어(Defense-in-depth) 통제 항목입니다. 강력한 잠금 정책이 없으면 공격자는 자동화된 도구를 사용하여 여러 계정에 대해 체계적으로 비밀번호를 추측(패스워드 스프레이, Password Spraying)하거나 단일 고가치 계정이 성공할 때까지 공격할 수 있습니다. 이러한 취약점은 일반적으로 로그인 포털, 비밀번호 재설정 양식 및 속도 제한(Rate Limiting) 또는 계정 수준의 보호가 부족한 API 엔드포인트(API Endpoints)에서 나타납니다. 공격자의 관점에서 취약하거나 없는 잠금 메커니즘은 무한한 비밀번호 시도를 허용하는 반면, 지나치게 공격적인 잠금 메커니즘은 의도적으로 계정을 잠금으로써 정당한 사용자에 대한 분산 서비스 거부(Denial of Service, DoS)를 유발하는 데 악용될 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *애플리케이션이 민감한 개인 식별 정보(Personally Identifiable Information, PII), 금융 데이터를 처리하거나 보조 통제 수단 없이 관리자 계정이 무차별 대입에 취약한 경우 위험도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### 정해진 횟수의 인증 시도 실패 후 계정 잠금 메커니즘이 강제 적용됩니까?
- [ ] 예 — 정의된 합리적인 임계값(예: 5-10회 시도) 후에 계정이 잠깁니다.
- [ ] 예 — 계정이 잠기지만 시도 횟수가 지나치게 높습니다.
- [ ] 아니요 — 계정을 잠글 수 없으며 무한한 무차별 대입을 허용합니다.

### 일반적인 우회 기법을 사용하여 잠금 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 잠금은 서버 측에서 강제 적용되며 클라이언트 측 변경 사항의 영향을 받지 않습니다.
- [ ] 예 — IP 주소를 교체하거나 프록시 풀을 사용하여 잠금을 우회할 수 있습니다.
- [ ] 예 — `X-Forwarded-For` 또는 `User-Agent`와 같은 HTTP 헤더를 조작하여 잠금을 우회할 수 있습니다.
- [ ] 예 — 쿠키를 삭제하거나 새 세션을 시작하여 잠금을 우회할 수 있습니다.

### 잠금 메커니즘이 계정 복구 또는 잠금 해제 경로를 제공합니까?
- [ ] 예 — 일정 기간이 지난 후 또는 이메일 인증을 통해 계정이 자동으로 잠금 해제됩니다.
- [ ] 예 — 계정 잠금을 해제하려면 관리자의 개입이 필요합니다.
- [ ] 아니요 — 명확한 복구 경로 없이 잠금이 영구적이어서 DoS 위험이 증가합니다.

### 애플리케이션이 잠금 발생 시 유효한 사용자 이름과 유효하지 않은 사용자 이름을 구분합니까?
- [ ] 아니요 — 기존 사용자와 존재하지 않는 사용자 모두에 대해 애플리케이션 응답이 동일합니다.
- [ ] 예 — 애플리케이션이 계정 잠금 여부를 드러내어 사용자 이름 열거(Username Enumeration)를 허용합니다.

### 잠금 메커니즘이 서비스 거부(DoS) 공격에 취약합니까?
- [ ] 아니요 — CAPTCHA 또는 IP 기반 속도 제한과 같은 통제 수단이 대규모 계정 잠금을 방지합니다.
- [ ] 예 — 공격자가 잘못된 비밀번호를 제공하여 알려진 모든 사용자를 체계적으로 잠글 수 있습니다.

---