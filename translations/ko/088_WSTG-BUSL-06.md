## WSTG-BUSL-06 — 워크플로 우회 테스트 (Testing for the Circumvention of Work Flows)

워크플로 우회(Circumvention of Work Flows)는 다단계 프로세스 내에서 의도된 순서나 전제 조건을 건너뛰기 위해 애플리케이션 로직을 조작하는 행위를 포함합니다. 공격자는 결제 확인, 관리자 승인 또는 다요소 인증(MFA) 완료 화면과 같은 민감한 엔드포인트(Endpoint)를 식별하고, 필수 중간 단계를 건너뛰어 해당 단계에 직접 접근을 시도합니다. 이 취약점은 서버 측 로직이 견고한 상태 머신(State Machine)을 유지하지 못하고, 사용자가 UI 중심의 경로를 따를 것이라는 가정에만 의존할 때 발생합니다. 성공적인 공격은 미승인 트랜잭션, 권한 상승(Privilege Escalation), 신원 확인 메커니즘 우회 등 치명적인 비즈니스 로직 결함으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **테스트 상태** | 미수행 |
| **심각도** | 중간 / 높음* |

> *우회를 통해 미승인 금융 트랜잭션, 권한 상승 또는 관리자 접근이 가능한 경우 심각도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### 애플리케이션에 다단계 비즈니스 프로세스가 포함되어 있습니까?
- [ ] 아니오 — 애플리케이션이 단일 요청 작업으로만 구성되어 있음  
- [ ] 예 — 다단계 프로세스(예: 장바구니, 위저드 폼, 회원가입 등)가 존재함  

### 단계별 순서가 서버에 의해 엄격하게 강제됩니까?
- [ ] 예 — 서버 측 상태 추적을 통해 단계를 건너뛸 수 없도록 보장함  
- [ ] 예 — 클라이언트 측 통제는 순서를 제시하지만, 서버는 진행 상황을 검증하지 않음  
- [ ] 아니오 — 애플리케이션이 특정 작업 순서를 강제하지 않음  

### 공격자가 최종 단계의 URL이나 엔드포인트를 직접 요청하여 프로세스의 마지막 단계에 도달할 수 있습니까?
- [ ] 아니오 — 이전 단계의 완료 없이는 최종 단계에 대한 직접 접근이 불가능함  
- [ ] 예 — 최종 단계(예: `/api/v1/checkout/complete`)에 직접 요청을 통해 도달할 수 있음  

### 애플리케이션이 이전 단계의 필수 데이터가 존재하고 유효한지 확인합니까?
- [ ] 예 — 서버는 진행하기 전에 모든 이전 단계의 데이터를 검증함  
- [ ] 아니오 — 데이터가 예상되지만 검증되지 않는 단계를 "건너뛰는" 공격에 취약함  

### 워크플로 조작을 통해 MFA나 신원 확인과 같은 보안 통제를 우회할 수 있습니까?
- [ ] 아니오 — 흐름 조작을 통해 중요 통제를 우회할 수 없음  
- [ ] 예 — 검증 후 단계로 바로 이동함으로써 MFA나 신원 확인 우회가 가능함  

---