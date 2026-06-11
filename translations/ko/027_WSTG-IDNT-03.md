## WSTG-IDNT-03 — 계정 생성 프로세스 테스트 (Account Provisioning Process)

계정 생성 프로세스(Account Provisioning Process)는 애플리케이션 환경 내에서 새로운 식별 정보(Identities)가 생성, 검증 및 권한이 할당되는 방식을 관리합니다. 공격자는 이 워크플로우를 타겟팅하여 스팸 또는 서비스 거부(Denial-of-Service, DoS)를 위한 자동화된 대량 등록을 수행하거나, 신원 확인 단계를 우회하여 부정 계정을 생성하거나, 가입 단계에서 파라미터를 조작하여 자신에게 높은 권한을 부여합니다. 취약점은 주로 셀프 서비스 등록 양식, 초대 전용 시스템 또는 비즈니스 로직 결함으로 인해 무단 계정 생성이나 권한 상승(Privilege Escalation)이 가능한 관리자용 프로비저닝 콘솔에서 나타납니다. 공격자의 관점에서 프로비저닝 흐름을 침해하는 것은 추가 공격, 데이터 유출 또는 사회 공학(Social Engineering) 공격을 위한 발판으로 사용할 수 있는 합법적인 거점을 확보하는 것과 같습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 중간(Medium) / 높음(High)* |

> *공격자가 관리자 계정을 생성하거나 전역 등록 제한을 우회할 수 있는 경우 심각도는 심각(Critical)으로 상향됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### 자체 등록이 엄격하게 제어되거나 인가된 사용자로만 제한됩니까?
- [ ] 예 — 등록 기능이 **비활성화**되어 있거나 관리자의 수동 승인이 필요함  
- [ ] 예 — 등록이 개방되어 있으나 **인가된** 이메일 도메인 또는 초대 토큰(Invitation Token)으로 제한됨  
- [ ] 아니요 — 도메인이나 토큰 제한 없이 모든 사용자에게 등록이 **개방**되어 있음  

### 계정 활성화 전에 강력한 신원 확인 메커니즘(예: 이메일/SMS)이 필요합니까?
- [ ] 예 — 확인이 **필수적**이며 우회할 수 **없음**  
- [ ] 예 — 확인이 **필수적**이지만 파라미터 변조(Parameter Tampering) 또는 예측 가능한 토큰을 통해 우회 **가능함**  
- [ ] 아니요 — 확인 절차 없이 제출 즉시 계정이 **활성화**됨  

### 사용자가 등록 요청 중에 역할 또는 권한 파라미터를 조작할 수 있습니까?
- [ ] 아니요 — 역할이 **서버 측(Server-side)**에서 할당되며 클라이언트 측 파라미터가 존재하지 않음  
- [ ] 예 — 역할 파라미터가 존재하지만 서버 측 검증으로 인해 조작이 **불가능함**  
- [ ] 예 — 역할/권한 파라미터(예: `role=admin`, `is_staff=true`)를 수정하여 높은 권한을 획득할 수 **있음** *(심각, Critical)*  

### 대량의 계정 생성을 방지하기 위한 속도 제한(Rate Limiting) 또는 자동화 방지 제어가 구현되어 있습니까?
- [ ] 예 — 속도 제한 및 CAPTCHA가 **활성화**되어 있으며 효과적임  
- [ ] 예 — 제어 항목이 존재하지만 헤더 변조(Header Spoofing) 또는 CAPTCHA 해결 서비스를 통해 우회가 **가능함**  
- [ ] 아니요 — 속도 제한이나 자동화 방지 제어가 **적용되지 않음**  

### 계정 생성 프로세스에서 기존 계정에 대한 정보가 노출됩니까 (사용자 열거, User Enumeration)?
- [ ] 아니요 — 응답 메시지가 기존 사용자나 존재하지 않는 사용자 모두에게 **동일함**  
- [ ] 예 — 응답 메시지의 차이 또는 응답 시간의 차이로 인해 기존 사용자를 **열거**할 수 있음  

---