## WSTG-INPV-15 — HTTP 분할 및 요청 변조 (HTTP Splitting Smuggling)

HTTP 분할 및 요청 변조(HTTP Splitting and Smuggling) 취약점은 프런트엔드 프록시(Frontend Proxies)와 백엔드 서버(Backend Servers)가 `Content-Length` 및 `Transfer-Encoding` 헤더와 관련된 HTTP 요청 경계를 해석하고 처리하는 방식의 차이로 인해 발생합니다. 공격자는 정교하게 조작된 요청을 통해 백엔드에 숨겨진 요청을 "변조 삽입(Smuggle)"하거나 CRLF 시퀀스(CRLF sequences)를 주입하여 응답을 분할할 수 있으며, 이는 캐시 포이즈닝(Cache Poisoning), 요청 하이재킹(Request Hijacking) 또는 보안 제어 우회로 이어질 수 있습니다. 이러한 결함은 일반적으로 일관되지 않은 파싱 로직을 가진 리버스 프록시(Reverse Proxies), 로드 밸런서(Load Balancers) 또는 콘텐츠 전송 네트워크(CDNs)를 사용하는 복잡한 환경에서 나타납니다. 공격자 관점에서 이는 사용자 트래픽의 리디렉션, 민감한 세션 토큰(Session Tokens) 탈취 및 다른 사용자 세션의 컨텍스트 내에서 권한이 없는 작업 수행을 가능하게 합니다.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | 수행되지 않음 (Not Performed) |
| **Severity** | 높음(High) / 심각(Critical) |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### 환경이 CL.TE 또는 TE.CL 불일치를 통한 요청 변조(Request Smuggling)에 취약합니까?
- [ ] 아니요 — 프런트엔드 및 백엔드 서버가 요청 경계를 일관되게 처리함  
- [ ] 예 — 불일치가 존재하지만 인프라 완화 조치로 인해 익스플로잇(Exploit)이 불가능함  
- [ ] 예 — CL.TE 또는 TE.CL 스머글링이 가능하며, 숨겨진 요청이 백엔드에 도달할 수 있음 *(High)*  
- [ ] 예 — 프런트엔드 필터를 우회하기 위해 TE.TE (이중 인코딩/난독화)가 가능함 *(Critical)*  

### 헤더의 CRLF 인젝션(CRLF Injection)을 통해 HTTP 응답 분할(HTTP Response Splitting)을 달성할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 모든 헤더 입력에서 CRLF 시퀀스를 적절히 정리(Sanitize)함  
- [ ] 예 — CRLF 시퀀스가 헤더에 반영되지만 응답 분할은 불가능함  
- [ ] 예 — CRLF 인젝션이 가능하며, 헤더 주입 또는 캐시 포이즈닝이 허용됨  

### 변조된 요청을 사용하여 보안 제어(WAF/ACL)를 우회합니까?
- [ ] 아니요 — 보안 제어가 외부 요청과 변조된 요청 모두에 적용됨  
- [ ] 예 — 변조된 요청이 프런트엔드 WAF 규칙 또는 IP 기반 ACL을 우회할 수 있음  

### 다른 사용자의 세션을 하이재킹하거나 트래픽을 리디렉션할 수 있습니까?
- [ ] 아니요 — 요청/응답 스트림이 격리되어 있으며 교차될 수 없음  
- [ ] 예 — 요청 변조가 가능하며 다른 사용자의 요청을 캡처할 수 있음 *(Critical)*  
- [ ] 예 — 응답 분할이 가능하며 브라우저 측 캐시 포이즈닝 또는 XSS를 허용함  

---