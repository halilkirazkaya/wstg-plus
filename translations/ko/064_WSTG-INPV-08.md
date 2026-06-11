## WSTG-INPV-08 — 서버 측 포함(Server-Side Includes, SSI) 인젝션 테스트

서버 측 포함(Server-Side Includes, SSI) 인젝션은 애플리케이션이 서버의 SSI 엔진에 의해 처리되는 HTML 파일에 사용자 입력을 포함하기 전에 이를 적절히 검증(Sanitize)하지 않을 때 발생합니다. 공격자는 `<!--#exec cmd="id" -->`와 같은 SSI 지시어(Directives)를 주입하여 임의의 셸 명령(Shell command)을 실행하거나, 로컬 파일을 읽고, 환경 변수(Environment variables)를 유출함으로써 이를 악용합니다. 이 취약점은 일반적으로 `.shtml`, `.shtm`, `.stm`과 같은 레거시 확장자를 사용하는 애플리케이션이나, 웹 서버가 표준 HTML 파일 내에서 SSI를 구문 분석(Parse)하도록 명시적으로 설정된 경우에 발견됩니다. 공격에 성공하면 공격자는 웹 서버 프로세스와 동일한 권한을 획득하게 되며, 이는 종종 전체 시스템 침해(System compromise) 또는 민감한 데이터 노출로 이어집니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### 웹 서버가 SSI 지시어를 지원하고 처리합니까?
- [ ] 아니요 — 서버가 SSI를 지원하지 않거나 `.shtml`과 같은 파일 확장자가 비활성화되어 있음  
- [ ] 예 — SSI가 활성화되어 있으나 사용자 입력이 처리된 페이지에 반영(Reflect)되지 않음  
- [ ] 예 — SSI가 활성화되어 있고 사용자 입력이 처리된 페이지에 반영됨  

### `#exec` 지시어를 통해 임의의 시스템 명령을 실행할 수 있습니까?
- [ ] 아니요 — `#exec` 지시어가 비활성화되어 있거나 서버 설정에 의해 제한됨  
- [ ] 예 — `#exec cmd` 또는 `#exec cgi` 페이로드(Payload)를 통해 명령 실행이 가능함  

### 민감한 파일이나 환경 변수의 유출이 가능합니까?
- [ ] 아니요 — `#include` 또는 `#config`와 같은 지시어가 비활성화되어 있음  
- [ ] 예 — `#include virtual`을 통해 로컬 파일(예: `/etc/passwd`) 읽기가 가능함  
- [ ] 예 — `#printenv` 또는 `#echo`를 통해 서버 환경 변수를 유출할 수 있음  

### 입력값 검증 또는 WAF 제어가 SSI 페이로드에 대해 효과적입니까?
- [ ] 예 — 엄격한 입력값 검증이 적용되어 있으며 우회가 불가능함  
- [ ] 예 — 검증/WAF(웹 애플리케이션 방화벽)가 적용되어 있으나 문자 인코딩 또는 다른 SSI 구문을 사용하여 우회가 가능함  
- [ ] 아니요 — SSI 관련 문자(`<`, `!`, `#`, `-`, `"`)에 대한 검증이나 WAF 보호가 존재하지 않음  

---