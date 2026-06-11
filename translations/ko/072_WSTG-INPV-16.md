## WSTG-INPV-16 — HTTP 요청 스머글링 (HTTP Request Smuggling)

HTTP 요청 스머글링 (HTTP Request Smuggling, HRS)은 로드 밸런서 (Load Balancer)와 백엔드 웹 서버 (Back-end Web Server)와 같은 HTTP 서버 체인이 주로 `Content-Length` 및 `Transfer-Encoding` 헤더의 충돌로 인해 요청의 경계를 서로 다르게 해석할 때 발생합니다. 공격자는 이러한 비동기화 (Desynchronization) 상태를 악용하여 백엔드 서버의 요청 버퍼에 항목을 "밀어 넣어(smuggle)", 결과적으로 다음 정당한 사용자의 요청 앞에 공격자가 제어하는 세그먼트가 접두사로 붙게 만듭니다. 이 기법을 통해 세션 하이재킹 (Session Hijacking), 보안 통제 우회 (Bypassing Security Controls), 캐시 포이즈닝 (Cache Poisoning) 등 심각한 공격이 가능해집니다. 취약점 공격은 주로 타이밍 기반 분석 (Timing-based Analysis)이나 후속 요청을 서버에 보낼 때 백엔드로부터 예기치 않은 응답이 오는지 관찰함으로써 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각 (Critical) / 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**도구:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### 프런트엔드와 백엔드 서버가 `Transfer-Encoding: chunked` 헤더를 일관되게 처리합니까?
- [ ] 예 — 두 서버 모두 청크 인코딩을 동일하게 처리하며 비동기화가 **가능하지 않음**  
- [ ] 아니오 — 서버가 헤더를 다르게 해석하지만 새니타이징 (Sanitization) 또는 정규화 (Normalization)가 **적용됨**  
- [ ] 아니오 — CL.TE (Content-Length/Transfer-Encoding) 비동기화가 **가능함** *(심각)*  
- [ ] 아니오 — TE.CL (Transfer-Encoding/Content-Length) 비동기화가 **가능함** *(심각)*  

### 공격자가 스머글링된 요청을 사용하여 서버 또는 클라이언트 측 캐시를 오염시킬 수 있습니까?
- [ ] 아니오 — 캐싱이 **비활성화**되어 있거나 포이즈닝에 취약하지 않음  
- [ ] 예 — 스머글링된 요청이 사용자를 악성 도메인으로 리다이렉트하거나 캐시 포이즈닝을 통해 스크립트를 삽입할 수 **있음**  

### 요청 연결 (Request Concatenation)을 통해 다른 사용자의 요청이나 세션 토큰을 탈취할 수 있습니까?
- [ ] 아니오 — 스머글링이 사용자 간 데이터 노출을 허용하지 **않음**  
- [ ] 예 — 스머글링을 통해 다음 사용자의 요청을 공격자가 제어하는 `POST` 본문에 추가하여 데이터 유출 (Exfiltration)이 **가능함**  

### 인프라에서 HTTP/2를 사용하며, H2.CL 또는 H2.TE 비동기화에 취약합니까?
- [ ] 아니오 — 애플리케이션이 HTTP/1.1만 사용하거나 HTTP/2가 다운그레이드 (Downgrade) 없이 안전하게 구현됨  
- [ ] 예 — HTTP/2에서 HTTP/1.1로의 일반 텍스트 다운그레이드가 발생하며 비동기화가 **가능함**  

### 잘못된 형식의 헤더(예: 공백이 포함된 `Transfer-Encoding:  chunked`)가 안전하게 처리됩니까?
- [ ] 예 — 잘못된 형식의 헤더가 모든 계층에서 거부되거나 정규화됨  
- [ ] 아니오 — 잘못된 형식이나 모호한 헤더의 일관되지 않은 처리로 인해 TE.TE 비동기화가 **가능함**  

---