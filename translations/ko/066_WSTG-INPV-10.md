## WSTG-INPV-10 — IMAP SMTP 인젝션(IMAP SMTP Injection)

IMAP 및 SMTP 인젝션(IMAP SMTP Injection) 취약점은 웹 애플리케이션이 사용자 제공 데이터를 메일 서버로 전송되는 명령에 포함하기 전에 부적절하게 필터링할 때 발생합니다. 캐리지 리턴 및 라인 피드(CRLF, Carriage Return and Line Feed) 문자를 삽입함으로써, 공격자는 의도된 명령을 종료하고 추가 수신자(CC/BCC)를 추가하거나 메시지 본문을 수정하는 등의 임의의 메일 명령을 덧붙일 수 있습니다. 이러한 결함은 주로 웹투메일(Web-to-mail) 게이트웨이, 연락처 양식, 그리고 IMAP4 또는 SMTP와 같은 프로토콜을 통해 메일 서버와 직접 통신하는 이메일 클라이언트에서 자주 발견됩니다. 성공적인 공격은 스팸의 무단 릴레이(Relay), 민감한 이메일 콘텐츠의 유출(Exfiltration), 그리고 경우에 따라 메일 서버 무결성의 완전한 침해를 허용합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-10 |
| **CWE** | CWE-93 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/10-Testing_for_IMAP_SMTP_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `netcat`, `telnet`, `nmap`

### 애플리케이션이 사용자 제공 입력을 처리하여 IMAP 또는 SMTP 명령을 생성합니까?
- [ ] 아니요 — 애플리케이션이 사용자 입력을 통해 메일 서버와 상호작용하지 **않습니다**  
- [ ] 예 — 애플리케이션이 메일 서버와 상호작용하지만 보안 API 또는 라이브러리를 사용합니다  
- [ ] 예 — 애플리케이션이 사용자 제어 파라미터를 사용하여 원시(raw) 메일 명령을 생성합니다  

### 캐리지 리턴(`\r`) 및 라인 피드(`\n`) 시퀀스 삽입을 방지하기 위해 입력값이 검증됩니까?
- [ ] 예 — CRLF 문자가 **엄격하게** 필터링, 인코딩 또는 거부됩니다  
- [ ] 예 — 입력값이 엄격한 허용 목록(Allowlist)을 기준으로 검증됩니다  
- [ ] 아니요 — CRLF 문자가 필터링되지 **않아** 명령 종료 및 인젝션이 가능합니다  

### 공격자가 `Bcc:` 또는 `Cc:`와 같은 추가 메일 헤더를 삽입할 수 있습니까?
- [ ] 아니요 — 엄격한 입력 제한으로 인해 헤더 수정이 **불가능합니다**  
- [ ] 예 — CRLF 시퀀스를 통한 추가 헤더 삽입이 **가능합니다**  
- [ ] 예 — 외부 도메인으로의 메일 릴레이가 **활성화되어 있으며** 악용 가능합니다 *(Critical)*  

### 애플리케이션이 승인되지 않은 사서함에 접근하기 위해 IMAP 명령을 조작하는 것을 허용합니까?
- [ ] 아니요 — 애플리케이션이 IMAP을 사용하지 않거나 사서함 접근을 인증된 사용자로 엄격히 제한합니다  
- [ ] 예 — IMAP 명령 인젝션이 **가능하지만** 메타데이터 열거(Enumeration)로 제한됩니다  
- [ ] 예 — IMAP 명령 인젝션을 통해 다른 사용자의 이메일을 읽거나 삭제할 수 있습니다  

### 백엔드 메일 서버가 명령 파이프라이닝(Command pipelining)에 취약합니까?
- [ ] 아니요 — 메일 서버가 일괄 명령 또는 단일 세션 내의 다중 명령을 거부합니다  
- [ ] 예 — 메일 서버가 단일 입력 필드를 통해 삽입된 다중 명령을 처리합니다  

---