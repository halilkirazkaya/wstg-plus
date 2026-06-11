## WSTG-CLNT-02 — 자바스크립트 실행 테스트 (Testing for JavaScript Execution)

자바스크립트 실행(JavaScript Execution) 테스트는 애플리케이션이 사용자 제공 데이터를 부적절하게 처리하여 피해자의 브라우저 컨텍스트 내에서 승인되지 않은 스크립트가 실행되는 사례를 식별하는 데 중점을 둡니다. 이 취약점은 일반적으로 교차 사이트 스크립팅(Cross-Site Scripting, XSS)으로 이어지며, 공격자가 세션 쿠키(Session Cookies)를 탈취하거나, 문서 객체 모델(Document Object Model, DOM)을 조작하거나, 사용자를 대신하여 특정 동작을 수행할 수 있게 합니다. 모의 침투 테스터는 URL 프래그먼트(URL Fragments), `localStorage`, 또는 `window.name`과 같은 소스(Sources)로부터 전달된 검증되지 않은 데이터가 `innerHTML`, `document.write()`, `eval()`과 같은 위험한 싱크(Sinks)에서 처리되는지 조사합니다. 성공적인 취약점 공격(Exploit)은 사용자 세션의 무결성과 기밀성을 훼손하며, 더 복잡한 클라이언트 측 공격(Client-side attacks)을 위한 교두보 역할을 할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**도구:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### 애플리케이션이 위험한 자바스크립트 싱크에서 사용자 제공 데이터를 처리합니까?
- [ ] 아니오 — 모든 데이터가 정제되거나 `textContent`와 같은 안전한 싱크에서 사용됨  
- [ ] 예 — 데이터가 위험한 싱크에서 처리되지만, 새니타이징(Sanitization) 또는 인코딩이 **적용되어 있음**  
- [ ] 예 — 데이터가 위험한 싱크에서 처리되며, 새니타이징이 **적용되어 있지 않음**  

### 스크립트 실행을 완화하기 위한 콘텐츠 보안 정책(Content Security Policy, CSP)이 존재합니까?
- [ ] 예 — 엄격한 CSP가 **활성화되어 있으며** 우회가 **불가능함**  
- [ ] 예 — CSP가 **활성화되어 있으나** `unsafe-inline` 또는 안전하지 않은 CDN을 통해 우회가 **가능함**  
- [ ] 아니오 — CSP가 **활성화되어 있지 않음**  

### URL 파라미터나 프래그먼트를 통해 자바스크립트를 실행할 수 있습니까 (DOM 기반 XSS)?
- [ ] 아니오 — 입력값이 적절히 인코딩되어 실행이 **불가능함**  
- [ ] 예 — 입력값이 DOM에 반영되지만, 현대적인 프레임워크의 보호 기능에 의해 실행이 **차단됨**  
- [ ] 예 — 입력값이 브라우저에서 직접 실행되며, 취약점 공격이 **가능함**  

### 이벤트 핸들러(Event handlers)나 URI 스킴(URI schemes)이 스크립트 삽입 공격에 취약합니까?
- [ ] 아니오 — 이벤트 속성과 `javascript:` 스킴을 제거하도록 입력값이 정제됨  
- [ ] 예 — 이벤트 핸들러를 삽입할 수 있으나, 필터로 인해 페이로드(Payload)의 복잡성이 제한됨  
- [ ] 예 — `onerror`, `onload` 또는 `href` 속성을 통한 임의의 자바스크립트 실행이 **가능함**  

---