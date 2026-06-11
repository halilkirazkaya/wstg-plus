## WSTG-INPV-03 — HTTP 동사 변조(HTTP Verb Tampering) 테스트

HTTP 동사 변조(HTTP Verb Tampering)는 사용된 HTTP 메서드(HTTP Method)를 기반으로 특정 리소스에 대한 액세스 권한을 부여하는 웹 서버 및 애플리케이션 프레임워크의 취약점을 악용합니다. 공격자는 `GET` 또는 `POST`와 같은 표준 메서드 대신 `HEAD`, `PUT`, `OPTIONS` 또는 백엔드에서 일관되지 않게 처리될 수 있는 임의의 비표준 문자열로 대체하여 보안 제약을 우회하려고 시도합니다. 이 취약점은 일반적으로 Java EE web.xml 필터나 .NET 권한 부여 규칙과 같은 보안 설정에서 허용된 메서드만 명시적으로 나열하고 다른 메서드를 고려하지 못했거나, "모두 거부(deny-all)" 접근 방식 대신 "메서드별 거부(deny-by-method)" 방식을 사용했을 때 발생합니다. 성공적인 공격은 관리 기능에 대한 무단 액세스, 데이터 수정 또는 서버의 내부 구성에 관한 정보 노출로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *인증 없이 관리 기능 수행 또는 데이터 수정(`PUT`/`DELETE`)이 가능한 경우 위험도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### 애플리케이션이 비표준 또는 임의의 HTTP 메서드에 응답합니까?
- [ ] 아니요 — 애플리케이션이 명시적으로 필요한 메서드를 제외한 모든 메서드를 거부합니다.
- [ ] 예 — 애플리케이션이 표준 메서드(OPTIONS, TRACE)에 응답하지만 보안을 우회할 수는 **없습니다**.
- [ ] 예 — 애플리케이션이 임의의 문자열(예: FOO, TEST)을 허용하고 이를 `GET` 또는 `POST` 요청으로 처리합니다.

### HTTP 메서드를 변경하여 인증/권한 부여를 우회할 수 있습니까?
- [ ] 아니요 — HTTP 동사에 관계없이 보안 제어가 전역적으로 적용됩니다.
- [ ] 예 — 제어 장치가 마련되어 있으나 `HEAD` 메서드를 사용하여 우회가 **가능합니다**.
- [ ] 예 — 대체 메서드에 보안 제어가 **적용되지 않아**, 제한된 엔드포인트에 대한 무단 액세스가 허용됩니다.

### 서버에 `PUT` 또는 `DELETE`와 같은 위험한 메서드가 활성화되어 있습니까?
- [ ] 아니요 — 위험한 메서드가 **비활성화**되어 있거나 405 Method Not Allowed를 반환합니다.
- [ ] 예 — 메서드가 활성화되어 있지만 유효한 고권한 인증이 필요합니다.
- [ ] 예 — 메서드가 **활성화**되어 있으며 무단 파일 업로드 또는 리소스 삭제가 가능합니다 *(심각)*.

### `OPTIONS` 메서드가 허용된 동사에 대한 민감한 정보를 노출합니까?
- [ ] 아니요 — `OPTIONS`가 **비활성화**되어 있거나 일반적인 응답을 반환합니다.
- [ ] 예 — `OPTIONS`가 `Allow` 헤더를 반환하지만 제한된 내부 메서드는 노출하지 않습니다.
- [ ] 예 — `OPTIONS`가 추가 공격에 도움이 되는 내부 전용 또는 관리 메서드를 노출합니다.

### `TRACE` 메서드가 활성화되어 있어 교차 사이트 트레이싱(Cross-Site Tracing, XST)이 가능합니까?
- [ ] 아니요 — `TRACE` 및 `TRACK` 메서드가 **비활성화**되어 있습니다.
- [ ] 예 — `TRACE`가 **활성화**되어 있으며 민감한 쿠키/토큰을 포함한 HTTP 헤더를 그대로 반환합니다.

---