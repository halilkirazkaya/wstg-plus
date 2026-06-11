## WSTG-CLNT-03 — HTML 인젝션(HTML Injection)

HTML 인젝션(HTML Injection)은 애플리케이션이 사용자 제공 입력을 브라우저에 렌더링하기 전에 적절히 정제(Sanitize)하지 않을 때 발생하며, 공격자가 웹 페이지의 문서 객체 모델(Document Object Model, DOM) 내에 임의의 HTML 태그를 삽입할 수 있게 합니다. 교차 사이트 스크립팅(Cross-Site Scripting, XSS)과 유사하지만, HTML 인젝션의 주요 목적은 페이지의 시각적 표현을 조작하거나 악성 폼(Form) 및 기만적인 콘텐츠를 삽입하여 피싱(Phishing) 공격을 용이하게 하는 것입니다. 공격자는 검색어, 사용자 프로필 필드 또는 에러 메시지와 같이 UI에 반영되는 파라미터를 표적으로 삼아, 피해자가 민감한 자격 증명(Credentials)을 공개하거나 승인되지 않은 제3자 링크와 상호 작용하도록 유도합니다. 이 취약점은 애플리케이션 사용자 인터페이스의 무결성에 중대한 위험을 초래하며, 더 복잡한 사회 공학(Social Engineering) 공격이나 세션 관련 공격의 전조가 될 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### 사용자 제어 입력이 적절한 HTML 인코딩(HTML Encoding) 없이 DOM에 반영됩니까?
- [ ] 아니요 — 모든 반영된 입력은 렌더링되기 전에 엄격하게 HTML 인코딩됩니다.
- [ ] 예 — 일부 문자는 인코딩되지만 특정 태그에 대한 우회(Bypass)가 가능합니다.
- [ ] 예 — 입력이 원본 그대로 반영되며 HTML 인젝션이 가능합니다.

### 페이지 레이아웃을 변경하기 위해 구조적 HTML 요소를 삽입할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 `<div>`, `<table>`, 또는 `<iframe>`과 같은 구조적 태그를 필터링하거나 제거합니다.
- [ ] 예 — 구조적 요소가 삽입될 수 있지만 UI에 큰 영향을 미치지는 않습니다.
- [ ] 예 — 삽입된 태그를 통해 심각한 UI 변조(Defacement)가 가능합니다.

### 폼이나 악성 링크와 같은 기능적인 피싱 요소를 삽입할 수 있습니까?
- [ ] 아니요 — `<form>`, `<input>`, 및 `<a>` 태그가 효과적으로 차단되거나 정제됩니다.
- [ ] 예 — 사용자를 외부 사이트로 리다이렉트(Redirect)하는 악성 링크가 삽입될 수 있습니다.
- [ ] 예 — 자격 증명을 수집하기 위한 기능적인 `<form>` 요소가 삽입될 수 있습니다. *(중간)*

### 보안 헤더 또는 콘텐츠 보안 정책(Content Security Policy, CSP)이 인젝션의 영향을 완화합니까?
- [ ] 예 — 제한적인 CSP가 적용되어 있으며 `form-action` 또는 `frame-src`가 적용되어 있습니다.
- [ ] 아니요 — 보안 헤더는 존재하지만 CSP가 비활성화되어 있거나 unsafe-inline/와일드카드(Wildcards)를 포함하고 있습니다.
- [ ] 아니요 — CSP나 관련 보안 헤더가 활성화되어 있지 않습니다.

---