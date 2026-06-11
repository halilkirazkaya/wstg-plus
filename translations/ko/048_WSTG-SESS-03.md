## WSTG-SESS-03 — 세션 고정 (Session Fixation)

세션 고정(Session Fixation)은 사용자가 성공적으로 인증된 후에도 애플리케이션이 세션 식별자(Session Identifier)를 무효화하거나 갱신(Rotate)하지 못할 때 발생하며, 공격자가 피해자에게 알려진 세션 토큰(Session Token)을 강제로 사용하게 할 수 있습니다. 애플리케이션이 로그인 후에도 인증 전의 세션 ID를 유지하는 경우, 공격자는 특정 세션 ID가 포함된 특수하게 제작된 링크를 피해자에게 제공하고 이후 인증된 세션을 탈취(Hijack)할 수 있습니다. 이 취약점은 주로 로그인 양식이나 URL 파라미터 및 쿠키를 통해 수락되는 세션 식별자에서 나타납니다. 공격자 관점에서 이 익스플로잇(Exploit)은 악성 링크나 헤더 주입(Header Injection) 취약점을 통해 세션을 "고정"하고 피해자가 자격 증명(Credentials)을 입력하기를 기다리는 방식이며, 이는 실제 토큰을 훔칠 필요 없이 인증을 우회(Bypass)할 수 있게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### 성공적인 인증 후 세션 식별자가 변경됩니까?
- [ ] 예 — 새로운 세션 식별자가 **발급되며** 기존 식별자는 무효화됩니다 *(가장 안전)*  
- [ ] 예 — 새로운 세션 식별자가 **발급되지만** 기존 식별자가 **유효하게 유지됩니다**  
- [ ] 아니요 — 인증 전후에 세션 식별자가 **동일하게 유지됩니다** *(치명적)*  

### 애플리케이션이 URL 파라미터를 통해 제공된 세션 식별자를 수락합니까?
- [ ] 아니요 — 세션 ID는 **쿠키를 통해서만** 관리됩니다  
- [ ] 예 — URL에서 세션 ID를 수락하지만 로그인 시 **무시되거나 갱신**됩니다  
- [ ] 예 — URL의 세션 ID가 **수락되며** 인증 후에도 **지속됩니다**  

### 공격자가 정의한 세션 ID를 애플리케이션에 강제할 수 있습니까?
- [ ] 아니요 — 애플리케이션은 사용자가 제공한 유효하지 않거나 존재하지 않는 세션 ID를 **거부합니다**  
- [ ] 예 — 애플리케이션은 쿠키에 제공된 임의의 ID를 사용하여 세션을 **수락하고 초기화합니다**  
- [ ] 예 — 애플리케이션은 URL 파라미터에 제공된 임의의 ID를 사용하여 세션을 **수락하고 초기화합니다**  

### 인증 전 세션이 올바르게 종료됩니까?
- [ ] 예 — 로그인 이벤트 발생 시 익명 세션이 **완전히 파괴됩니다**  
- [ ] 아니요 — 익명 세션이 **파괴되지 않아** 잠재적인 세션 하베스팅(Session Harvesting)이 가능합니다  

### 세션 쿠키가 클라이언트 측 주입으로부터 보호됩니까?
- [ ] 예 — 스크립트 기반의 고정을 방지하기 위해 `HttpOnly` 및 `Secure` 플래그가 **적용되어 있습니다**  
- [ ] 아니요 — 플래그가 **적용되지 않아** 크로스 사이트 스크립팅(Cross-Site Scripting, XSS)을 통해 세션 ID를 설정할 수 있습니다  

---