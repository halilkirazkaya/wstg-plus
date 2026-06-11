## WSTG-INPV-01 — 반사형 크로스 사이트 스크립팅 (Reflected Cross Site Scripting, XSS)

반사형 크로스 사이트 스크립팅(Reflected Cross Site Scripting, XSS)은 애플리케이션이 충분한 검증이나 인코딩 없이 신뢰할 수 없는 데이터를 HTTP 응답에 포함시켜, 페이로드(Payload)가 피해자의 브라우저 컨텍스트 내에서 실행될 때 발생합니다. 공격자는 사회공학(Social Engineering) 기법을 통해 특수하게 제작된 URL이나 양식을 전달하며, 이를 통해 사용자 세션(User Session) 탈취, 민감한 쿠키(Cookie) 유출 또는 사용자를 대신한 무단 행위를 수행합니다. 이 취약점은 주로 검색 매개변수, 에러 메시지 및 입력 값을 사용자 인터페이스로 직접 반사하는 모든 엔드포인트(Endpoint)에서 발견됩니다. 공격 성공 여부는 피해자가 악성 링크와 상호작용하는지에 달려 있으며, 이는 비지속적이지만 특정 관리자나 인증된 사용자를 타겟팅하는 데 매우 효과적인 공격 벡터입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **테스트 상태** | Not Performed |
| **심각도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### 사용자 입력 값이 응답 본문에 반사됩니까?
- [ ] 아니요 — 입력 값이 사용자에게 절대 반사되지 않음  
- [ ] 예 — 입력 값이 반사되지만 적절히 인코딩되어 우회가 불가능함  
- [ ] 예 — 입력 값이 인코딩이나 필터링 없이 그대로 반사됨  

### 컨텍스트 인식 출력 인코딩(Context-aware output encoding)이 구현되어 있습니까?
- [ ] 예 — 특정 반사 컨텍스트에 따라 적절한 인코딩(HTML, 속성, 자바스크립트)이 적용됨  
- [ ] 예 — 인코딩이 적용되었으나 특정 컨텍스트(예: `<script>` 태그 내 HTML 인코딩)에 대해 불충분함  
- [ ] 아니요 — 인코딩이 적용되지 않음  

### 입력 값 검증 또는 웹 애플리케이션 방화벽(Web Application Firewall, WAF) 필터를 우회할 수 있습니까?
- [ ] 아니요 — 필터가 모든 일반적인 페이로드 및 난독화된 XSS 페이로드를 효과적으로 차단함  
- [ ] 예 — 필터가 존재하지만 문자 인코딩, 비표준 태그 또는 폴리글랏(Polyglots)을 사용하여 우회가 가능함  
- [ ] 아니요 — 필터나 검증 메커니즘이 전혀 존재하지 않음  

### 반사된 입력 값의 실행 컨텍스트는 무엇입니까?
- [ ] 안전함 — 입력 값이 실행 불가능한 위치(예: 일반적인 `<div>` 또는 `<span>` 태그 내)에 반사됨  
- [ ] 위험함 — 입력 값이 HTML 속성 내부 또는 태그의 `src`/`href` 내부에 반사됨  
- [ ] 심각함 — 입력 값이 `<script>` 블록, 이벤트 핸들러 또는 템플릿 리터럴 내부에 직접 반사됨  

### XSS 영향을 완화하기 위한 현대적인 브라우저 보안 헤더가 존재합니까?
- [ ] 예 — `Content-Security-Policy` (CSP)가 엄격한 script-src와 함께 활성화되어 있음  
- [ ] 예 — `Content-Security-Policy` (CSP)가 활성화되어 있으나 `unsafe-inline` 또는 취약한 화이트리스트를 포함하고 있음  
- [ ] 아니요 — CSP 또는 기존의 `X-XSS-Protection` 헤더가 존재하지 않음  

---