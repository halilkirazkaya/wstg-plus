## WSTG-SESS-05 — 사이트 간 요청 위조(Cross-Site Request Forgery) 테스트

사이트 간 요청 위조(Cross-Site Request Forgery, CSRF)는 공격자가 피해자의 브라우저를 속여, 피해자가 현재 인증된 상태인 다른 웹사이트에서 원치 않는 작업을 수행하도록 유도하는 취약점입니다. 이 공격 방식은 세션 쿠키(Session cookies)나 권한 부여(Authorization) 헤더와 같은 "주변 자격 증명(Ambient credentials)"을 나가는 요청에 자동으로 포함시키는 브라우저의 동작 방식을 악용합니다. 공격자는 일반적으로 제3자 사이트에 악성 스크립트나 숨겨진 폼(Form)을 호스팅하여 비밀번호 변경, 이메일 주소 업데이트 또는 금융 송금 실행과 같은 상태 변경 작업(State-changing operations)을 표적으로 삼습니다. 성공적인 공격은 사용자의 인지나 동의 없이 완전한 계정 탈취(Account takeover) 또는 승인되지 않은 데이터 수정으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **테스트 상태** | 수행되지 않음 |
| **위험도 (Severity)** | 중간(Medium) / 높음(High)* |

> *취약한 동작이 계정 탈취, 권한 상승(Privilege escalation) 또는 승인되지 않은 금융 거래를 허용할 경우 위험도는 '높음'으로 분류됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**도구:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### 상태 변경 요청에 대해 Anti-CSRF 토큰이 구현되어 있습니까?
- [ ] 예 — 모든 상태 변경 작업에 고유하고 암호학적으로 강력한 토큰이 요구됨  
- [ ] 예 — 토큰이 존재하지만 세션별로 고유하지 않거나 예측 가능함  
- [ ] 아니요 — Anti-CSRF 토큰이 구현되지 않음  

### Anti-CSRF 토큰에 대한 서버 측 검증(Server-side validation)이 견고합니까?
- [ ] 예 — 서버가 토큰을 엄격하게 검증하며 우회(Bypass)가 불가능함  
- [ ] 예 — 검증이 수행되지만 토큰 파라미터를 제거함으로써 우회 가능함  
- [ ] 예 — 검증이 수행되지만 빈 값이나 더미(Dummy) 토큰을 제공하여 우회 가능함  
- [ ] 예 — 검증이 수행되지만 토큰이 사용자의 세션과 연동되어 있지 않음  

### 애플리케이션이 쉽게 우회 가능한 CSRF 방어 방식에 의존하고 있습니까?
- [ ] 아니요 — 애플리케이션이 약한 헤더나 Origin 체크에만 의존하지 않음  
- [ ] 예 — 방어 기재가 스푸핑(Spoofing)되거나 제거될 수 있는 `Referer` 또는 `Origin` 헤더에만 의존함  
- [ ] 예 — `X-Requested-With` 헤더 확인에 의존하며, 이는 CORS 설정 오류(CORS misconfigurations)를 통해 우회 가능함  

### 세션 쿠키에 `SameSite` 속성이 설정되어 있습니까?
- [ ] 예 — 모든 세션 관련 쿠키에 `SameSite` 속성이 `Strict` 또는 `Lax`로 설정됨  
- [ ] 아니요 — `SameSite` 속성이 누락되어 브라우저별 기본 동작에 의존함  
- [ ] 아니요 — `SameSite`가 `None`으로 명시적으로 설정되어 있으며, `Secure` 플래그나 추가적인 완화 조치가 없음  

### HTTP 요청 메서드를 변경하여 CSRF 방어를 우회할 수 있습니까?
- [ ] 아니요 — 사용된 HTTP 메서드에 관계없이 방어가 강제됨  
- [ ] 예 — 토큰이 `POST` 요청에서만 검증되지만, `GET`을 통해 해당 작업을 수행할 수 있음  
- [ ] 예 — 메서드 변경(예: `PUT` 또는 `DELETE`로 변경)을 통해 토큰 확인을 우회할 수 있음  

---