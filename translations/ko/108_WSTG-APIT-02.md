## WSTG-APIT-02 — API 객체 수준 권한 관리 취약점 (Broken Object Level Authorization, BOLA)

객체 수준 권한 관리 취약점(Broken Object Level Authorization, BOLA)은 안전하지 않은 직접 객체 참조(Insecure Direct Object Reference, IDOR)라고도 하며, API가 사용자가 ID로 식별된 특정 리소스에 액세스하거나 조작할 수 있는 적절한 권한이 있는지 검증하지 못할 때 발생합니다. 공격자는 요청 경로, 쿼리 매개변수 또는 JSON 본문에서 숫자 ID나 UUID와 같은 식별자를 체계적으로 열거하거나 추측하여 다른 사용자의 데이터에 액세스함으로써 이를 악용합니다. 이 취약점은 현대 API 보안에서 가장 흔하고 영향력이 큰 문제로, 대규모 데이터 유출, 무단 수정 또는 완전한 계정 탈취로 이어질 수 있습니다. 공격자의 관점에서 목표는 객체 식별자를 허용하는 엔드포인트를 식별하고, 서버 측 권한 부여(Authorization) 로직이 누락되었거나 해당 ID를 조작하여 우회할 수 있는지 테스트하는 것입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 / 치명적 |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**도구:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### API 요청 내에서 객체 식별자를 예측하거나 열거할 수 있습니까?
- [ ] 아니요 — 식별자가 길고 무작위적이며 암호학적으로 안전합니다 (예: UUIDv4)  
- [ ] 예 — 식별자가 순차적인 정수입니다 (예: `101`, `102`)  
- [ ] 예 — 식별자가 예측 가능한 패턴을 따르거나 공개된 정보에서 파생됩니다  

### API가 모든 요청에 대해 객체 소유권에 대한 서버 측 검증을 수행합니까?
- [ ] 예 — 권한 부여 확인이 모든 요청에 적용되며 우회가 **불가능합니다**  
- [ ] 예 — 확인이 적용되지만 매개변수 오염(Parameter Pollution) 또는 메서드 터널링(Method Tunneling)을 통해 우회가 **가능합니다**  
- [ ] 아니요 — 애플리케이션이 특정 리소스 소유권을 확인하지 않고 사용자가 인증(Authentication)되었다는 사실에만 전적으로 의존합니다  

### 식별자를 변경하여 다른 사용자의 리소스에 액세스하거나 수정할 수 있습니까?
- [ ] 아니요 — 다른 사용자의 리소스에 대한 무단 액세스가 **불가능합니다**  
- [ ] 예 — 무단 **읽기** 액세스(수평적 BOLA)가 **가능합니다**  
- [ ] 예 — 무단 **수정** 또는 **삭제**(수평적 BOLA)가 **가능합니다**  

### API가 ID를 대체하여 관리자 또는 시스템 수준 객체에 액세스하는 것을 허용합니까?
- [ ] 아니요 — 관리 리소스는 보조 권한 부여 계층에 의해 보호됩니다  
- [ ] 예 — 시스템 수준 리소스 액세스 또는 수정(수직적 BOLA)이 **가능합니다**  

### 식별자를 요청의 다른 부분으로 이동하여 권한 부여 확인을 우회할 수 있습니까?
- [ ] 아니요 — 매개변수 위치에 관계없이 권한 부여 로직이 일관됩니다  
- [ ] 예 — ID를 URL 경로에서 JSON 본문이나 헤더로 이동할 때 우회가 **가능합니다**  
- [ ] 예 — 여러 ID가 제공되고 서버가 권한이 없는 식별자를 처리할 때 우회가 **가능합니다**  

---