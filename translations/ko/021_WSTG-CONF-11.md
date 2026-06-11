## WSTG-CONF-11 — 클라우드 스토리지 테스트 (Test Cloud Storage)

클라우드 스토리지 설정 오류(Cloud Storage Misconfigurations) 테스트는 Amazon S3, Azure Blobs 또는 Google Cloud Storage와 같이 공개적으로 접근 가능한 객체 스토리지(Object Storage) 서비스를 식별하고 점검하는 과정을 포함합니다. 이러한 리소스는 과도하게 허용된 접근 제어 목록(ACLs)이나 ID 및 액세스 관리(IAM) 정책으로 인해 백업, 소스 코드, 개인정보(PII) 또는 설정 파일과 같은 민감한 데이터가 노출되는 경우가 많습니다. 공격자는 JavaScript 파일, HTML 소스 및 브루트 포스(Brute Force) 기법을 통한 정찰(Reconnaissance)을 수행하여 버킷(Bucket) 이름을 열거하고, 데이터 유출(Data Exfiltration)이나 무단 파일 수정을 위한 진입점을 찾습니다. 취약점 악용은 전체 데이터 유출이나 신뢰할 수 있는 도메인에서 악성 파일을 배포하는 능력 등 심각한 영향으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-11 |
| **CWE** | CWE-922 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 심각(Critical)* |

> *개인정보(PII), 자격 증명 또는 운영 백업에 접근 가능하거나, 인증되지 않은 쓰기 권한이 활성화된 경우 위험도는 '심각(Critical)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `aws-cli`, `gsutil`, `azure-cli`, `S3Scanner`, `CloudEnum`, `FFUF`, `Burp Suite`

### 클라우드 스토리지 엔드포인트(버킷/컨테이너)가 식별 및 열거되었습니까?
- [ ] 아니요 — 클라우드 스토리지 엔드포인트가 사용되지 않거나 발견되지 않음  
- [ ] 예 — 코드 분석 또는 트래픽 모니터링을 통해 스토리지 엔드포인트가 식별됨  
- [ ] 예 — 이름 지정 규칙 브루트 포스를 통해 스토리지 엔드포인트가 발견됨  

### 인증되지 않은 사용자가 클라우드 스토리지의 콘텐츠를 나열할 수 있습니까?
- [ ] 아니요 — 버킷 목록 나열(Listing)이 비활성화되어 있으며 403 Forbidden 또는 404 Not Found를 반환함  
- [ ] 예 — 목록 나열이 활성화되어 있으나 민감한 파일은 존재하지 않음  
- [ ] 예 — 목록 나열이 활성화되어 있으며 민감한 파일 이름/메타데이터가 노출됨  

### 식별된 버킷에서 민감한 파일을 다운로드할 수 있습니까?
- [ ] 아니요 — 파일이 IAM 정책으로 보호되거나 유효한 인증이 필요함  
- [ ] 예 — 파일에 접근 가능하지만 쉽게 추측할 수 없는 서명된 URL(Signed URL)이 필요함  
- [ ] 예 — 민감한 파일(백업, `.env`, PII)에 공개적으로 접근하여 다운로드할 수 있음 *(심각/Critical)*  

### 인증되지 않은 파일 업로드 또는 수정이 허용됩니까?
- [ ] 아니요 — 인증되지 않은 업로드 또는 수정이 불가능함  
- [ ] 예 — 인증된 업로드가 필요하지만 취약한 정책 조건을 통해 우회(Bypass)가 가능함  
- [ ] 예 — 인증되지 않은 파일 쓰기, 덮어쓰기 또는 삭제가 가능함 *(심각/Critical)*  

### 스토리지 엔드포인트의 교차 출처 리소스 공유(CORS) 정책이 제한적입니까?
- [ ] 예 — CORS가 비활성화되어 있거나 특정 신뢰할 수 있는 오리진(Origin)으로 제한됨  
- [ ] 아니요 — CORS가 와일드카드 `*` 오리진으로 활성화되어 있어 무단 웹 기반 접근을 허용함  

---