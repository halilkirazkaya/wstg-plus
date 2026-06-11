## WSTG-CLNT-05 — CSS 주입(CSS Injection) 테스트

CSS 주입(CSS Injection)은 애플리케이션이 적절한 새니타이징(Sanitization) 또는 이스케이프(Escaping) 처리 없이 사용자 입력값이 웹 페이지의 CSS(Cascading Style Sheets)에 영향을 미치도록 허용할 때 발생합니다. 흔히 단순한 외관상의 문제로 치부되기도 하지만, 공격자는 속성 선택자(Attribute Selectors)와 background-image 속성을 사용하여 외부 요청을 트리거함으로써 CSRF 토큰(CSRF Tokens)이나 세션 ID(Session IDs)와 같은 민감한 데이터를 탈취하는 데 CSS 주입을 악용할 수 있습니다. 이 취약점은 일반적으로 사용자 설정(예: 사용자 정의 테마, 글꼴 색상)이 `<style>` 블록이나 인라인 `style` 속성 내에 반영되는 엔드포인트에서 발생합니다. 공격자 관점에서 성공적인 공격은 UI 리드레싱(UI Redressing), 승인되지 않은 레이아웃 수정을 통한 피싱(Phishing), 또는 엄격한 콘텐츠 보안 정책(Content Security Policy, CSP)으로 인해 JavaScript 기반 공격이 차단된 환경에서의 은밀한 데이터 탈취로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *주입 지점에서 CSRF 토큰과 같은 민감한 속성을 탈취할 수 있거나, 민감한 워크플로우에서 UI 리드레싱에 사용될 수 있는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### 애플리케이션이 CSS 컨텍스트 내에서 사용자 제어 입력값을 반영합니까?
- [ ] 아니요 — 입력값이 스타일 태그, 속성 또는 CSS 파일에 반영되지 않음  
- [ ] 예 — 입력값이 반영되지만 안전한 영숫자 값으로 엄격히 제한됨  
- [ ] 예 — 입력값이 `style` 속성 또는 `<style>` 블록 내에 반영됨  

### CSS 전용 메타 문자 및 함수가 적절하게 새니타이징 되었습니까?
- [ ] 예 — `{`, `}`, `:`와 같은 문자와 `url()`과 같은 함수가 **적절히 이스케이프 처리됨**  
- [ ] 예 — 새니타이징이 적용되어 있으나 인코딩 또는 CSS 주석을 통한 **우회가 가능함**  
- [ ] 아니요 — CSS 메타 문자에 대해 새니타이징이 적용되지 않음  

### CSS 선택자를 사용한 데이터 탈취가 가능합니까?
- [ ] 아니요 — 선택자가 외부 요청을 트리거하거나 데이터를 유출할 수 없음  
- [ ] 예 — 속성 선택자와 `background-image`를 통한 **부분적인 데이터 탈취가 가능함**  
- [ ] 예 — 자동화된 브루트 포스(Brute Force) 기법을 사용하여 민감한 토큰의 **전체 탈취가 가능함**  

### 콘텐츠 보안 정책(CSP)이 CSS 주입 위험을 완화합니까?
- [ ] 예 — `style-src`가 'self'로 제한되어 있고 `img-src` 또는 `connect-src`가 **제한되어 있음**  
- [ ] 예 — CSP가 존재하지만 'unsafe-inline'을 사용하거나 와일드카드 외부 도메인을 허용함  
- [ ] 아니요 — CSS를 통한 외부 리소스 로드를 방지하기 위한 CSP가 구현되지 않음  

### 취약점을 악용하여 UI 리드레싱 또는 피싱을 수행할 수 있습니까?
- [ ] 아니요 — 주입 범위가 너무 제한적이어서 레이아웃을 크게 수정할 수 없음  
- [ ] 예 — **UI 수정이 가능**하며, 악성 요소를 겹쳐 놓을 수 있음  
- [ ] 예 — 전체 페이지 레이아웃 **제어가 가능**하여, 매우 정교한 피싱 공격이 가능함  

---