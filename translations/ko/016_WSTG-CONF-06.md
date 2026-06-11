## WSTG-CONF-06 — HTTP 메소드 테스트 (Test HTTP Methods)

HTTP 메소드 테스트는 웹 서버 및 애플리케이션에서 지원하는 동사(Verbs)를 식별하여 필요한 기능만 노출되도록 보장하는 과정을 포함합니다. 표준 `GET` 및 `POST` 외에도 서버는 `PUT`, `DELETE` 또는 `TRACE`와 같은 위험한 메소드를 의도치 않게 활성화할 수 있으며, 이는 공격자가 악성 파일을 업로드하거나 기존 콘텐츠를 삭제하고, 또는 교차 사이트 트레이싱(Cross-Site Tracing, XST)을 통해 세션 쿠키를 탈취할 수 있게 합니다. 모의해킹 전문가(Pentester)는 `Allow` 또는 `Public`과 같은 응답 헤더를 조사하고 메소드 오버라이딩(Method Overriding) 기법이나 동사 변조(Verb Tampering)를 사용하여 제한을 우회하고 비인가 기능에 접근을 시도합니다. 이 테스트는 공격 표면(Attack Surface)을 강화하고 서버 침해 또는 무단 데이터 조작으로 이어질 수 있는 설정 오류를 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간* |

> *`PUT` 또는 `DELETE`가 비인가 파일 조작을 허용하거나 `TRACE`가 세션 토큰 탈취를 용이하게 하는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### 애플리케이션 전반에서 `PUT` 또는 `DELETE`와 같은 위험한 HTTP 메소드가 비활성화되어 있습니까?
- [ ] 예 — 안전한 메소드(GET, POST, HEAD)만 **활성화되어 있음**  
- [ ] 아니요 — `PUT` 또는 `DELETE`가 **활성화되어 있으나** 유효한 인증이 필요함  
- [ ] 아니요 — `PUT` 또는 `DELETE`가 **활성화되어 있으며** 인증 없이 접근 가능함 *(치명적)*  

### 교차 사이트 트레이싱(XST)을 방지하기 위해 `TRACE` 메소드가 비활성화되어 있습니까?
- [ ] 예 — `TRACE` 메소드가 **비활성화되었거나** 405 Method Not Allowed를 반환함  
- [ ] 아니요 — `TRACE` 메소드가 **활성화되어 있으나** 민감한 헤더를 반영하지 않음  
- [ ] 아니요 — `TRACE` 메소드가 **활성화되어 있으며** `Cookie` 또는 `Authorization` 헤더를 반영함 *(중간)*  

### 서버에서 HTTP 메소드 오버라이딩(Method Overriding, 예: `X-HTTP-Method-Override`)을 지원합니까?
- [ ] 아니요 — 서버가 메소드 오버라이드 헤더를 **처리하지 않음**  
- [ ] 예 — 서버가 오버라이드 헤더를 처리하지만 접근 제어가 **적용됨**  
- [ ] 예 — 서버가 오버라이드 헤더를 처리하며 접근 제어 우회가 **가능함**  

### 임의의 또는 잘못된 형식의 HTTP 메소드에 대해 서버가 보안상 안전하게 응답합니까?
- [ ] 예 — 서버가 405 Method Not Allowed 또는 501 Not Implemented를 반환함  
- [ ] 아니요 — `JEFF` 또는 `TEST`와 같은 임의의 메소드에 대해 서버가 200 OK 또는 500 Error를 반환함  
- [ ] 아니요 — 임의의 메소드를 사용하여 인증 또는 인가 필터 우회가 가능함  

### 정보 노출을 최소화하도록 `OPTIONS` 메소드가 제한되거나 설정되어 있습니까?
- [ ] 예 — `OPTIONS`가 비활성화되어 있거나 최소한의 `Allow` 헤더만 반환함  
- [ ] 아니요 — `OPTIONS`가 DAV 또는 디버깅 동사를 포함하여 활성화된 광범위한 메소드를 노출함  

---