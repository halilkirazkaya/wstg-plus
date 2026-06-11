## WSTG-CLNT-06 — 클라이언트 측 리소스 변조 테스트 (Testing for Client-Side Resource Manipulation)

클라이언트 측 리소스 변조(Client-side resource manipulation)는 애플리케이션이 사용자 제어 입력을 허용하여 브라우저에서 로드되는 스크립트, 스타일시트 또는 iframe과 같은 리소스의 소스나 경로에 영향을 줄 때 발생합니다. URI 프래그먼트(URI fragments), 쿼리 파라미터(Query parameters) 또는 기타 DOM 소스(DOM sources)를 조작함으로써, 공격자는 애플리케이션이 악성 외부 콘텐츠를 로드하거나 승인되지 않은 코드를 실행하도록 강제할 수 있습니다. 이 취약점은 주로 신뢰할 수 있는 허용 목록(Allow-list)에 대한 충분한 검증 없이 HTML 요소의 `src` 또는 `href` 속성을 동적으로 채우는 JavaScript 로직에서 발견됩니다. 공격자 관점에서는 리소스 로딩을 공격자가 제어하는 서버로 리다이렉트(Redirect)하는 URL을 생성하여 이를 악용하며, 이는 잠재적으로 DOM 기반 크로스 사이트 스크립팅(DOM-based Cross-Site Scripting, XSS), 민감한 데이터 탈취(Exfiltration) 또는 UI 리드레싱(UI redressing)으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### 애플리케이션이 리소스를 동적으로 로드하거나 삽입하기 위해 클라이언트 측 싱크(Sinks)를 사용합니까?
- [ ] 아니요 — 리소스는 정적 HTML 또는 서버 측 로직을 통해서만 로드됩니다.  
- [ ] 예 — 클라이언트 측 JavaScript가 `src`, `href` 또는 `data`와 같은 리소스 속성을 동적으로 채웁니다.  

### 리소스 URI 소스에 대해 검증 제어 또는 허용 목록이 구현되어 있습니까?
- [ ] 예 — 모든 동적 리소스에 대해 엄격한 허용 목록 및 오리진 검증(Origin validation)이 적용됩니다.  
- [ ] 예 — 검증이 존재하지만 취약한 차단 목록(Blacklists) 또는 쉽게 우회 가능한 정규 표현식(Regex)에 의존합니다.  
- [ ] 아니요 — 사용자가 제어하는 리소스 경로에 대해 어떠한 검증 제어도 적용되지 않습니다.  

### 대체 프로토콜이나 인코딩을 사용하여 URI 검증을 우회하는 것이 가능합니까?
- [ ] 아니요 — 리소스 필터 우회가 불가능합니다.  
- [ ] 예 — `data:`, `vbscript:` 또는 `javascript:`와 같은 다른 프로토콜을 사용하여 우회가 가능합니다.  
- [ ] 예 — 인코딩된 문자나 널 바이트(Null bytes)를 사용하여 단순 문자열 매칭을 우회하는 것이 가능합니다.  

### 공격자가 리소스 로딩을 신뢰할 수 없는 외부 도메인으로 리다이렉트할 수 있습니까?
- [ ] 아니요 — 리소스 경로는 동일 오리진(Same origin) 또는 고정된 하위 디렉토리로 제한됩니다.  
- [ ] 예 — 애플리케이션이 임의의 외부 도메인에서 스크립트나 스타일을 로드하도록 강제될 수 있습니다.  

### 리소스 변조의 최대 영향은 무엇입니까?
- [ ] 낮음 — 변조가 이미지나 민감하지 않은 텍스트와 같은 실행 불가능한 요소에만 영향을 미칩니다.  
- [ ] 중간 — 변조를 통해 CSS 인젝션(CSS Injection) 또는 상당한 수준의 UI 리드레싱이 가능합니다.  
- [ ] 높음 — 변조가 DOM 기반 XSS 또는 임의의 JavaScript 실행으로 이어집니다. *(치명적)*  

---