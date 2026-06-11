## WSTG-BUSL-05 — 기능 사용 횟수 제한 테스트 (Test Number of Times a Function Can Be Used Limits)

이 테스트는 애플리케이션이 단일 사용자, 세션 또는 IP 주소에 의해 특정 작업이나 기능이 실행될 수 있는 빈도 및 총 횟수에 대해 비즈니스 로직(Business Logic) 제약 조건을 적용하는지 평가합니다. 이러한 제한을 구현하지 못하면 공격자가 SMS OTP(One-Time Password) 발송, 비밀번호 재설정 이메일 트리거, 할인 코드 적용 또는 대용량 데이터 내보내기와 같은 고가치 기능을 남용할 수 있게 되어 재정적 손실, 리소스 고갈 또는 평판 저하로 이어질 수 있습니다. 이러한 취약점은 일반적으로 트랜잭션 엔드포인트(Transactional Endpoints)나 통신 기능에서 발견되며, 의도된 비즈니스 임계값을 훨씬 초과하여 작업을 반복하는 자동화된 스크립트를 통해 악용됩니다. 공격자 관점에서 이는 명시적인 속도 제한(Rate Limiting) 또는 횟수 기반 스로틀(Throttles)이 부족한 SMS 펌핑(SMS Pumping), 메일 폭격(Mail Bombing) 또는 브루트 포스(Brute Force) 워크플로우의 주요 표적이 됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **테스트 상태** | 수행되지 않음 |
| **위험도 (Severity)** | 중간 (Medium) / 높음 (High)* |

> *기능이 금융 거래, SMS 비용과 관련되거나 시스템 전체의 가용성에 영향을 미치는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### 애플리케이션이 민감한 비즈니스 기능에 대해 제한을 정의하고 강제합니까?
- [ ] 예 — 제한이 엄격하게 강제되며 우회(Bypass)가 **불가능함**  
- [ ] 예 — 제한이 강제되지만 남용을 방지하기에는 너무 높음  
- [ ] 아니요 — 기능 실행에 아무런 제한이 적용되지 않음  

### 속도 제한 및 사용량 카운터가 서버 측(Server-side)에서 강제됩니까?
- [ ] 예 — 강제가 엄격히 서버 측에서 이루어지며 조작할 수 **없음**  
- [ ] 아니요 — 강제가 클라이언트 측(Client-side) 로직(예: JavaScript 또는 숨겨진 필드)에 의존함  
- [ ] 아니요 — 어느 수준에서도 강제가 존재하지 않음  

### 헤더 또는 세션 조작을 통해 사용 제한을 우회할 수 있습니까?
- [ ] 아니요 — `X-Forwarded-For`, `Origin` 또는 세션 로테이션(Session Rotation)을 통한 우회가 **불가능함**  
- [ ] 예 — IP 헤더 스푸핑(Spoofing) 또는 쿠키 삭제를 통해 제한을 우회할 수 **있음**  
- [ ] 예 — 여러 계정 또는 세션을 생성하여 제한을 우회할 수 **있음**  

### 제한을 초과했을 때 적절한 방어 응답이 트리거됩니까?
- [ ] 예 — 애플리케이션이 `429 Too Many Requests`를 반환하고 이벤트를 로그에 기록함 *(가장 안전)*  
- [ ] 예 — 애플리케이션이 오류를 반환하지만 잠재적인 남용을 로그에 기록하지 **않음**  
- [ ] 아니요 — 애플리케이션이 요청을 계속 처리하지만 출력은 무시함  
- [ ] 아니요 — 애플리케이션이 응답을 제공하지 않거나 부하 상황에서 중단됨  

### 기능 남용의 영향이 비즈니스에 중대합니까?
- [ ] 아니요 — 기능 남용이 비용이나 리소스에 미치는 영향이 미미함  
- [ ] 예 — 남용이 경미한 리소스 소비 또는 사용자 불편을 초래할 수 **있음**  
- [ ] 예 — 남용이 상당한 재정적 비용(예: SMS 요금) 또는 서비스 거부(Denial of Service)로 이어질 수 **있음** *(치명적)*  

---