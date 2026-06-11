## WSTG-BUSL-09 — 악성 파일 업로드 테스트 (Test Upload of Malicious Files)

파일 업로드 기능은 사용자가 서버로 데이터를 전송할 수 있게 하며, 애플리케이션 환경에 악성 콘텐츠를 유입시키는 직접적인 벡터를 제공합니다. 공격자는 취약한 검증 로직을 악용하여 웹쉘(Web Shells), 악성코드(Malware) 또는 SVG나 HTML과 같이 특별히 제작된 파일을 업로드함으로써 원격 코드 실행(Remote Code Execution, RCE)이나 교차 사이트 스크립팅(Cross-Site Scripting, XSS)을 수행합니다. 이 취약점은 주로 프로필 설정, 문서 관리 시스템, 첨부 파일 처리 기능 등 서버가 파일 유형, 내용 및 실행 권한을 적절히 확인하지 않는 곳에서 발견됩니다. 성공적인 공격은 종종 전체 시스템 장악, 데이터 유출 또는 내부 네트워크 공격을 위한 거점으로 서버가 이용되는 결과로 이어집니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-09 |
| **CWE** | CWE-434 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 심각 (High / Critical) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/09-Test_Upload_of_Malicious_Files  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**도구:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`, `Weevely`

### 애플리케이션이 파일 업로드 기능을 제공합니까?
- [ ] 아니요 — 파일 업로드 기능이 존재하지 않음  
- [ ] 예 — 인증된 사용자를 위한 기능이 존재함  
- [ ] 예 — **인증되지 않은** 사용자도 기능을 사용할 수 있음 *(심각)*  

### 서버 측에서 파일 확장자 검증이 강제됩니까?
- [ ] 예 — 엄격한 확장자 허용 목록(Allowlist)이 **적용**되어 있으며 우회가 **불가능함**  
- [ ] 예 — 허용 목록을 사용하지만 대소문자 구분이나 이중 확장자를 통해 우회가 **가능함**  
- [ ] 예 — 차단 목록(Blocklist)을 사용하며, 대체 확장자(예: `.phtml`, `.asa`)로 우회 **가능함**  
- [ ] 아니요 — 서버 측에서 확장자 검증이 **적용되지 않음**  

### 확장자나 MIME 유형 이외에 파일 콘텐츠 검증이 수행됩니까?
- [ ] 예 — 애플리케이션이 매직 바이트(Magic Bytes)를 확인하고 심층 콘텐츠 검사(Deep Content Inspection)를 수행함  
- [ ] 예 — 애플리케이션이 쉽게 변조 가능한 `Content-Type` 헤더만 확인함  
- [ ] 아니요 — 애플리케이션이 검증을 위해 파일 확장자에만 전적으로 의존함  

### 업로드된 파일이 실행 권한이 있는 디렉토리에 저장됩니까?
- [ ] 아니요 — 파일이 전용 스토리지 서비스(예: S3) 또는 실행 권한이 없는 디렉토리에 저장됨  
- [ ] 예 — 파일이 웹 서버에 저장되지만 설정을 통해 실행이 **비활성화**되어 있음  
- [ ] 예 — 파일이 웹에서 접근 가능한 디렉토리에 저장되며 실행이 **활성화**되어 있음 *(심각)*  

### 파일명 조작을 통해 보안 필터를 우회할 수 있습니까?
- [ ] 아니요 — 파일명이 서버에 의해 정화(Sanitization)되거나 무작위로 재생성됨  
- [ ] 예 — 널 바이트(Null Bytes, 예: `shell.php%00.jpg`)를 사용하여 필터를 우회할 수 있음  
- [ ] 예 — 경로 탐색(Path Traversal) 문자(예: `../../`)를 사용하여 임의의 디렉토리에 파일을 업로드할 수 있음  

---