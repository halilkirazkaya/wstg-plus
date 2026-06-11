## WSTG-CLNT-08 — 크로스 사이트 플래싱 테스트 (Testing for Cross Site Flashing)

크로스 사이트 플래싱 (Cross-Site Flashing, XSF)은 플래시 (Flash, SWF) 파일이 사용자가 제어하는 입력값을 부적절하게 처리할 때 발생하는 클라이언트 측 취약점으로, 공격자가 악성 액션스크립트 (ActionScript) 코드를 실행하거나 브라우저의 자바스크립트 (JavaScript) 환경으로 연결할 수 있게 합니다. `ExternalInterface.call`, `getURL`, 또는 `loadMovie`와 같은 싱크(sink)로 전달되는 `FlashVars` 또는 URL 쿼리 문자열과 같은 파라미터를 조작함으로써, 공격자는 취약한 도메인의 컨텍스트에서 세션 하이재킹 (Session Hijacking) 및 데이터 유출 (Data Exfiltration)을 포함한 동작을 수행할 수 있습니다. 이 취약점은 주로 SWF 파일을 여전히 호스팅하고 있는 레거시 기업용 애플리케이션에서 발견되며, 불충분한 입력값 검증 (Input Validation)으로 인해 플래시 무비가 크로스 사이트 스크립팅 (Cross-Site Scripting, XSS)의 벡터로 재사용되거나 허용적인 교차 도메인 설정을 통해 동일 출처 정책 (Same-Origin Policy, SOP)을 우회하는 데 악용될 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 중간 (Medium) / 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### 애플리케이션에 레거시 Adobe Flash (.swf) 파일이 호스팅되어 있습니까?
- [ ] 아니요 — 애플리케이션 범위 내에 호스팅되거나 참조되는 플래시 파일이 없습니다.  
- [ ] 예 — SWF 파일이 존재하지만 정적 콘텐츠만 제공합니다.  
- [ ] 예 — SWF 파일이 존재하며 `FlashVars` 또는 URL 문자열을 통해 동적 파라미터를 허용합니다.  

### ActionScript 싱크로 전달되는 입력값이 살균(sanitize) 처리됩니까?
- [ ] 예 — 모든 입력값이 엄격하게 검증되며 ActionScript 싱크를 조작할 수 없습니다.  
- [ ] 예 — 검증이 적용되어 있으나 특정 인코딩 기술을 통해 우회가 가능합니다.  
- [ ] 아니요 — 입력값이 `ExternalInterface.call` 또는 `getURL`과 같은 민감한 싱크로 직접 전달됩니다. *(위험)*  

### 플래시 파일을 사용하여 임의의 자바스크립트(XSS)를 실행할 수 있습니까?
- [ ] 아니요 — `ExternalInterface`가 비활성화되어 있거나 `allowScriptAccess` 파라미터가 `never`로 설정되어 있습니다.  
- [ ] 예 — `allowScriptAccess`가 `sameDomain`으로 설정되어 있으나 SWF가 대상 도메인에 호스팅되어 있습니다.  
- [ ] 예 — `allowScriptAccess`가 `always`로 설정되어 있어 모든 도메인에서 XSS가 가능합니다.  

### `crossdomain.xml` 정책이 승인되지 않은 교차 출처 요청을 방지합니까?
- [ ] 예 — `crossdomain.xml`이 제한적이며 신뢰할 수 있는 특정 출처만 허용합니다.  
- [ ] 아니요 — `crossdomain.xml` 파일이 누락되었습니다.  
- [ ] 예 — 정책이 과도하게 허용적입니다 (예: `<allow-access-from domain="*" />`).  

### SWF 파일이 공격자가 제어하는 외부 무비를 강제로 로드하도록 할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 외부 SWF 파일을 로드하도록 강제할 수 없습니다.  
- [ ] 예 — `loadMovie` 또는 `loadMovieNum` 함수가 검증되지 않은 외부 URL을 허용합니다.  

---