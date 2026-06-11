## WSTG-INPV-04 — HTTP 매개변수 오염(HTTP Parameter Pollution, HPP) 테스트

HTTP 매개변수 오염(HTTP Parameter Pollution, HPP)은 애플리케이션이 동일한 이름의 여러 HTTP 매개변수를 수신하고 이를 일관되지 않거나 안전하지 않은 방식으로 처리할 때 발생합니다. 공격자는 중복된 매개변수를 제공함으로써 애플리케이션의 내부 로직을 조작하여 웹 애플리케이션 방화벽(Web Application Firewall, WAF), 입력값 검증 필터 또는 접근 제어 메커니즘을 우회할 수 있습니다. 이 취약점은 기반 웹 서버 또는 애플리케이션 프레임워크에 크게 의존하며, 프레임워크는 반복된 매개변수 중 첫 번째, 마지막 또는 연결된 버전을 선택할 수 있습니다. 공격은 일반적으로 매개변수를 백엔드(Back-end) API, 데이터베이스 쿼리 또는 URL 리다이렉션 메커니즘으로 전달하는 엔드포인트(Endpoints)를 대상으로 하며, 교차 사이트 스크립팅(Cross-Site Scripting, XSS)에서 무단 데이터 유출에 이르는 영향을 미칩니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### 애플리케이션이 동일한 이름의 여러 HTTP 매개변수를 어떻게 처리합니까?
- [ ] 아니요 — 중복된 매개변수는 서버에 의해 거부되거나 무시됩니다.
- [ ] 예 — 서버가 일관되게 첫 번째 또는 마지막 항목만 선택합니다.
- [ ] 예 — 서버가 값을 연결(concatenate)하며, 이는 잠재적인 인젝션(Injection)을 허용할 수 있습니다.

### HPP를 활용하여 WAF 또는 입력 필터와 같은 보안 통제를 우회할 수 있습니까?
- [ ] 아니요 — 보안 통제가 매개변수의 모든 항목을 올바르게 검사합니다.
- [ ] 예 — WAF 또는 필터가 첫 번째 항목만 검사하여, 악성 두 번째 항목이 통과될 수 있습니다.
- [ ] 예 — 연결 방식을 통해 공격자가 탐지를 피하기 위해 여러 매개변수에 걸쳐 페이로드(Payload)를 분할할 수 있습니다.

### 애플리케이션이 적절한 새니타이제이션(Sanitization) 없이 응답에 오염된 매개변수를 반영합니까?
- [ ] 아니요 — 매개변수가 UI에 반영되기 전에 새니타이제이션되거나 인코딩됩니다.
- [ ] 예 — 오염으로 인해 콘텐츠가 반영되지만, 새니타이제이션이 적용됩니다.
- [ ] 예 — 오염으로 인해 콘텐츠가 반영되고 새니타이제이션이 적용되지 않아, XSS 또는 로직 오류가 발생합니다.

### 내부 API 호출 또는 백엔드 시스템으로 전달되는 매개변수가 오염에 취약합니까?
- [ ] 아니요 — 백엔드 요청이 안전하고 비동적인 구성 방식을 사용합니다.
- [ ] 예 — HPP를 통해 공격자가 백엔드 요청 구조에서 매개변수를 주입하거나 덮어쓸 수 있습니다.

### 애플리케이션이 GET 요청과 POST 요청에서 매개변수가 오염되었을 때 서로 다른 동작을 보입니까?
- [ ] 아니요 — 모든 HTTP 메서드에서 동작이 일관됩니다.
- [ ] 예 — GET 매개변수만 오염에 취약합니다.
- [ ] 예 — POST(본문) 매개변수만 오염에 취약합니다.

---