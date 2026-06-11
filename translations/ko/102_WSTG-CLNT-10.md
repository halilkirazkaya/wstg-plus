## WSTG-CLNT-10 — 웹소켓(WebSockets) 테스트

웹소켓(WebSocket) 테스트는 클라이언트와 서버 사이에 설정된 전 이중(Full-duplex) 통신 채널에 중점을 둡니다. 이는 전통적인 HTTP 요청-응답 패턴을 우회합니다. 침투 테스트 수행자(Pentesters)는 핸드쉐이크(Handshake) 과정의 보안성, 크로스 사이트 웹소켓 하이재킹(Cross-Site WebSocket Hijacking, CSWSH) 방어 기제의 존재 여부, 그리고 메시지 페이로드(Payload)에 적용된 입력값 검증의 엄격함을 평가합니다. 익스플로잇(Exploit)은 일반적으로 활성화된 세션을 하이재킹(Hijacking)하여 실시간으로 데이터를 유출하거나, 지속적인 소켓을 통해 커맨드 인젝션(Command Injection) 또는 SQL 인젝션(SQL Injection, SQLi)과 같은 서버 측 취약점을 유발하는 악성 페이로드를 주입하는 방식으로 이루어집니다. 많은 기존 웹 애플리케이션 방화벽(Web Application Firewalls, WAF)이 웹소켓 트래픽을 효과적으로 검사하지 못하기 때문에, 이 프로토콜은 종종 경계 방어를 우회하는 은밀한 벡터를 제공합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**도구:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### 웹소켓 연결이 보안 채널을 통해 설정되어 있습니까?
- [ ] 예 — 모든 연결에 대해 `wss://` (WebSocket Secure)가 강제됩니다.  
- [ ] 아니오 — `ws://`가 사용되어 중간자 공격(Person-in-the-Middle, PITM) 도청이 가능합니다.  

### 애플리케이션이 크로스 사이트 웹소켓 하이재킹(CSWSH)으로부터 보호되고 있습니까?
- [ ] 아니오 — 서버가 핸드쉐이크 동안 `Origin` 헤더를 엄격하게 검증합니다.  
- [ ] 예 — `Origin` 헤더 검증이 약하거나 null 또는 스푸핑(Spoofed)된 origin을 통해 우회될 수 있습니다.  
- [ ] 예 — `Origin` 헤더 검증이 수행되지 않아 크로스 사이트 공격자가 연결을 시작할 수 있습니다 *(치명적, Critical)*.  

### 웹소켓 메시지 페이로드에 입력값 검증 및 필터링(Sanitization)이 적용되었습니까?
- [ ] 예 — 모든 수신 메시지가 필터링되고 스키마(Schema)에 대해 검증됩니다.  
- [ ] 예 — 일부 검증이 존재하지만 인코딩되거나 비표준 페이로드를 사용한 우회가 가능합니다.  
- [ ] 아니오 — 메시지가 직접 처리되어 인젝션(XSS, SQLi, RCE) 또는 로직 조작이 가능합니다.  

### 모든 웹소켓 메시지에 대해 인증(Authentication) 및 인가(Authorization) 검사가 수행됩니까?
- [ ] 예 — 모든 메시지에 대해 세션이 확인되거나 프로토콜이 토큰을 사용하는 상태 비저장(Stateless) 방식입니다.  
- [ ] 예 — 핸드쉐이크 시 인증이 수행되지만, 특정 작업에 대한 인가는 메시지별로 강제되지 않습니다.  
- [ ] 아니오 — 인증이 핸드쉐이크 시에만 확인되므로, 세션 하이재킹을 통해 전체 권한 획득이 가능합니다.  

### 서버가 웹소켓에 속도 제한(Rate Limiting) 또는 리소스 제약을 구현하고 있습니까?
- [ ] 예 — 속도 제한 및 최대 메시지 크기가 활성화되어 강제됩니다.  
- [ ] 아니오 — 제한이 존재하지 않아 메시지 플러딩(Flooding) 또는 대용량 페이로드를 통한 서비스 거부(Denial of Service, DoS) 공격이 가능합니다.  

---