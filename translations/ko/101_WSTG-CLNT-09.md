## WSTG-CLNT-09 — 클릭재킹(Clickjacking) 테스트

클릭재킹(Clickjacking)은 UI 레드레싱(UI redressing)이라고도 하며, 공격자가 투명하거나 불투명한 레이어를 사용하여 사용자가 의도했던 상위 페이지의 버튼이나 링크가 아닌 다른 페이지의 요소를 클릭하도록 속이는 기법입니다. 이 취약점은 사용자 동작의 무결성을 훼손하여, 피해자를 대신해 승인되지 않은 데이터 수정, 계정 변경 또는 금융 송금 등을 실행할 가능성이 있습니다. 공격자는 일반적으로 제어 하에 있는 악성 사이트의 `<iframe>` 내에 대상 웹사이트를 삽입하고, 그 위에 기만적인 콘텐츠나 유도 장치를 겹쳐서 이를 악용합니다. 이는 "계정 삭제" 버튼, 비밀번호 재설정 양식 또는 관리 설정 패널과 같이 민감한 상태 변경 작업(state-changing actions)을 수행하는 페이지에서 가장 빈번하게 발생합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *클릭재킹을 통해 민감한 작업(예: 자금 이체, 계정 삭제 또는 권한 상승)을 실행할 수 있는 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**도구:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### 애플리케이션이 권한이 없는 프레이밍(framing)을 방지하기 위해 서버 측 헤더를 구현하고 있습니까?
- [ ] 예 — `X-Frame-Options` 또는 `Content-Security-Policy`가 **적용되어** 프레이밍을 방지함 *(가장 안전)*  
- [ ] 예 — 헤더가 존재하지만 **잘못 설정됨** (예: 브라우저 지원이 광범위하지 않은 `X-Frame-Options: ALLOW-FROM` 사용)  
- [ ] 아니요 — 프레임 보호 헤더가 **적용되지 않음**  

### `Content-Security-Policy` (CSP)의 `frame-ancestors` 지시문이 구현되어 있습니까?
- [ ] 예 — `frame-ancestors 'none'` 또는 `'self'`가 **적용됨**  
- [ ] 예 — `frame-ancestors`가 존재하지만 신뢰할 수 없거나 너무 광범위한 출처(origin)를 **허용함**  
- [ ] 아니요 — 지시문이 **누락되었거나** CSP가 구현되지 않음  

### 레거시 브라우저 지원을 위해 `X-Frame-Options` (XFO) 헤더가 올바르게 설정되어 있습니까?
- [ ] 예 — `DENY` 또는 `SAMEORIGIN`이 **적용됨**  
- [ ] 예 — XFO가 존재하지만 CSP `frame-ancestors` 지시문이 있어 최신 브라우저에서 **무시됨**  
- [ ] 아니요 — XFO 헤더가 **누락되었거나** 안전하지 않은 값으로 설정됨  

### 알려진 기법을 사용하여 프레이밍 보호를 우회할 수 있습니까?
- [ ] 아니요 — 표준 기법(Double-framing 등)을 통한 우회가 **불가능함**  
- [ ] 예 — `frame-ancestors` 화이트리스트의 취약한 정규식 또는 로직으로 인해 보호를 **우회할 수 있음**  
- [ ] 예 — 애플리케이션이 자바스크립트 기반의 프레임 버스팅(Frame-busting)에만 의존하여 보호를 **우회할 수 있음**  

### 클릭재킹 개념 증명(PoC)을 통해 민감한 작업을 악용할 수 있습니까?
- [ ] 아니요 — 프레이밍이 차단되었거나 민감한 작업에 접근할 수 없음  
- [ ] 예 — UI 레드레싱이 **가능하지만** 복잡한 사용자 상호작용이 필요함  
- [ ] 예 — UI 레드레싱이 **가능하며** 즉각적인 상태 변경 작업을 허용함 *(심각함)*  

---