## WSTG-ATHZ-03 — 권한 상승(Privilege Escalation) 테스트

권한 상승 (Privilege Escalation)은 공격자가 취약점을 악용하여 더 높은 권한 수준이나 다른 ID를 가진 사용자에게 허용된 리소스 또는 기능에 액세스할 때 발생합니다. 수직적 권한 상승 (Vertical Escalation)에서는 일반 사용자가 관리자 기능에 액세스하려고 시도하는 반면, 수평적 권한 상승 (Horizontal Escalation)은 동일한 권한 수준을 가진 다른 사용자의 데이터에 액세스하는 것을 포함합니다. 이러한 결함은 일반적으로 제대로 구현되지 않은 접근 제어 목록 (ACL), 부적절한 직접 객체 참조 (IDOR), 또는 세션이나 ID 토큰 내의 파라미터 변조 (Parameter Tampering)에서 나타납니다. 공격자의 관점에서는 숫자 ID를 조작하거나, 쿠키 또는 JWT의 역할 기반 파라미터를 수정하거나, 서버 측 권한 검증(Authorization)이 결여된 숨겨진 API 엔드포인트(API Endpoint)를 직접 호출함으로써 이를 달성하는 경우가 많습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### 애플리케이션이 다중 권한 수준 또는 멀티 테넌시(Multi-tenancy)를 구현하고 있습니까?
- [ ] 아니요 — 애플리케이션이 단일 사용자 또는 단일 역할 시스템입니다.  
- [ ] 예 — 다중 역할 또는 테넌트가 정의되어 있으며 권한 검증 테스트가 필요합니다.  

### 수평적 권한 상승(동일 수준 액세스)이 가능합니까?
- [ ] 아니요 — 모든 리소스 식별자 및 세션 소유자에 대해 권한 검증이 적용됩니다.  
- [ ] 예 — IDOR 또는 파라미터 변조를 통해 다른 사용자의 데이터에 액세스할 수 있습니다.  
- [ ] 예 — 데이터 유출은 가능하지만 다른 사용자의 데이터 수정은 불가능합니다.  

### 수직적 권한 상승(낮은 권한에서 높은 권한으로)이 가능합니까?
- [ ] 아니요 — 관리 엔드포인트가 역할 기반 접근 제어(RBAC)를 엄격하게 적용합니다.  
- [ ] 예 — 직접 URL 접속을 통해 낮은 권한의 사용자가 관리 기능에 액세스할 수 있습니다.  
- [ ] 예 — 역할 관련 헤더, 쿠키 또는 JWT 클레임을 조작하여 관리 기능에 액세스할 수 있습니다.  

### 매스 어사인먼트(Mass Assignment) 또는 파라미터 오염(Parameter Pollution)을 통해 사용자 역할이나 권한을 수정할 수 있습니까?
- [ ] 아니요 — 역할 기반 필드는 엄격하게 읽기 전용이며 사용자가 수정할 수 없습니다.  
- [ ] 예 — 프로필 업데이트 또는 회원가입 시 숨겨진 파라미터(예: `role=admin`)를 포함하여 사용자 권한을 상승시킬 수 있습니다.  

### 모든 API 버전 및 HTTP 메서드에 대해 권한 검증이 일관되게 적용됩니까?
- [ ] 예 — 모든 버전 및 메서드에 대해 통제 항목이 일관되게 적용됩니다.  
- [ ] 아니요 — 레거시 API 버전(예: `/v1/`) 또는 특정 HTTP 메서드(예: `PUT`, `DELETE`, `PATCH`)에서 권한 검증을 우회할 수 있습니다.  
- [ ] 아니요 — 관리 기능이 UI에서는 숨겨져 있지만 API 수준에서는 모든 사용자에 대해 활성화되어 있습니다.  

---