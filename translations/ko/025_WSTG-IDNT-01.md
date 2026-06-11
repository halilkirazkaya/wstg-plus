## WSTG-IDNT-01 — 역할 정의 테스트 (Test Role Definitions)

역할 정의 테스트는 애플리케이션 내에 정의된 다양한 사용자 역할과 권한 수준을 식별하고 문서화하여, 이들이 논리적으로 분리되어 있으며 최소 권한의 원칙 (Principle of Least Privilege)을 준수하는지 확인하는 작업을 포함합니다. 공격자는 애플리케이션을 분석하여 고권한 역할과 일반 사용자 역할을 매핑하며, 권한 경계의 모호함이나 문서화되지 않은 관리 기능을 탐색합니다. 이러한 역할을 식별하는 것은 권한 상승 (Privilege Escalation) 경로를 발견하는 첫 번째 단계입니다. 역할 정의의 불일치는 종종 민감한 데이터나 관리 기능에 대한 무단 액세스로 이어지기 때문입니다. 이 단계는 일반적으로 초기 정찰 (Reconnaissance) 및 비즈니스 로직 매핑 (Business Logic Mapping) 중에 발생하며, 사용자 프로필, 관리 대시보드 및 API 권한 구조 (API Permission Structures)에 집중합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도 (Severity)** | 낮음 (Low) / 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### 애플리케이션 또는 해당 문서에 역할이 명확하게 정의되고 문서화되어 있습니까?
- [ ] 예 — 역할이 완전히 문서화되어 있으며 **최소 권한의 원칙**을 준수함  
- [ ] 예 — 역할이 문서화되어 있으나 **중복된 권한**이 포함되어 있음  
- [ ] 아니요 — 공식적인 역할 문서 또는 정의가 존재하지 않음  

### 관리 기능과 일반 사용자 기능이 명확하게 분리되어 있습니까?
- [ ] 예 — 관리 기능이 일반 사용자 뷰로부터 **완전히 격리**되어 있음  
- [ ] 예 — 분리가 되어 있으나 관리 엔드포인트가 **예측 가능하거나 추측 가능함**  
- [ ] 아니요 — 동일한 인터페이스 내에 관리 기능과 일반 기능이 **혼재**되어 있음  

### 애플리케이션이 역할 기반 액세스 제어 (RBAC)를 위한 중앙 집중식 메커니즘을 사용합니까?
- [ ] 예 — 중앙 집중식 역할 기반 액세스 제어 (RBAC) 또는 속성 기반 액세스 제어 (ABAC)가 **구현**되어 있으며 일관되게 적용됨  
- [ ] 예 — 분산된 제어 기능이 존재하지만 모듈 전체에 **비일관적으로 적용됨**  
- [ ] 아니요 — 액세스 제어 로직이 개별 페이지나 구성 요소에 **하드코딩 (Hardcoded)**되어 있음  

### 시스템에 문서화되지 않거나 숨겨진 역할이 존재합니까?
- [ ] 아니요 — 발견된 모든 역할이 문서화된 기능과 일치함  
- [ ] 예 — 숨겨진 역할 또는 "섀도우(shadow)" 역할(예: `super_admin`, `support`, `test`)이 **존재함**  

### 낮은 권한의 사용자가 다른 사용자의 권한이나 역할을 열거 (Enumerate)할 수 있습니까?
- [ ] 아니요 — 사용자가 다른 엔티티의 역할/권한을 조회하거나 **열거할 수 없음**  
- [ ] 예 — 사용자가 프로필 페이지, 메타데이터 또는 API 응답을 통해 역할을 **열거할 수 있음** *(중간)*  

---