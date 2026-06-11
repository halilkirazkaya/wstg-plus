## WSTG-BUSL-08 — 예상치 못한 파일 유형 업로드 테스트 (Test Upload of Unexpected File Types)

예상치 못한 파일 유형을 업로드하면 공격자가 애플리케이션에서 의도하지 않은 파일을 제출하여 비즈니스 로직(Business Logic) 제약을 우회할 수 있으며, 이는 원격 코드 실행(Remote Code Execution, RCE), 교차 사이트 스크립팅(Cross-Site Scripting, XSS) 또는 서비스 거부(Denial of Service, DoS)로 이어질 수 있습니다. 공격자는 파일 확장자, MIME 유형(MIME types), 매직 바이트(Magic Bytes)를 수정하여 서버 측 검증 로직이 악성 콘텐츠를 수락하도록 속임으로써 이러한 취약점을 탐색합니다. 이는 일반적으로 프로필 사진 업로드, 문서 관리 시스템 또는 지원 티켓 첨부 파일 등 백엔드(Back-end)에서 불충분한 검증이 존재하는 곳에서 발생합니다. 공격에 성공하여 업로드된 파일이 실행될 경우 호스트 서버가 장악되거나, 파일이 잘못된 `Content-Type`으로 다른 사용자에게 전달될 경우 클라이언트 측 공격으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-08 |
| **CWE** | CWE-434 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음(High) / 치명적(Critical) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/08-Test_Upload_of_Unexpected_File_Types  
* https://hacktricks.wiki/en/pentesting-web/file-upload/index.html  
* https://portswigger.net/web-security/file-upload  

**도구:** `Burp Suite (Repeater/Intruder)`, `ExifTool`, `Ffuf`, `CyberChef`

### 애플리케이션이 파일 업로드를 특정 확장자 세트로 제한합니까?
- [ ] 예 — 엄격한 확장자 화이트리스트(Whitelist)가 적용되어 있으며 우회가 **불가능함**  
- [ ] 예 — 확장자 화이트리스트가 존재하지만 이중 확장자 또는 널 바이트(Null Bytes)를 통한 우회가 **가능함**  
- [ ] 아니요 — 모든 파일 확장자를 업로드할 수 **있음**  

### 파일 확장자 이외에 파일 콘텐츠에 대한 검증이 수행됩니까?
- [ ] 예 — 서버 측 로직이 매직 바이트(Magic Bytes)를 확인하고 심층 콘텐츠 검사(Deep content inspection)를 수행함  
- [ ] 예 — 서버 측 로직이 `Content-Type` 헤더를 통해서만 MIME 유형을 확인함  
- [ ] 아니요 — 확장자 확인 후 어떠한 콘텐츠 검증도 **적용되지 않음**  

### 공격자가 웹쉘(Web Shell) 또는 스크립트를 업로드하여 코드를 실행할 수 있습니까?
- [ ] 아니요 — 업로드된 파일이 실행 권한이 없는 디렉토리 또는 스토리지 버킷(S3/Azure Blob)에 저장됨  
- [ ] 예 — 파일이 웹에서 접근 가능한 디렉토리에 저장되지만, 서버 설정을 통해 실행이 **비활성화됨**  
- [ ] 예 — 업로드된 악성 스크립트가 예측 가능한 URL 경로를 통해 직접 **실행될 수 있음** *(치명적)*  

### 업로드된 파일 유형을 통해 XSS와 같은 클라이언트 측 공격이 가능합니까?
- [ ] 아니요 — 파일이 `Content-Disposition: attachment` 및 `X-Content-Type-Options: nosniff`와 함께 제공됨  
- [ ] 예 — SVG 또는 HTML 파일이 업로드되고 브라우저에서 렌더링되어 XSS로 이어질 수 **있음**  
- [ ] 예 — 이미지 메타데이터(EXIF)가 처리되어 새니타이제이션(Sanitization) 없이 UI에 **반영됨**  

---