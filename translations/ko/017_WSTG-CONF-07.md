## WSTG-CONF-07 — HTTP 엄격 전송 보안(HTTP Strict Transport Security, HSTS) 테스트

HTTP 엄격 전송 보안(HSTS)은 웹 서버가 브라우저에게 오직 HTTPS를 통해서만 접속해야 함을 알리는 보안 메커니즘으로, 프로토콜 다운그레이드 공격(Protocol Downgrade Attack) 및 쿠키 하이재킹(Cookie Hijacking)을 방지합니다. HSTS는 암호화된 연결을 강제함으로써, 공격자가 초기 비보안 HTTP 요청을 가로채 TLS로의 전환을 방해하는 SSLStrip과 같은 중간자 공격(Man-in-the-Middle, MitM)을 완화합니다. 공격자 관점에서 이 헤더가 없거나 `max-age` 설정 기간이 짧을 경우, 초기 평문(Cleartext) 핸드셰이크(Handshake) 과정에서 민감한 세션 토큰이나 자격 증명을 가로챌 수 있는 기회가 제공됩니다. 이 테스트는 모든 민감한 애플리케이션 엔드포인트에서 `Strict-Transport-Security` 헤더의 존재 여부, 지속 시간 및 범위를 검증하는 데 중점을 둡니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간* |

> *애플리케이션이 개인정보(PII), 세션 토큰 또는 자격 증명을 처리하는 경우, HSTS의 부재가 MitM 공격의 성공률을 높이므로 위험도는 '중간'으로 격상됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### HTTPS 응답에 `Strict-Transport-Security` 헤더가 포함되어 있습니까?
- [ ] 예 — 모든 민감한 엔드포인트에 헤더가 **존재함**  
- [ ] 예 — 헤더가 **존재하나** 랜딩 페이지에만 설정됨  
- [ ] 아니요 — 애플리케이션 전체에서 헤더가 **누락됨**  

### `max-age` 지시문이 충분한 기간으로 설정되어 있습니까?
- [ ] 예 — `max-age`가 1년 이상(예: `31536000`)으로 설정됨  
- [ ] 예 — `max-age`가 설정되어 있으나 기간이 **너무 짧음** (예: 6개월 미만)  
- [ ] 아니요 — `max-age` 지시문이 **누락되었거나** `0`으로 설정됨 *(심각)*  

### HSTS 정책이 모든 서브도메인에 적용됩니까?
- [ ] 예 — `includeSubDomains` 지시문이 **적용됨**  
- [ ] 아니요 — `includeSubDomains` 지시문이 **누락되어** 서브도메인이 다운그레이드 공격에 취약함  
- [ ] 아니요 — 서브도메인이 존재하지 않아 해당 사항 없음  

### 애플리케이션이 HSTS 프리로딩(Preloading)에 등록되어 있습니까?
- [ ] 예 — `preload` 지시문이 **존재하며** 도메인이 HSTS 프리로드 목록에 포함되어 있음  
- [ ] 예 — `preload` 지시문은 **존재하나** 도메인이 아직 프리로드 목록에 포함되지 않음  
- [ ] 아니요 — `preload` 지시문이 **누락되어** 첫 방문 시 MitM 공격의 위험이 있음  

### HSTS 헤더가 평문 HTTP를 통해 잘못 전송되고 있습니까?
- [ ] 아니요 — 헤더가 보안 HTTPS 연결을 통해서만 **전송됨**  
- [ ] 예 — 헤더가 HTTP를 통해 **전송됨** (브라우저에 의해 무시되며 설정 세부 정보가 노출될 수 있음)  

---