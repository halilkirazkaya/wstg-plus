## WSTG-INPV-20 — 대량 할당 (Mass Assignment)

대량 할당 (Mass Assignment)은 오버포스팅 (Overposting) 또는 매개변수의 안전하지 않은 역직렬화 (Insecure Deserialization)라고도 하며, 웹 애플리케이션이 적절한 속성 필터링 없이 유입되는 HTTP 요청 매개변수(HTTP request parameters)를 내부 객체 속성에 자동으로 바인딩할 때 발생합니다. 이 취약점은 JSON, XML 또는 폼 인코딩된 데이터가 애플리케이션 측 데이터 모델로 직접 역직렬화되는 현대적인 MVC 프레임워크와 RESTful API에서 흔히 발견됩니다. 공격자는 요청에 `isAdmin`, `role` 또는 `account_balance`와 같이 정의되지 않은 추가 매개변수를 주입하여 개발자가 사용자 제어에 노출하려 의도하지 않았던 민감한 필드를 수정함으로써 이 동작을 악용합니다. 성공적인 공격은 일반적으로 권한 상승 (Privilege Escalation), 무단 데이터 수정 또는 중요한 비즈니스 로직 워크플로우 우회를 초래합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **테스트 상태** | Not Performed |
| **위험도** | High / Medium* |

> *관리자 속성, 권한 수준 또는 결제 관련 필드를 수정할 수 있는 경우 위험도는 High(높음)가 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### 애플리케이션이 유입되는 요청 매개변수에 대해 자동 바인딩(Automatic Binding)을 사용합니까?
- [ ] 아니요 — 매개변수가 특정 변수나 안전한 객체에 수동으로 매핑됩니다.  
- [ ] 예 — 자동 바인딩이 사용되지만 엄격한 화이트리스트 (White-lists) 또는 DTO (Data Transfer Objects)가 강제 적용됩니다.  
- [ ] 예 — 자동 바인딩이 사용되며 민감한 내부 속성을 수정할 수 있습니다.  

### 관리자 또는 권한 관련 속성을 성공적으로 주입할 수 있습니까?
- [ ] 아니요 — 주입된 매개변수는 무시되거나 제거되거나 유효성 검사 오류를 발생시킵니다.  
- [ ] 예 — 추가 매개변수가 수용되지만 백엔드 객체 상태에는 영향을 미치지 않습니다.  
- [ ] 예 — `is_admin`, `role` 또는 `permissions`와 같은 매개변수 주입을 통해 권한 상승 (Privilege Escalation)에 성공합니다. *(Critical)*  

### 오버포스팅을 통해 민감한 비즈니스 로직이나 금융 필드를 수정하는 것이 가능합니까?
- [ ] 아니요 — `price`, `balance` 또는 `status`와 같은 필드는 대량 할당으로부터 보호됩니다.  
- [ ] 예 — JSON 또는 폼 요청 본문에 속성을 추가함으로써 민감한 비즈니스 속성을 수정할 수 있습니다.  

### 애플리케이션이 매개변수 추측을 용이하게 하는 내부 객체 구조를 노출합니까?
- [ ] 아니요 — 응답에는 의도된 공개 필드만 포함되며 문서는 제한되어 있습니다.  
- [ ] 예 — GET 응답에서 내부 객체 필드가 노출되어 매개변수 발견이 가능합니다.  
- [ ] 예 — API 문서 (Swagger/OpenAPI)에 할당 가능한 내부 속성이 명시적으로 나열되어 있습니다.  

---