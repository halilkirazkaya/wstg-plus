## WSTG-INPV-05 — SQL 삽입 (SQL Injection)

SQL 삽입(SQL Injection)은 사용자 입력값이 적절한 매개변수화(Parameterization) 또는 필터링(Sanitization) 없이 SQL 쿼리에 포함되어 공격자가 쿼리 로직을 조작할 수 있을 때 발생합니다. 공격에 성공할 경우 무단 데이터 유출(Data Exfiltration), 인증 우회(Authentication Bypass), 데이터 수정이 가능하며, 일부 경우에는 `xp_cmdshell`이나 `LOAD_FILE()`과 같은 데이터베이스 기능을 통해 원격 코드 실행(Remote Code Execution, RCE)으로 이어질 수 있습니다. 이 취약점은 일반적으로 로그인 양식, 검색 기능, REST API 매개변수, HTTP 헤더 및 사용자 제어 입력을 통해 동적 SQL 쿼리를 생성하는 모든 엔드포인트(Endpoint)에서 발생합니다. 공격자 관점에서 취약점 지점(Injection Point)을 식별하는 것은 데이터베이스 전체 장악 및 인프라 내부에서의 측면 이동(Lateral Movement)을 위한 주요 관문이 됩니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**도구:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### 사용자 제어 매개변수가 안전한 데이터베이스 액세스 방식을 통해 처리됩니까?
- [ ] 예 — 애플리케이션이 ORM 또는 매개변수화된 쿼리(Parameterized queries)만을 사용하며 우회가 **불가능함**  
- [ ] 예 — 매개변수화된 쿼리가 사용되지만, 예외적인 경우(예: `ORDER BY` 절)를 통해 우회가 **가능함**  
- [ ] 아니요 — 매개변수화 **없이** 원시 문자열 연결(Raw string concatenation)을 사용하여 SQL 쿼리를 구성함  

### SQL 삽입을 통해 인증 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 로그인 매개변수에 삽입 취약점이 **없음**  
- [ ] 예 — 타우톨로지(Tautology) 기반 페이로드(Payload)(예: `' OR 1=1 --`)를 사용하여 인증 우회가 **가능함**  

### 인밴드(In-band) 또는 아웃오브밴드(Out-of-band) 기술을 통한 데이터 유출이 가능합니까?
- [ ] 아니요 — 식별된 데이터 유출 경로가 없음  
- [ ] 예 — UNION 기반 또는 오류 기반(Error-based) 유출이 **가능함**  
- [ ] 예 — 블라인드(Blind)(불리언 또는 시간 기반) 유출이 **가능함**  
- [ ] 예 — 아웃오브밴드(DNS/HTTP) 유출이 **가능함**  

### 애플리케이션 기능에서 2차 SQL 삽입(Second-order SQL Injection) 취약점이 나타납니까?
- [ ] 아니요 — 저장된 데이터를 이후 쿼리에서 호출할 때 안전하게 처리함  
- [ ] 예 — 사용자 입력이 저장된 후, 검증 **없이** 안전하지 않은 SQL 쿼리에 나중에 사용됨  

### 데이터베이스 서비스 계정 권한이 필요한 최소한으로 제한되어 있습니까?
- [ ] 예 — 서비스 계정이 **최소 권한**(예: 특정 테이블/작업으로 제한됨)을 보유함  
- [ ] 아니요 — 서비스 계정이 높은 권한을 보유함 (예: `DROP TABLE`, `FILE` 권한 또는 `xp_cmdshell`이 **활성화됨**)  

---