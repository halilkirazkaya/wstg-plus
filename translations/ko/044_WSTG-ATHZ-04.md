## WSTG-ATHZ-04 — 비인가 직접 객체 참조(Insecure Direct Object References, IDOR) 테스트

비인가 직접 객체 참조(Insecure Direct Object References, IDOR)는 애플리케이션이 사용자 제공 입력에 기반하여 객체에 대한 직접적인 접근을 허용하면서, 요청자의 권한을 확인하는 권한 부여(Authorization) 검토를 수행하지 않을 때 발생합니다. 이 취약점은 공격자가 다른 사용자 또는 시스템에 속한 데이터를 조회, 수정 또는 삭제할 수 있게 하므로 기밀성과 무결성에 중대한 영향을 미칩니다. 일반적으로 내부 데이터베이스 키, 파일 이름 또는 계정 식별자를 참조하는 URL 파라미터, POST 바디 데이터 또는 JSON 키에서 나타납니다. 공격자 관점에서 이 취약점의 익스플로잇(Exploit)은 정수 값을 증가시키거나 UUID를 퍼징(Fuzzing)하는 등 식별자를 조작하여 자신의 권한 범위를 벗어난 리소스에 접근하는 방식으로 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### 애플리케이션이 요청 파라미터에서 직접 객체 식별자를 사용합니까?
- [ ] 아니오 — 애플리케이션이 간접 참조 또는 암호화된 맵을 사용함 *(가장 안전)*  
- [ ] 예 — 애플리케이션이 예측 불가능한 식별자(예: 긴 UUID/해시)를 사용함  
- [ ] 예 — 애플리케이션이 예측 가능한 식별자(예: 순차적 정수 또는 사용자 이름)를 사용함  

### 객체 식별자가 포함된 모든 요청에 대해 서버 측 권한 부여가 수행됩니까?
- [ ] 예 — 어떤 식별자에 대해서도 서버 측 검증을 우회할 수 없음  
- [ ] 예 — 서버 측 검증이 존재하지만 파라미터 오염(Parameter Pollution) 또는 헤더 조작을 통해 우회가 가능함  
- [ ] 아니오 — 요청된 객체의 소유권을 확인하기 위한 권한 부여 검증이 적용되지 않음  

### 공격자가 다른 사용자에게 속한 객체에 접근하거나 이를 수정할 수 있습니까? (수평적 IDOR)
- [ ] 아니오 — 다른 사용자의 데이터에 접근할 수 없음  
- [ ] 예 — 다른 사용자의 데이터를 조회할 수 있으나 수정은 불가능함  
- [ ] 예 — 다른 사용자의 데이터를 조회 및 수정할 수 있음 *(치명적)*  

### 관리자 또는 시스템 수준의 객체에 접근할 수 있습니까? (수직적 IDOR)
- [ ] 아니오 — 더 높은 권한의 객체에 접근할 수 없음  
- [ ] 예 — 관리 설정 또는 시스템 파일에 접근할 수 있음  

### 애플리케이션이 검색, 메타데이터 또는 기타 엔드포인트를 통해 객체 식별자를 유출합니까?
- [ ] 아니오 — 식별자가 의도치 않게 노출되지 않음  
- [ ] 예 — 보조 엔드포인트 또는 공개 프로필을 통해 다른 사용자/객체의 식별자를 열거할 수 있음  

---