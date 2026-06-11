## WSTG-SESS-11 — 동시 세션 테스트 (Testing for Concurrent Sessions)

동시 세션 테스트는 애플리케이션이 단일 사용자 계정에 대해 서로 다른 브라우저, 장치 또는 IP 주소에서 여러 개의 동시 활성 세션(Concurrent Sessions)을 유지할 수 있도록 허용하는지 여부를 평가합니다. 동시 세션을 제한하지 못하면 공격자가 탈취한 세션 토큰(Session Tokens)이나 유출된 자격 증명(Compromised Credentials)을 사용하여 기존 세션을 종료하거나 정당한 사용자에게 알리지 않고도 공격을 수행할 수 있는 기회의 창이 넓어집니다. 이러한 동작은 일반적으로 여러 소스에서 인증을 시도하고 세션 안정성을 모니터링하여 평가하며, 이는 종종 세션 관리 로직의 취약성이나 보안 중심의 비즈니스 규칙 부재를 드러냅니다. 공격자의 관점에서 동시성 제어(Concurrency Control)의 부재는 초기 침해 후 은밀하게 지속성(Persistence)을 유지할 수 있게 하며 병렬 자동화 공격을 용이하게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 낮음 (Low) / 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### 애플리케이션 정책에 의해 동시 세션이 제한됩니까?
- [ ] 예 — 사용자당 단 하나의 세션만 **허용됩니다**. 새로운 로그인은 기존 로그인을 무효화합니다 *(가장 안전함)*  
- [ ] 예 — 고정된 수의 동시 세션이 **강제 적용되며** 이를 초과할 수 없습니다  
- [ ] 아니요 — 제한 없는 수의 동시 세션이 **가능합니다**  

### 세션 제한에 도달했을 때 애플리케이션은 어떻게 응답합니까?
- [ ] 가장 오래된 활성 세션이 **자동으로 종료됩니다** (세션 고정(Session Fixation)/하이재킹(Hijacking) 완화)  
- [ ] 새로운 세션이 승인되기 전에 사용자에게 기존 세션을 종료하도록 **요청합니다**  
- [ ] 다른 장치에서 수동 로그아웃이 발생할 때까지 새로운 로그인 시도가 **차단됩니다**  
- [ ] 아무런 조치도 취하지 않습니다 — 여러 세션이 동시에 **활성화된** 상태로 유지됩니다  

### 애플리케이션이 사용자를 위한 세션 관리 인터페이스를 제공합니까?
- [ ] 예 — 사용자가 활성 세션(IP, 장치, 시간)을 확인하고 원격으로 **종료할 수 있습니다**  
- [ ] 예 — 사용자가 활성 세션을 확인할 수 있지만 원격으로 **종료할 수는 없습니다**  
- [ ] 아니요 — 사용자는 자신의 계정과 연결된 다른 활성 세션을 확인하거나 관리할 수 **없습니다**  

### 동시 로그인 활동이 감지될 때 사용자에게 알림이 전송됩니까?
- [ ] 예 — 새로운 장치나 위치에서의 로그인에 대해 알림(이메일, SMS 또는 앱 내 알림)이 **트리거됩니다**  
- [ ] 아니요 — 동시 세션이 수립될 때 아무런 알림도 **제공되지 않습니다**  

---