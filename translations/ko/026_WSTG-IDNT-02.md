## WSTG-IDNT-02 — 사용자 등록 프로세스 테스트

사용자 등록 프로세스 테스트는 새로운 식별 정보(Identity)가 생성되는 워크플로우를 평가하여, 무단 액세스나 자동화된 익스플로잇(Exploit)에 악용되지 않도록 보장하는 작업을 포함합니다. 이 프로세스의 취약점은 주로 신원 확인(이메일/SMS)의 부재, 봇(Bot)을 통한 대량 계정 생성 가능성, 또는 기존 사용자 이름을 노출하는 상세한 오류 메시지를 통한 정보 노출로 나타납니다. 공격자들은 이러한 결함을 악용하여 사용자 열거(User Enumeration)를 수행하거나, 대규모 스팸 캠페인을 진행하거나, 계정 생성 단계에서 등록 파라미터를 조작하여 권한 상승(Privilege Escalation)을 시도합니다. 이 테스트는 애플리케이션의 무결성을 보호하고 공격자가 시스템에 허위 또는 악성 계정을 대량으로 생성하는 것을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### 새로운 계정이 활성화되기 전에 신원 확인이 필요합니까?
- [ ] 예 — 등록 시 이메일 또는 SMS를 통해 전송되는 고유하고 유효 시간이 제한된 토큰이 필요함  
- [ ] 예 — 확인이 필요하지만 토큰이 취약하거나 예측 가능함  
- [ ] 아니요 — 확인 단계 없이 계정이 즉시 활성화됨  

### 등록 프로세스가 사용자 열거(User Enumeration)를 방지합니까?
- [ ] 예 — 애플리케이션이 신규 및 기존 이메일/사용자 이름에 대해 동일한 응답을 반환함  
- [ ] 아니요 — 오류 메시지(예: "이미 사용 중인 이메일입니다")를 통해 공격자가 등록된 사용자를 식별할 수 있음  
- [ ] 아니요 — 서버 응답의 시간 차이로 인해 사용자 존재 여부가 노출됨  

### 대량 등록을 방지하기 위한 자동화 방지 제어(Anti-automation controls)가 구현되어 있습니까?
- [ ] 예 — CAPTCHA 또는 작업 증명(Proof-of-work) 메커니즘이 존재하며 효과적임  
- [ ] 예 — 제어 수단이 존재하지만 API 엔드포인트 또는 헤더 조작을 통해 우회가 가능함  
- [ ] 아니요 — 등록 엔드포인트에 속도 제한(Rate Limiting) 또는 CAPTCHA가 적용되지 않음  

### 권한 상승을 위해 등록 파라미터를 조작할 수 있습니까?
- [ ] 아니요 — 사용자 역할이 서버 측 로직에 의해 엄격하게 할당됨  
- [ ] 예 — 요청 내의 `role`, `admin` 또는 `group_id`와 같은 파라미터를 수정하여 더 높은 권한을 획득할 수 있음  

### 등록 단계에서 비밀번호 정책이 강제됩니까?
- [ ] 예 — 강력한 비밀번호 요구 사항이 서버 측에서 강제됨  
- [ ] 아니요 — 취약한 비밀번호(예: "password123")가 시스템에서 허용됨  

---