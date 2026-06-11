## WSTG-CLNT-01 — DOM 기반 크로스 사이트 스크립팅 테스트

DOM 기반 크로스 사이트 스크립팅 (DOM XSS)은 애플리케이션에 포함된 클라이언트 측 자바스크립트(JavaScript)가 신뢰할 수 없는 소스(Source)로부터 전달된 데이터를 안전하지 않은 방식으로 처리하여, 위험한 싱크(Sink)를 통해 문서 객체 모델 (DOM)에 데이터를 다시 기록할 때 발생합니다. 반사형(Reflected) 또는 저장형(Stored) XSS와 달리, 이 취약점은 전적으로 클라이언트 측 코드 내에 존재합니다. 페이로드(Payload)는 `innerHTML`, `eval()`, 또는 `document.write()`와 같은 싱크에 의해 처리되므로 서버로 전송되지 않는 경우가 많습니다. 공격자는 악의적인 프래그먼트나 파라미터가 포함된 URL을 제작하여 이를 악용하며, 피해자의 브라우저에서 해당 URL이 로드되면 사용자 세션 컨텍스트에서 임의의 자바스크립트가 실행됩니다. 이는 세션 토큰(Session Tokens) 유출, 민감한 데이터 탈취, 그리고 인증된 사용자를 대신하여 수행되는 무단 행위로 이어질 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**도구:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### 자바스크립트에서 사용자 제어 데이터를 캡처하기 위해 신뢰할 수 없는 소스를 사용합니까?
- [ ] 아니오 — 자바스크립트가 `location.hash`, `location.search`, 또는 `document.referrer`를 사용하지 않습니다.  
- [ ] 예 — 신뢰할 수 없는 소스가 사용되지만 데이터가 실행 싱크로 전달되지 않습니다.  
- [ ] 예 — 신뢰할 수 없는 소스가 사용되며 데이터가 클라이언트 측 로직으로 직접 전달됩니다.  

### 사용자 제어 데이터가 위험한 DOM 싱크로 전달됩니까?
- [ ] 아니오 — `innerHTML`, `outerHTML`, `document.write()`, 또는 `eval()`과 같은 싱크가 사용되지 않습니다.  
- [ ] 예 — 위험한 싱크가 존재하지만, `DOMPurify`와 같은 보안 라이브러리를 사용하여 데이터를 엄격하게 새니타이제이션(Sanitization)합니다.  
- [ ] 예 — 위험한 싱크가 존재하며 데이터가 불충분하거나 사용자 정의 정규식 기반 필터링으로 처리됩니다.  
- [ ] 예 — 위험한 싱크가 존재하며 데이터가 새니타이제이션 또는 인코딩(Encoding) 없이 전달됩니다. *(치명적)*  

### URL 기반 페이로드를 통해 실행 싱크에 도달할 수 있습니까?
- [ ] 아니오 — 외부 입력을 통해 소스에서 싱크로의 흐름(Source-to-sink flow)을 유도할 수 없습니다.  
- [ ] 예 — 흐름은 가능하지만 복잡한 사용자 상호작용이 필요합니다.  
- [ ] 예 — 흐름이 가능하며 단순한 URL 프래그먼트나 쿼리 파라미터를 통해 유도될 수 있습니다.  

### 현대적인 보안 헤더 또는 브라우저 기반 완화 조치가 위험을 중화하고 있습니까?
- [ ] 예 — `script-src` 및 `trusted-types`가 포함된 엄격한 콘텐츠 보안 정책 (CSP)이 활성화되어 실행을 방지합니다.  
- [ ] 예 — CSP가 존재하지만 `unsafe-inline` 또는 취약한 해시(Hash)로 인해 우회(Bypass)가 가능합니다.  
- [ ] 아니오 — DOM 기반 실행을 완화할 CSP가 존재하지 않습니다.  

### 애플리케이션이 보호 기능이 내장된 클라이언트 측 프레임워크를 사용합니까?
- [ ] 예 — 프레임워크(예: Angular, React)가 데이터를 자동으로 인코딩하며 우회가 불가능합니다.  
- [ ] 예 — 프레임워크를 사용하지만 `dangerouslySetInnerHTML`과 같은 "이스케이프 해치(Escape hatches)"가 활성화되어 있습니다.  
- [ ] 아니오 — 애플리케이션이 자동 이스케이프 기능이 없는 바닐라 자바스크립트(Vanilla JavaScript) 또는 레거시 라이브러리(예: 구버전 jQuery)를 사용합니다.  

---