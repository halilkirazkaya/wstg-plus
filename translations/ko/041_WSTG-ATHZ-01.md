## WSTG-ATHZ-01 — 디렉토리 트래버설 및 파일 인클루전 테스트 (Testing Directory Traversal File Include)

디렉토리 트래버설(Directory Traversal) 및 파일 인클루전(File Inclusion) 취약점은 애플리케이션이 충분한 검증이나 정제(Sanitization) 없이 사용자 제어 입력을 사용하여 파일 또는 디렉토리 경로를 생성할 때 발생합니다. 공격자는 의도된 디렉토리 외부로 이동하기 위해 `../`와 같은 시퀀스를 주입하여 이 결함을 악용하며, 이를 통해 민감한 시스템 파일, 설정 데이터 또는 애플리케이션 소스 코드에 접근할 수 있습니다. 로컬 파일 인클루전(Local File Inclusion, LFI) 또는 리모트 파일 인클루전(Remote File Inclusion, RFI)이 포함된 심각한 사례의 경우, 공격자는 악성 스크립트를 포함하거나 로그 포이즈닝(Log Poisoning) 및 PHP 래퍼(PHP Wrappers)를 활용하여 원격 코드 실행(Remote Code Execution, RCE)을 달성할 수 있습니다. 이러한 취약점은 일반적으로 동적 콘텐츠 로딩, 템플릿 엔진 또는 서버 측(Server-side) 로직이 파일 시스템 경로를 처리하는 이미지 검색 엔드포인트에서 사용되는 파라미터(Parameter)에 존재합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-01 |
| **CWE** | CWE-22 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 치명적 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/01-Testing_Directory_Traversal_File_Include  
* https://hacktricks.wiki/en/pentesting-web/file-inclusion/index.html  
* https://portswigger.net/web-security/file-path-traversal  

**도구:** `Burp Suite`, `FFUF`, `DotDotPwn`, `LFI Suite`, `Wfuzz`

### 파라미터가 서버 측 처리를 위해 파일 이름이나 경로를 수락합니까?
- [ ] 아니요 — 파일 시스템과 상호작용하는 파라미터가 없는 것으로 보입니다.
- [ ] 예 — 파라미터가 존재하지만 파일 식별자에 대한 엄격한 화이트리스트(Allowlist)를 사용합니다.
- [ ] 예 — 파라미터가 직접적인 파일 이름이나 경로를 수락합니다.

### 디렉토리 트래버설 시퀀스를 방지하기 위해 입력값이 정제되었습니까?
- [ ] 예 — 입력값이 엄격한 화이트리스트를 통해 검증되며 트래버설 시퀀스 사용이 **불가능합니다**.
- [ ] 예 — `../` 시퀀스를 제거하여 입력값을 정제하지만, 재귀적 우회가 **가능합니다**.
- [ ] 아니요 — 경로 관련 입력에 정제 또는 검증이 **적용되지 않았습니다**.

### 트래버설 시퀀스를 통해 제한된 디렉토리 외부의 파일에 접근하는 것이 가능합니까?
- [ ] 아니요 — 애플리케이션 또는 OS가 정의된 범위 밖의 파일 접근을 차단합니다.
- [ ] 예 — 웹 루트(Web root) 내의 파일에 대한 접근이 **가능합니다**.
- [ ] 예 — 민감한 시스템 파일(예: `/etc/passwd`, `C:\Windows\win.ini`)에 대한 접근이 **가능합니다** *(치명적)*.

### 서버가 원격 URL의 포함(RFI)을 허용합니까?
- [ ] 아니요 — 서버/애플리케이션 수준에서 RFI가 **비활성화되어 있습니다**.
- [ ] 예 — 원격 파일을 포함할 수 있지만 실행은 **불가능합니다**.
- [ ] 예 — 원격 파일을 포함할 수 있으며 서버에서 실행이 가능합니다 *(치명적)*.

### 인코딩이나 특수 문자를 사용하여 필터를 우회할 수 있습니까?
- [ ] 아니요 — 필터가 다양한 인코딩 및 널 바이트(Null byte)를 효과적으로 처리합니다.
- [ ] 예 — URL 인코딩(URL encoding), 이중 URL 인코딩 또는 16비트 유니코드를 사용하여 우회가 **가능합니다**.
- [ ] 예 — 널 바이트 인젝션(Null byte injection, `%00`) 또는 파일 시스템 래퍼(예: `php://filter`)를 사용하여 우회가 **가능합니다**.

---