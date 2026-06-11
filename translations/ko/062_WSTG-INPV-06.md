## WSTG-INPV-06 — LDAP 인젝션(LDAP Injection) 테스트

LDAP 인젝션(LDAP Injection)은 애플리케이션이 적절한 정리(Sanitization)나 이스케이프(Escaping) 처리 없이 사용자 제공 데이터를 LDAP(Lightweight Directory Access Protocol) 필터에 포함할 때 발생하며, 이를 통해 공격자가 쿼리 로직을 조작할 수 있게 합니다. 별표(*), 괄호, 논리 연산자와 같은 특수 문자를 주입함으로써, 공격자는 검색 필터를 수정하여 인증 메커니즘을 우회하거나 사용자 이름, 그룹 멤버십, 조직 속성을 포함한 민감한 디렉토리 정보를 유출할 수 있습니다. 이 취약점은 주로 Active Directory 또는 OpenLDAP과 연동되는 기업 디렉토리 검색, 직원 포털, 또는 싱글 사인온(SSO, Single Sign-On) 시스템과 같은 기능에서 나타납니다. 공격자의 관점에서, 직접적인 쿼리 결과 출력이 애플리케이션에 의해 제한될 경우, 성공적인 익스플로잇(Exploit)을 위해 디렉토리 구조나 속성 값을 비트 단위로 열거하는 블라인드(Blind) 기법이 자주 사용됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**도구:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### 애플리케이션이 인증 또는 디렉토리 검색을 위해 LDAP을 사용합니까?
- [ ] 아니요 — 애플리케이션 아키텍처에서 LDAP이 사용되지 않음  
- [ ] 예 — LDAP이 사용되며 모든 입력이 엄격하게 정리되거나 매개변수화됨  
- [ ] 예 — LDAP이 사용되며 사용자 입력이 적절한 이스케이프 처리 없이 필터에 결합됨  

### 사용자 입력에서 LDAP 메타 문자가 적절히 이스케이프되거나 필터링됩니까?
- [ ] 예 — `(`, `)`, `&`, `|`, `*`, `\`와 같은 문자가 허용되지 않거나 올바르게 이스케이프됨  
- [ ] 아니요 — 메타 문자를 주입하여 LDAP 필터 구조를 변경할 수 있음  

### 인젝션을 통해 인증 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 로그인 로직이 쿼리 조작에 취약하지 않음  
- [ ] 예 — 사용자 이름 또는 비밀번호 필드에서 논리 OR(`|`) 또는 와일드카드(`*`) 인젝션을 사용하여 인증을 우회할 수 있음  

### 디렉토리 서비스에서 블라인드 데이터 유출이 가능합니까?
- [ ] 아니요 — 검색 결과가 제한적이며 시간차 또는 불리언(Boolean) 차이가 관찰되지 않음  
- [ ] 예 — 불리언 기반 블라인드 인젝션(Boolean-based blind injection) 기법을 통해 디렉토리 속성을 비트 단위로 열거할 수 있음  
- [ ] 예 — 쿼리 조작으로 인해 전체 디렉토리 레코드가 애플리케이션 응답에 반영됨  

### 보조 애플리케이션 구성 요소(예: 메일러, SSO)가 LDAP 기반 속성 주입에 취약합니까?
- [ ] 아니요 — 모든 구성 요소에서 LDAP 속성이 안전하게 처리됨  
- [ ] 예 — 공격자가 인젝션을 통해 자신의 속성(예: 이메일, 그룹 멤버십)을 수정하여 권한을 상승시키거나 통신을 리다이렉트할 수 있음  

---