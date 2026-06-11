## WSTG-INPV-14 — 배양된 취약점 테스트 (Testing for Incubated Vulnerabilities)

배양된 취약점(Incubated Vulnerabilities)은 지속형 또는 2차 취약점(Persistent or Second-order Vulnerabilities)으로도 불리며, 악성 페이로드(Payload)가 애플리케이션의 "휴면" 상태로 저장되었다가 나중에 다른 문맥에서 실행되거나 처리될 때 발생합니다. 이 공격 패턴은 일반적으로 적절한 재검증이나 출력 인코딩(Output Encoding) 없이 공유 데이터베이스나 파일 시스템에서 데이터를 가져오는 백엔드 시스템, 관리자 콘솔 또는 자동화된 보고 도구를 대상으로 합니다. 모의 해킹 전문가는 입력 지점("배양기")부터 해당 데이터가 최종적으로 렌더링되거나 다른 구성 요소 또는 보조 애플리케이션에서 활용되는 모든 위치까지 사용자 입력의 생명주기를 추적해야 합니다. 성공적인 공격은 저장형 XSS(Stored XSS)를 통한 관리자 세션 탈취나 내부 보고 모듈의 2차 SQL 인젝션(Second-order SQL Injection)을 통한 데이터 유출과 같이 파급력이 큰 결과를 초래하는 경우가 많습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**도구:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### 사용자 제어 입력값이 나중에 다른 사용자나 프로세스에 의해 조회되기 위해 저장되는 엔드포인트(API)가 있습니까?
- [ ] 아니요 — 애플리케이션이 나중에 사용하기 위해 사용자 입력을 저장하지 않음  
- [ ] 예 — 입력값이 데이터베이스 필드, 로그 파일 또는 설정 파일에 저장됨  

### 데이터가 영구 저장 계층에 기록되기 전에 입력 검증 또는 새니타이제이션(Sanitization)이 적용됩니까?
- [ ] 예 — 입력 시점에 엄격한 허용 목록 검증(Allow-list validation)이 적용됨  
- [ ] 예 — 일반적인 새니타이제이션이 적용되지만 인코딩이나 비표준 문자를 통한 우회(Bypass)가 가능함  
- [ ] 아니요 — 입력값이 검증되지 않은 원본(Raw) 형태 그대로 저장됨  

### 저장된 데이터가 관리자 또는 보조 인터페이스에서 렌더링될 때 적절하게 인코딩되거나 새니타이제이션 처리됩니까?
- [ ] 예 — 데이터가 렌더링되는 모든 위치에서 문맥 인식 출력 인코딩(Context-aware output encoding)이 적용됨  
- [ ] 예 — 일부 위치에서는 인코딩이 적용되지만 다른 위치(예: 내부 관리자 대시보드)에서는 누락됨  
- [ ] 아니요 — 데이터가 인코딩이나 새니타이제이션 없이 렌더링되어 페이로드 실행이 가능함  

### 데이터베이스에 저장된 페이로드가 백엔드 배치 프로세스 또는 아웃 오브 밴드(OOB) 모니터링 시스템에 영향을 미칠 수 있습니까?
- [ ] 아니요 — 백엔드 프로세스가 안전한 파싱 방법이나 매개변수화된 쿼리(Parameterized queries)를 사용함  
- [ ] 예 — 저장된 페이로드가 백엔드 로직 내에서 상호작용을 유발할 수 있음 (예: CSV Injection, 로그 내 Command Injection)  
- [ ] 예 — 저장된 페이로드가 공격자가 제어하는 서버로 아웃 오브 밴드(OOB) 요청을 트리거할 수 있음  

---