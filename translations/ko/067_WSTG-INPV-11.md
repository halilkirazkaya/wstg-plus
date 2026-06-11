## WSTG-INPV-11 — 코드 인젝션(Code Injection)

코드 인젝션(Code Injection)은 애플리케이션이 `eval()`, `exec()`, 또는 `system()`과 같은 안전하지 않은 언어별 함수를 통해 런타임 환경 내에서 사용자 제공 코드 조각을 평가할 때 발생합니다. 이 취약점은 기반 운영체제(OS)의 쉘을 대상으로 하는 커맨드 인젝션(Command Injection)과는 달리, 프로그래밍 언어의 실행 컨텍스트(Execution Context)(예: PHP, Python, Node.js)를 대상으로 한다는 점에서 차이가 있습니다. 공격자는 이러한 지점을 악용하여 임의의 로직을 실행하거나, 환경 변수를 유출하거나, 서버에서 완전한 원격 코드 실행(Remote Code Execution, RCE)을 달성할 수 있습니다. 모의 해킹 전문가(Pentester)는 입력값이 실행 가능한 코드로 해석될 수 있는 수학식 처리, 서버 사이드 템플릿(Server-side Template), 또는 동적 구성 매개변수와 같은 기능을 조사해야 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-11 |
| **CWE** | CWE-94 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11-Testing_for_Code_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/deserialization  
* https://portswigger.net/web-security/llm-attacks  

**도구:** `Burp Suite (Repeater/Intruder)`, `FFUF`, `PayloadsAllTheThings`, `Commix`

### 애플리케이션 기능 중 언어별 평가 함수를 통해 동적 입력을 실행하는 기능이 있습니까?
- [ ] 아니요 — 코드베이스에 동적 평가 함수가 **존재하지 않습니다**  
- [ ] 예 — `eval()`, `exec()`, 또는 `include()`와 같은 함수가 사용되지만 사용자 입력을 **수락하지 않습니다**  
- [ ] 예 — 함수가 사용되며 사용자가 제공한 입력을 처리합니다  

### 평가 전에 입력값에 대한 입력 유효성 검사 또는 새니타이제이션(Sanitization) 메커니즘이 적용됩니까?
- [ ] 예 — 입력값이 **하드코딩된 화이트리스트(Whitelist)**에 대해 엄격하게 검증됩니다  
- [ ] 예 — 블랙리스팅(Blacklisting)을 통해 입력값이 필터링되지만, 우회가 **가능합니다**  
- [ ] 아니요 — 입력값이 평가 함수로 직접 전달되며 **아무런 통제**가 적용되지 않습니다  

### 실행 환경이 시스템 수준의 접근을 방지하기 위해 샌드박스(Sandbox) 처리되거나 제한되어 있습니까?
- [ ] 예 — 시스템 호출(System Call)을 할 수 없는 매우 제한된 샌드박스 내에서 실행이 이루어집니다  
- [ ] 예 — 일부 제한이 존재하지만, 샌드박스 탈출(Sandbox Escape)이 **가능합니다**  
- [ ] 아니요 — 환경이 기반 언어 API 및 OS 함수에 대한 완전한 접근을 허용합니다 *(심각)*  

### 코드 인젝션을 사용하여 민감한 정보를 유출할 수 있습니까?
- [ ] 아니요 — 실행 결과가 확인되지 않는 블라인드(Blind) 상태이며, 대역 외 통신(Out-of-band communication)이 **불가능합니다**  
- [ ] 예 — HTTP/DNS 요청을 통해 환경 변수나 로컬 파일을 유출할 수 **있습니다**  
- [ ] 예 — 실행된 코드의 결과가 애플리케이션 응답에 **직접 반영**됩니다  

### 애플리케이션이 원격 파일 인클루전(Remote File Inclusion) 또는 동적 라이브러리 로딩을 허용합니까?
- [ ] 아니요 — 로컬에 미리 정의된 코드 경로만 실행할 수 있습니다  
- [ ] 예 — 애플리케이션이 원격 또는 공격자가 제어하는 소스로부터 코드를 로드하고 실행하도록 강제할 수 **있습니다**  

---