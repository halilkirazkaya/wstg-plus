## WSTG-ATHN-05 — 취약한 암호 기억 기능 테스트 (Testing for Vulnerable Remember Password)

"암호 기억(Remember Me)" 또는 지속적 로그인(Persistent Login) 기능은 클라이언트 측에 토큰(Token)이나 자격 증명(Credential)을 저장하여 브라우저 재시작 후에도 사용자의 인증 상태를 유지하도록 설계되었습니다. 이 기능이 평문 암호(Plaintext Password), 취약한 해시(Weak Hash), 또는 낮은 엔트로피(Low-entropy) 토큰을 쿠키에 저장하는 등 부적절하게 구현된 경우, 사용자는 교차 사이트 스크립팅(XSS), 물리적 접근 또는 네트워크 가로채기를 통한 계정 탈취(Account Takeover) 위험에 노출됩니다. 모의 침투 테스터는 이러한 지속성 토큰의 엔트로피, 저장 메커니즘에 적용된 보안 플래그, 세션의 생명주기 등을 평가하여 지속적 접근의 편의성이 다요소 인증(MFA)이나 세션 무효화(Session Invalidation)와 같은 중요한 보안 통제를 우회(Bypass)하지 않는지 확인해야 합니다. 취약점 공격은 주로 이러한 장기 생존 토큰을 수집하여 원래의 자격 증명 없이 계정에 무단으로 접근하는 방식으로 이루어집니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *지속적 쿠키에 평문 자격 증명 또는 솔트가 없는 해시(Unsalted Hash)가 저장되어 있거나, 토큰이 다요소 인증(MFA)을 우회하는 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### 애플리케이션이 "암호 기억" 또는 "로그인 상태 유지" 기능을 구현하고 있습니까?
- [ ] 아니오 — 해당 기능이 애플리케이션에 존재하지 않음  
- [ ] 예 — 기능이 존재하며 암호학적으로 강력하고 예측 불가능한 토큰을 사용함  
- [ ] 예 — 기능이 존재하지만 예측 가능하거나 낮은 엔트로피 토큰을 사용함 *(중간)*  
- [ ] 예 — 기능이 존재하며 평문 자격 증명 또는 Base64로 인코딩된 암호를 저장함 *(높음)*  

### 지속적 쿠키에 보안 플래그가 올바르게 적용되어 있습니까?
- [ ] 예 — `HttpOnly`, `Secure`, `SameSite` 플래그가 **적용됨**  
- [ ] 예 — 일부 플래그가 적용되었으나 `HttpOnly` 플래그가 **누락됨**  
- [ ] 예 — 일부 플래그가 적용되었으나 HTTPS 환경에서 `Secure` 플래그가 **누락됨**  
- [ ] 아니오 — 지속적 쿠키에 보안 플래그가 **전혀 적용되지 않음**  

### 수동 로그아웃 후에도 "암호 기억" 토큰이 유효하게 유지됩니까?
- [ ] 아니오 — 로그아웃 즉시 서버 측에서 토큰이 무효화됨  
- [ ] 예 — 토큰이 유효하게 유지되지만 민감한 작업 수행 시 암호 입력을 요구함  
- [ ] 예 — 토큰이 유효하게 유지되며 재인증 **없이** 전체 계정 권한에 접근 가능함 *(중간)*  

### 암호 변경 시 지속적 세션이 종료됩니까?
- [ ] 예 — 사용자가 암호를 변경할 때 모든 지속적 토큰이 폐기됨  
- [ ] 아니오 — 암호 재설정/변경이 **수행된 후에도** "암호 기억" 토큰이 유효하게 유지됨 *(높음)*  

### 이후 방문 시 지속적 토큰이 다요소 인증(MFA)을 우회합니까?
- [ ] 아니오 — "암호 기억" 토큰이 있더라도 MFA가 요구됨  
- [ ] 예 — MFA를 우회하지만, 토큰이 특정 기기 지문(Device Fingerprint)에 결합되어 있음  
- [ ] 예 — 어떤 기기에서든 지속적 쿠키만으로 MFA를 **완전히 우회**함 *(높음)*  

---