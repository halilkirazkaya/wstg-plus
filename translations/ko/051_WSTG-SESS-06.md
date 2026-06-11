## WSTG-SESS-06 — 로그아웃 기능 테스트(Testing for Logout Functionality)

로그아웃 기능(Logout functionality)은 사용자의 세션을 종료하고 클라이언트와 서버 양측에서 연결된 세션 식별자(Session Identifier)를 무효화하도록 설계된 중요한 보안 통제 항목입니다. 로그아웃을 제대로 구현하지 못하면 세션 고정(Session Fixation) 또는 세션 하이재킹(Session Hijacking) 공격이 발생할 수 있습니다. 사용자가 "로그아웃"한 후 머신에 접근한 공격자가 여전히 활성 상태인 세션 토큰(Session Token)을 재사용할 수 있기 때문입니다. 모의 침투 테스터(Pentester)는 브라우저에서 세션 쿠키(Session Cookie)가 삭제되는지, 서버 측 세션 상태가 명시적으로 파괴되는지, 그리고 뒤로 가기 버튼 탐색을 통해 캐시된 민감한 데이터에 접근할 수 있는지 확인하여 이를 평가합니다. 안전한 로그아웃 구현은 해당 동작이 트리거된 후, 이전 세션 토큰을 사용한 어떠한 후속 요청도 애플리케이션에서 승인되지 않도록 보장합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 보통(Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### 기능적인 로그아웃 메커니즘이 존재하며 접근 가능한가?
- [ ] 예 — 로그아웃 버튼이 존재하며 세션 종료 요청을 트리거함  
- [ ] 아니요 — 로그아웃 버튼이 **없거나** 종료 이벤트를 **트리거하지 않음**  

### 서버 측에서 세션 식별자가 무효화되는가?
- [ ] 예 — 서버가 이전 세션 토큰을 사용한 모든 후속 요청을 거부함  
- [ ] 아니요 — 서버가 로그아웃 후에도 이전 세션 토큰을 **계속 수락함**  

### 로그아웃 시 애플리케이션이 브라우저의 세션 쿠키를 삭제하는가?
- [ ] 예 — 쿠키가 만료된 날짜 또는 빈 값으로 덮어쓰여짐  
- [ ] 예 — 쿠키는 남아 있으나 서버에서 **더 이상 유효하지 않음**  
- [ ] 아니요 — 쿠키가 브라우저에 남아 있으며 원래 값을 **유지함**  

### 로그아웃 후 브라우저의 '뒤로 가기' 버튼을 통해 인증된 민감한 콘텐츠에 접근할 수 있는가?
- [ ] 아니요 — `Cache-Control: no-store` 또는 유사한 헤더가 로그아웃 후 민감한 페이지의 열람을 방지함  
- [ ] 예 — 민감한 페이지가 캐시되어 세션이 종료된 후 탐색을 통해 열람 **가능함**  

---