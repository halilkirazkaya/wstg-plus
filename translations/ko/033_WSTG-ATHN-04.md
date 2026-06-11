## WSTG-ATHN-04 — 인증 스키마 우회 테스트 (Testing for Bypassing Authentication Schema)

인증 스키마 우회(Bypassing Authentication Schema)는 유효한 자격 증명(Credentials)을 제공하지 않고도 공격자가 보호된 리소스에 접근할 수 있게 하는 애플리케이션 로직의 결함을 식별하고 악용하는 과정을 포함합니다. 이 취약점은 일반적으로 애플리케이션이 클라이언트 측 제어에 의존하거나, 모든 요청에 대해 서버 측 권한 부여(Server-side authorization)를 강제하지 못하거나, 관리자용 "백도어(Backdoors)" 및 디버그 엔드포인트(Debug endpoints)가 운영 환경에 노출되어 있을 때 발생합니다. 공격자는 민감한 URL에 대한 강제 브라우징(Forced Browsing)을 수행하거나, `authenticated=true`와 같은 HTTP 파라미터를 조작하거나, 헤더를 스푸핑(Spoofing)하여 세션(Session)이 이미 수립된 것으로 애플리케이션을 속임으로써 이러한 약점을 악용합니다. 성공적인 공격은 인증 경계의 완전한 붕괴를 초래하며, 잠재적으로 무단 데이터 유출이나 전체 시스템 침해로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 위험 (Critical) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### 활성 세션 없이 민감한 내부 페이지에 직접 접근할 수 있습니까?
- [ ] 아니오 — 직접 접근 시 로그인 페이지로 리다이렉션되거나 403/404 응답이 반환됨  
- [ ] 예 — 일부 민감한 페이지에 접근 가능하지만 제한적이거나 민감하지 않은 데이터가 제공됨  
- [ ] 예 — 민감한 관리자용 또는 사용자 전용 페이지가 인증 없이 완전히 접근 가능함 *(Critical)*  

### 애플리케이션이 인증 상태를 결정하기 위해 클라이언트 측 플래그나 파라미터에 의존합니까?
- [ ] 아니오 — 인증 상태는 서버 측 세션 상태를 통해 엄격하게 관리됨  
- [ ] 예 — 클라이언트 측 플래그가 존재하지만 이를 수정해도 보호된 영역에 대한 접근 권한이 부여되지 않음  
- [ ] 예 — 파라미터(예: `is_authenticated=true`, `user_role=admin`)를 수정하여 인증 우회가 가능함  

### 특정 HTTP 헤더를 스푸핑하거나 조작하여 인증을 우회할 수 있습니까?
- [ ] 아니오 — `X-Forwarded-For`, `Referer` 또는 사용자 정의 헤더와 같은 헤더가 인증에 영향을 미치지 않음  
- [ ] 예 — 헤더(예: `X-Custom-IP-Authorization`, `X-Remote-User`)를 조작하는 것이 가능하며 무단 접근 권한을 얻을 수 있음  

### 표준 로그인 흐름을 우회하는 대체 경로 또는 개발 아티팩트(Artifacts)가 존재합니까?
- [ ] 아니오 — 모든 보호된 리소스는 중앙화된 운영 수준의 인증 확인을 거침  
- [ ] 예 — 레거시 엔드포인트, 디버그 경로(예: `/debug/login`) 또는 잊혀진 API 버전이 자격 증명 없이 접근 가능함  

### 애플리케이션이 높은 권한의 작업에 대해 재인증을 수행하거나 세션을 검증하지 못합니까?
- [ ] 아니오 — 높은 권한이 필요하거나 민감한 작업은 새로운 또는 유효한 세션 토큰을 요구함  
- [ ] 예 — 한 엔드포인트에서 우회 방법이 발견되면, 이를 사용하여 애플리케이션 전반에서 관리자 작업을 수행할 수 있음  

---