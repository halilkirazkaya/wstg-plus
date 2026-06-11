## WSTG-CLNT-11 — 웹 메시징(Web Messaging) 테스트

웹 메시징(Web Messaging) 또는 `postMessage`는 부모 페이지와 생성된 팝업 또는 임베디드 아이프레임(embedded iframe)과 같은 윈도우 객체 간의 교차 출처 통신(Cross-origin communication)을 가능하게 합니다. 이 메커니즘은 현대적인 웹 기능에 필수적이지만, 수신 측 애플리케이션이 발신자의 신원을 확인하지 않거나 메시지 페이로드(Payload)를 부적절하게 처리할 경우 심각한 위험을 초래합니다. 공격자는 대상 애플리케이션을 임베드한 악성 사이트를 호스팅하고, 조작된 메시지를 전송하여 의도하지 않은 작업을 유도하거나, 민감한 데이터를 탈취(Exfiltrate)하거나, DOM 기반 교차 사이트 스크립팅(DOM-based XSS)을 실행함으로써 이러한 취약점을 악용합니다. 메시지 리스너(Message listener)가 `origin` 속성을 엄격하게 검증하지 않거나 `message.data`를 위험한 실행 싱크(Execution sinks)에 직접 전달할 때 성공적인 공격이 이루어집니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 중간(Medium) / 높음(High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### 애플리케이션이 문서 간 통신을 위해 `postMessage` API를 사용합니까?
- [ ] 아니오 — 클라이언트 측 코드에 `postMessage` 리스너가 존재하지 않음  
- [ ] 예 — 내부 컴포넌트 통신을 위해 `postMessage`가 사용됨  
- [ ] 예 — 제3자 도메인 또는 위젯과의 통신을 위해 `postMessage`가 사용됨  

### 수신 메시지의 출처(Origin)가 엄격하게 검증됩니까?
- [ ] 예 — `===` 또는 동일 비교 연산자를 사용하여 엄격한 화이트리스트(Whitelist)에 대해 출처를 확인합니다 *(가장 안전)*  
- [ ] 예 — 정규식(Regex)을 사용하여 출처를 검증하지만, 로직이 우회(Bypass)될 가능성이 있습니다 (예: `^https://trusted.com`)  
- [ ] 아니오 — `origin` 속성을 확인하지 않아 모든 도메인에서 메시지를 보낼 수 있습니다  
- [ ] 아니오 — 송신자의 `targetOrigin`에 와일드카드 `*`가 사용되어 악의적인 리스너에게 데이터가 유출될 가능성이 있습니다  

### 메시지 페이로드가 애플리케이션에서 처리되기 전에 살균(Sanitize)됩니까?
- [ ] 예 — 데이터를 일반 텍스트(Plain text)로 취급하며 위험한 싱크(Sink)를 사용하지 않습니다  
- [ ] 예 — 데이터를 사용하기 전에 파싱(예: `JSON.parse`) 및 검증을 수행하며, 우회가 불가능합니다  
- [ ] 아니오 — 메시지 데이터가 `innerHTML`, `eval()`, 또는 `setTimeout()`과 같은 위험한 싱크에 직접 전달됩니다  
- [ ] 아니오 — 검증 없이 메시지 데이터를 사용하여 윈도우를 탐색하거나 `location.href`를 변경합니다  

### 공격자가 악성 메시지를 통해 권한 없는 작업을 유도하거나 데이터를 탈취할 수 있습니까?
- [ ] 아니오 — 위조된 메시지를 사용하더라도 민감한 작업이나 데이터에 접근할 수 없습니다  
- [ ] 예 — 조작된 메시지로 민감한 상태 변경(예: 비밀번호 변경, 프로필 업데이트)을 유도할 수 있습니다  
- [ ] 예 — 조작된 메시지로 인해 DOM 기반 XSS가 발생하거나 CSRF 토큰/세션 데이터가 탈취될 수 있습니다  

---