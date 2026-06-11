## WSTG-INPV-12 — 커맨드 인젝션(Command Injection)

커맨드 인젝션(Command Injection)은 애플리케이션이 폼 입력, HTTP 헤더 또는 쿠키와 같이 안전하지 않은 사용자 제공 데이터를 시스템 셸(System shell)로 전달할 때 발생합니다. 이 취약점은 공격자가 웹 애플리케이션 프로세스의 권한으로 서버에서 임의의 운영체제 명령(Arbitrary operating system commands)을 실행할 수 있게 합니다. 공격자 관점에서는 세미콜론, 파이프 또는 백틱과 같은 셸 메타 문자(Metacharacters)를 사용하여 의도된 실행 흐름에 악성 명령을 연결하는 방식으로 익스플로잇(Exploit)합니다. 그 결과는 대개 치명적이며, 전체 시스템 침해, 무단 데이터 유출(Data exfiltration) 또는 내부 네트워크에서의 측면 이동(Lateral movement)으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**도구:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### 애플리케이션이 시스템 수준의 명령이나 셸 실행 파일을 호출합니까?
- [ ] 아니요 — 애플리케이션이 OS 셸과 상호작용하지 않음  
- [ ] 예 — 애플리케이션이 시스템 작업을 위해 내부 API 또는 상위 레벨 라이브러리를 사용함  
- [ ] 예 — 애플리케이션이 `system()`, `exec()`, 또는 `popen()`과 같은 시스템 호출을 통해 셸 명령을 직접 호출함  

### 사용자 입력이 시스템 호출로 전달되기 전에 검증(Validation)되거나 필터링(Sanitization)됩니까?
- [ ] 예 — 엄격한 화이트리스트(Allow-listing) 및 입력 검증이 적용됨  
- [ ] 예 — 필터링 또는 이스케이프(Escaping) 처리가 사용되지만 우회(Bypass)가 가능함  
- [ ] 아니요 — 사용자 입력이 검증 없이 셸로 직접 전달됨  

### 셸 메타 문자를 사용하여 명령 실행 흐름을 조작할 수 있습니까?
- [ ] 아니요 — 메타 문자가 올바르게 필터링되거나 이스케이프됨  
- [ ] 예 — 명령 실행 결과가 응답에 반영되는 인밴드(In-band) 실행이 가능함  
- [ ] 예 — 실행 결과가 응답에 반영되지 않는 블라인드(Blind) 실행이 가능함  

### 호스트에서 아웃오브밴드(Out-of-band, OOB) 데이터 유출이 가능합니까?
- [ ] 아니요 — 서버의 외부 통신(Egress)이 차단되었거나 OOB 신호(DNS/HTTP) 전송에 실패함  
- [ ] 예 — DNS 또는 HTTP 기반 OOB 신호를 통한 데이터 유출이 가능함  

### 애플리케이션 프로세스가 승격된 시스템 권한으로 실행됩니까?
- [ ] 아니요 — 애플리케이션이 전용 저권한 서비스 계정으로 실행됨  
- [ ] 예 — 애플리케이션이 `root`, `Administrator` 또는 고권한 사용자로 실행됨 *(Critical)*  

---