## WSTG-SESS-07 — 세션 타임아웃 테스트 (Testing Session Timeout)

세션 타임아웃 테스트(Session timeout testing)는 애플리케이션이 미리 정의된 비활성 기간 또는 총 지속 시간 이후에 사용자의 세션을 효과적으로 종료하는지 평가합니다. 이 통제 항목은 특히 공용 워크스테이션이나 공격자가 네트워크 가로채기 또는 교차 사이트 스크립팅(XSS)을 통해 세션 식별자(Session identifier)를 탈취하는 시나리오에서 세션 하이재킹(Session Hijacking) 위험을 완화하는 데 필수적입니다. 모의해킹 전문가(Pentester)는 일정 기간 비활성 상태일 때 발생하는 유휴 타임아웃(Idle timeout)과 활동 여부와 관계없이 세션의 전체 수명을 제한하는 절대 타임아웃(Absolute timeout)을 모두 분석합니다. 공격자의 관점에서 무기한 또는 지나치게 긴 세션은 비인가 접근을 유지하고 재인증(Re-authentication)의 필요성을 우회할 수 있는 연장된 기회의 창을 제공합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### 애플리케이션에 유휴 세션 타임아웃(Idle session timeout)이 구현되어 있습니까?
- [ ] 예 — 일정 기간(예: 15-30분) 비활성 후 세션이 만료됨  
- [ ] 예 — 세션이 만료되지만 타임아웃 기간이 지나치게 김 (예: 24시간 초과)  
- [ ] 아니요 — 비활성 중에도 세션이 무기한 활성 상태로 유지됨  

### 세션 타임아웃이 서버 측(Server-side)에서 강제됩니까?
- [ ] 예 — 클라이언트 측 상태와 관계없이 서버가 만료된 토큰의 요청을 거부함  
- [ ] 아니요 — 타임아웃이 클라이언트 측 자바스크립트(JavaScript) 또는 meta-refresh를 통해서만 강제됨  

### 애플리케이션에 절대 세션 타임아웃(Absolute session timeout)이 구현되어 있습니까?
- [ ] 예 — 활동 여부와 관계없이 고정된 최대 지속 시간 이후 세션이 종료됨  
- [ ] 아니요 — 지속적인 활동이 있는 한 세션이 무기한 연장될 수 있음  

### 타임아웃 시 서버에서 세션 식별자가 무효화됩니까?
- [ ] 예 — 타임아웃 임계값에 도달하면 세션 토큰을 재사용할 수 없음  
- [ ] 아니요 — 클라이언트가 로그인 페이지로 리다이렉트된 후에도 서버에서 세션 토큰이 여전히 유효함  

### 자동화된 하트비트(Heartbeat) 요청을 사용하여 세션을 무기한 유지할 수 있습니까?
- [ ] 아니요 — 절대 타임아웃 또는 보조 통제 항목이 무한한 세션 연장을 방지함  
- [ ] 예 — 주기적인 백그라운드 요청(예: AJAX)이 세션을 무기한 유지할 수 있음  

---