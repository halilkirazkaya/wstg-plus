## WSTG-INPV-19 — 서버 측 요청 위조 (Server-Side Request Forgery, SSRF) 테스트

서버 측 요청 위조(Server-Side Request Forgery, SSRF)는 공격자가 웹 애플리케이션을 조작하여 서버에서 내부 또는 외부 리소스로 무단 요청을 보내도록 할 때 발생합니다. URL, 호스트 이름 또는 IP 주소를 기대하는 파라미터를 조작함으로써, 공격자는 방화벽이나 ACL(Access Control List)과 같은 네트워크 수준의 통제를 우회하여 내부 네트워크 세그먼트를 조사하거나, 클라우드 메타데이터 서비스(IMDS)에 액세스하거나, 취약한 내부 API 및 데이터베이스와 상호 작용할 수 있습니다. 이 취약점은 일반적으로 이미지 가져오기, PDF 생성, 웹훅(Webhooks) 또는 URL을 통한 파일 업로드 기능에서 나타납니다. 공격자 관점에서 SSRF는 격리된 환경으로 침투(Pivot)하고, 민감한 설정 데이터를 탈취(Exfiltrate)하며, Redis나 Memcached와 같은 내부 서비스와의 상호 작용을 통해 잠재적으로 원격 코드 실행(Remote Code Execution, RCE)을 달성할 수 있는 강력한 수단(Primitive)입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **테스트 상태** | 미실행 |
| **위험도** | 높음 / 치명적 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**도구:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### 애플리케이션이 사용자 제공 URL 또는 호스트 이름을 처리합니까?
- [ ] 아니오 — 외부 URL 또는 호스트 이름을 허용하는 기능이 없습니다.
- [ ] 예 — 기능이 존재하지만 입력값이 허용 목록(Allow-list)에 따라 엄격하게 검증됩니다.
- [ ] 예 — 기능이 존재하며 사용자 제공 URL을 허용합니다.

### 요청된 대상에 대해 검증 통제 또는 차단 목록(Deny-list)이 적용되어 있습니까?
- [ ] 예 — 신뢰할 수 있는 도메인/IP에 대한 엄격한 허용 목록(Allow-list)이 강제됩니다.
- [ ] 예 — 차단 목록(Deny-list)이 사용됩니다 (예: `127.0.0.1`, `169.254.169.254` 차단).
- [ ] 아니오 — 입력값에 대해 검증이나 필터링이 적용되지 않습니다.

### 인코딩, 리다이렉트 또는 DNS 리바인딩(DNS Rebinding)을 통해 필터를 우회할 수 있습니까?
- [ ] 아니오 — 필터가 견고하며 리다이렉트/DNS 해석을 안전하게 처리합니다.
- [ ] 예 — URL 인코딩 또는 대체 IP 형식(16진수, 8진수)을 사용하여 우회가 가능합니다.
- [ ] 예 — 내부 대상을 향하는 3xx HTTP 리다이렉트를 통해 우회가 가능합니다.
- [ ] 예 — 내부 IP로 해석되도록 하는 DNS 리바인딩 공격을 통해 우회가 가능합니다.

### 애플리케이션이 내부 메타데이터 서비스 또는 로컬 인터페이스에 도달할 수 있습니까?
- [ ] 아니오 — 로컬 네트워크 및 클라우드 메타데이터 엔드포인트에 연결할 수 없습니다.
- [ ] 예 — 로컬 호스트(`127.0.0.1`) 또는 루프백(Loopback) 서비스에 액세스가 가능합니다.
- [ ] 예 — 클라우드 메타데이터 서비스(예: AWS/GCP/Azure IMDS)에 액세스가 가능합니다. *(치명적)*

### SSRF 응답의 성격은 무엇입니까?
- [ ] Blind — 사용자에게 응답이나 메타데이터가 반환되지 않습니다.
- [ ] Partial — 응답 메타데이터(헤더, 크기, 시간 등)가 사용자에게 반환됩니다.
- [ ] Full — 내부 리소스로부터의 전체 응답 본문(Response Body)이 사용자에게 반사(Reflected)되어 나타납니다.

---