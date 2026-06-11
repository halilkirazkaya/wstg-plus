## WSTG-ATHZ-02 — 권한 부여 스키마 우회 테스트 (Testing for Bypassing Authorization Schema)

권한 부여 스키마 우회(Bypassing Authorization Schema)는 애플리케이션이 액세스 제어(Access Control)를 제대로 시행하지 못하여, 공격자가 의도된 권한을 벗어나 리소스나 기능에 접근할 수 있을 때 발생합니다. 이 취약점은 일반적으로 공격자가 동일한 등급의 다른 사용자 데이터에 접근하는 수평적 권한 상승(Horizontal Privilege Escalation)이나, 낮은 권한의 사용자가 관리자 권한을 획득하는 수직적 권한 상승(Vertical Privilege Escalation)으로 나타납니다. 공격자는 리소스 ID(Resource ID)와 같은 요청 파라미터를 조작하거나, 세션 토큰(Session Token)을 수정하고, `X-Original-URL`과 같은 HTTP 헤더(HTTP Header)를 활용하여 경로 기반 제한을 우회함으로써 이러한 결함을 악용합니다. 강력한 권한 부여를 보장하는 것은 데이터 기밀성을 유지하고 전체 시스템을 손상시킬 수 있는 무단 관리 작업을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음(High) / 심각(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### 동일한 역할을 가진 사용자 간의 수평적 권한 상승이 차단되어 있습니까?
- [ ] 예 — 모든 리소스 요청에 권한 부여 검사가 **적용되며** 우회가 **불가능함**  
- [ ] 예 — 권한 부여 검사가 **적용되지만** IDOR 또는 파라미터 변조(Parameter Tampering)를 통해 우회 가능함  
- [ ] 아니요 — 사용자가 단순히 리소스 ID를 변경함으로써 다른 사용자의 데이터에 접근할 수 **있음** *(High)*  

### 낮은 권한의 사용자에 대한 수직적 권한 상승이 차단되어 있습니까?
- [ ] 아니요 — 애플리케이션에 단 하나의 권한 레벨만 존재함  
- [ ] 예 — 낮은 권한의 사용자가 관리 기능에 접근할 수 **없음**  
- [ ] 예 — 관리 기능이 UI에서는 숨겨져 있으나 직접 URL을 통해 접근할 수 **있음**  
- [ ] 아니요 — 낮은 권한의 사용자가 역할 또는 권한을 조작하여 관리자 작업을 수행할 수 **있음** *(Critical)*  

### HTTP 메소드 변조(HTTP Verb Tampering)를 사용하여 권한 부여를 우회할 수 있습니까?
- [ ] 아니요 — 사용된 HTTP 메소드에 관계없이 액세스 제어가 시행됨  
- [ ] 예 — 메소드를 변경(예: `GET`에서 `POST` 또는 `PUT`으로)하면 권한 부여 검사가 **우회됨**  

### 인증 없이 관리자 또는 제한된 엔드포인트(Endpoint)에 접근할 수 있습니까?
- [ ] 아니요 — 모든 민감한 엔드포인트에 유효한 세션과 적절한 권한이 필요함  
- [ ] 예 — 공격자가 직접 URL을 아는 경우 일부 관리자 엔드포인트에 접근 가능함  
- [ ] 예 — 민감한 기능에 대한 비인증 접근이 **가능함** *(Critical)*  

### HTTP 헤더를 통해 경로 기반 권한 부여 우회가 가능합니까?
- [ ] 아니요 — 애플리케이션이 헤더 기반 우회에 취약하지 **않음**  
- [ ] 예 — `X-Original-URL`, `X-Rewrite-URL`, 또는 `X-Forwarded-For`와 같은 헤더를 사용하여 액세스 제어를 **우회할 수 있음**  

---