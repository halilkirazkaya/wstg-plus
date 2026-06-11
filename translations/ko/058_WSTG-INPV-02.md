## WSTG-INPV-02 — 저장형 크로스 사이트 스크립팅 (Stored Cross Site Scripting, XSS)

저장형 크로스 사이트 스크립팅 (Stored Cross Site Scripting, XSS)은 지속성 XSS (Persistent XSS)라고도 하며, 애플리케이션이 사용자로부터 데이터를 수신하여 적절한 검증 (Validation) 또는 인코딩 (Encoding) 없이 영구적인 데이터베이스, 파일 시스템 또는 기타 저장 매체에 저장할 때 발생합니다. 이후 희생자가 이 정화되지 않은 데이터를 검색하여 표시하는 페이지로 이동하면, 애플리케이션의 오리진 (Origin) 하에 브라우저 컨텍스트 (Browser context) 내에서 악성 스크립트가 실행됩니다. 이 취약점은 희생자가 특수하게 조작된 링크를 클릭할 필요가 없기 때문에 특히 위험합니다. 해당 페이지를 열람하는 모든 사용자가 공격 대상이 되며, 이는 세션 하이재킹 (Session hijacking), 무단 행위 또는 자격 증명 수집 (Credential harvesting)으로 이어질 수 있습니다. 공격자는 주로 댓글 섹션, 사용자 프로필, 메시지 게시판, 그리고 입력 값이 다른 사용자나 관리자에게 일관되게 렌더링되는 관리자 로그를 목표로 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**도구:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### 다른 사용자에게 나중에 표시하기 위해 사용자 입력을 저장하는 진입점이 존재합니까?
- [ ] 아니요 — 애플리케이션이 추후 렌더링을 위해 사용자 제어 입력을 저장하지 않음  
- [ ] 예 — 입력이 저장되지만 (예: 프로필, 댓글), HTML 컨텍스트 내에서 렌더링되지 않음  
- [ ] 예 — 입력이 저장되며 다른 사용자나 관리자에게 렌더링됨  

### 저장된 데이터가 렌더링될 때 출력 인코딩 또는 정화 (Sanitization)가 적용됩니까?
- [ ] 예 — 컨텍스트 인지 출력 인코딩이 **적용되었으며** 우회 (Bypass)가 **불가능함** *(가장 안전)*  
- [ ] 예 — 정화 (예: `DOMPurify`)가 **적용되었으나** 설정 오류를 통해 우회가 **가능함**  
- [ ] 아니요 — 인코딩이나 정화 **없이** 데이터가 DOM에 직접 렌더링됨 *(높음)*  

### 대체 페이로드(Payload)를 사용하여 입력 값 검증 또는 정화를 우회할 수 있습니까?
- [ ] 아니요 — 필터가 다양한 인코딩, 비표준 태그 및 이벤트 핸들러를 안전하게 처리함  
- [ ] 예 — HTML 엔티티 인코딩 또는 특정 태그 (예: `<svg>`, `<img>`)를 사용하여 우회가 **가능함**  
- [ ] 예 — 폴리글로트 페이로드 (Polyglot payloads) 또는 변이 기반 XSS (mXSS)를 통해 우회가 **가능함**  

### 콘텐츠 보안 정책 (Content Security Policy, CSP)이 보조 방어 계층을 제공합니까?
- [ ] 예 — 엄격한 CSP가 **활성화되어 있으며** 인라인 스크립트 및 승인되지 않은 소스의 실행을 방지함  
- [ ] 예 — CSP가 **활성화되어 있으나** 잘못 설정됨 (예: `unsafe-inline` 또는 광범위한 `script-src`)  
- [ ] 아니요 — 애플리케이션에 CSP가 **활성화되지 않음**  

### "저장형 XSS를 통한 RCE" 또는 기타 고영향 체인 (블라인드 XSS)이 가능합니까?
- [ ] 아니요 — 영향이 클라이언트 측 브라우저 컨텍스트로 제한됨  
- [ ] 예 — 저장형 XSS가 내부 패널의 관리자를 대상으로 하는 블라인드 XSS (Blind XSS)에 사용될 수 **있음**  
- [ ] 예 — 저장형 XSS가 다른 취약점 (예: CSRF, 민감한 데이터 유출)과 연결될 수 **있음**  

---