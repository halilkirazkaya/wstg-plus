## WSTG-ERRH-01 — 부적절한 오류 처리(Improper Error Handling) 테스트

부적절한 오류 처리는 웹 애플리케이션이 오류 응답을 통해 스택 트레이스(stack traces), 데이터베이스 스키마(database schema) 정보 또는 내부 파일 경로(internal file paths)와 같은 민감한 기술적 세부 정보를 노출할 때 발생합니다. 공격자는 잘못된 형식의 입력값(malformed input)을 제출하거나, 존재하지 않는 리소스에 접근하거나, 서버 측 예외(server-side exceptions)를 유발하여 애플리케이션의 기본 아키텍처를 파악하고 추가 공격을 위한 잠재적 벡터를 식별합니다. 이러한 정보 유출은 공격자에게 정확한 환경 사양을 제공함으로써 SQL 인젝션(SQL Injection) 또는 경로 탐색(Path Traversal)을 포함한 더 심각한 공격의 전조 역할을 하는 경우가 많습니다. 안전한 구현을 위해서는 상세한 진단 정보는 보안이 유지되는 서버 측 로그에만 기록하고, 사용자에게는 일반적인 사용자 친화적 메시지를 제공해야 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### 애플리케이션이 처리되지 않은 예외에 대해 일반적인 글로벌 오류 핸들러를 사용합니까?
- [ ] 예 — 일반 오류 페이지가 **활성화**되어 있으며 기술적인 세부 정보를 **노출하지 않음**  
- [ ] 예 — 사용자 정의 오류 페이지를 사용하지만 특정 헤더를 통해 정보 유출이 **가능함**  
- [ ] 아니요 — 기본 서버 오류 페이지(예: Tomcat, IIS, Nginx)가 **노출됨**  

### 잘못된 형식의 입력값이나 경계값 케이스를 통해 기술적인 정보를 나열(Enumeration)할 수 있습니까?
- [ ] 아니요 — 로깅을 위한 고유 참조 ID와 함께 오류가 적절히 처리됨  
- [ ] 예 — 응답에 스택 트레이스 또는 데이터베이스 쿼리가 **노출됨**  
- [ ] 예 — 내부 파일 경로, 환경 변수 또는 서버 버전 문자열이 **노출됨**  

### API 응답에서 상세한 오류 객체나 디버그 정보가 유출되고 있습니까?
- [ ] 아니요 — API가 표준화된 오류 코드와 정제된 JSON 메시지를 반환함  
- [ ] 예 — API 응답 본문에 상세한 디버그 객체나 전체 예외 세부 정보가 포함됨  
- [ ] 예 — JSON 응답의 `details` 또는 `exception` 필드에 스택 트레이스가 포함됨  

### 오류 발생 시 애플리케이션이 다르게 동작(시간 기반 또는 콘텐츠 기반)합니까?
- [ ] 아니요 — 오류 유형에 관계없이 응답 시그니처와 타이밍이 일관됨  
- [ ] 예 — 차분 응답(differential responses)을 사용하여 유효한 상태와 유효하지 않은 상태를 구분 및 나열할 수 있음 (예: 사용자 나열(User Enumeration))  

---