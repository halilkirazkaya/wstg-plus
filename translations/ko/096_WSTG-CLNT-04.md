## WSTG-CLNT-04 — 클라이언트 측 URL 리다이렉트(Client-Side URL Redirect) 테스트

클라이언트 측 URL 리다이렉트(Client-Side URL Redirect)는 웹 애플리케이션이 주로 URL 파라미터나 해시 프래그먼트(Hash Fragments)를 통해 사용자 제어 입력을 수락하고, 이를 `window.location` 또는 `document.location.href`와 같은 클라이언트 측 리다이렉션 싱크(Sink) 내에서 사용할 때 발생합니다. 이 취약점은 피해자가 악성 사이트로 조용히 리다이렉트되기 전에 먼저 합법적인 도메인을 신뢰하게 되므로, 공격자가 정교한 피싱(Phishing) 캠페인을 수행하는 데 주로 활용됩니다. 피싱 외에도, URL이나 `Referer` 헤더에 해당 값이 포함된 상태에서 리다이렉션이 발생하는 경우, 클라이언트 측 리다이렉트를 연쇄적으로 이용하여 OAuth 액세스 토큰(Access Tokens)이나 세션 식별자(Session Identifiers)와 같은 민감한 데이터를 탈취(Exfiltrate)할 수 있습니다. 현대의 단일 페이지 애플리케이션(SPA)에서는 라우팅 로직, 인증 후 "return to" 파라미터, 또는 언어 전환 기능에서 이 취약점이 자주 나타납니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium)* |

> *리다이렉트를 사용하여 OAuth 토큰, 세션 ID를 탈취하거나 인증 흐름의 보안 통제를 우회할 수 있는 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### 애플리케이션이 사용자 입력을 기반으로 리다이렉트를 처리하기 위해 클라이언트 측 스크립트를 사용합니까?
- [ ] 아니요 — 리다이렉션은 `3xx` HTTP 상태 코드를 통해 서버 측에서만 처리됩니다.  
- [ ] 예 — 애플리케이션이 URL 파라미터를 기반으로 이동하기 위해 `window.location`과 같은 JavaScript 싱크를 사용합니다.  
- [ ] 예 — 애플리케이션이 사용자 제공 경로를 처리하는 클라이언트 측 프레임워크 라우터(예: React Router, Vue Router)를 사용합니다.  

### 대상 URL에 대해 입력 검증 또는 화이트리스트(Whitelisting) 통제가 구현되어 있습니까?
- [ ] 예 — 허용된 도메인/경로에 대한 엄격한 **화이트리스트**가 적용되어 있으며 우회가 **불가능**합니다.  
- [ ] 예 — 검증이 존재하지만 취약한 정규식(Regex) 또는 블랙리스트(Blacklist)에 의존하며 우회가 **가능**합니다.  
- [ ] 아니요 — 애플리케이션이 임의의 문자열을 리다이렉션 대상으로 수락합니다.  

### 일반적인 난독화 기법을 사용하여 리다이렉트 로직을 우회할 수 있습니까?
- [ ] 아니요 — 통제 장치가 인코딩된 문자, 프로토콜 상대 URL(`//`) 및 백슬래시를 올바르게 처리합니다.  
- [ ] 예 — URL 인코딩 또는 이중 인코딩(Double-encoding)을 사용하여 우회가 **가능**합니다.  
- [ ] 예 — 프로토콜 상대 URL 또는 `@` 문자(예: `https://expected.com@attacker.com`)를 사용하여 우회가 **가능**합니다.  
- [ ] 예 — 특정 브라우저의 특이성(Quirks) 또는 널 바이트 주입(Null Byte Injection)을 사용하여 우회가 **가능**합니다.  

### 리다이렉션 과정에서 대상 사이트에 민감한 데이터가 노출됩니까?
- [ ] 아니요 — URL에 민감한 데이터가 없으며 리다이렉트 중에 헤더를 통해 전송되지 않습니다.  
- [ ] 예 — 민감한 데이터(예: 세션 토큰, API 키)가 `Referer` 헤더를 통해 외부 사이트로 **유출**됩니다.  
- [ ] 예 — URL 프래그먼트(`#`)에 포함된 민감한 데이터에 대상 사이트의 스크립트가 **접근 가능**합니다.  

### 리다이렉트 싱크를 통해 JavaScript를 실행할 수 있습니까 (DOM 기반 XSS)?
- [ ] 아니요 — 싱크가 `http` 또는 `https` 프로토콜로의 이동만 허용합니다.  
- [ ] 예 — 리다이렉트 싱크가 `javascript:` 또는 `data:` URI를 허용하여 DOM 기반 XSS(DOM-based XSS)가 **가능**합니다.  

---