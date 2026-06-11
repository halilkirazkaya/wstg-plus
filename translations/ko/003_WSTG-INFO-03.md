## WSTG-INFO-03 — 정보 유출(Information Leakage) 방지를 위한 웹 서버 메타파일(Webserver Metafiles) 검토

`robots.txt`, `sitemap.xml`, `security.txt`와 같은 웹 서버 메타파일은 크롤러(Crawlers)를 안내하고 관리용 메타데이터를 제공하기 위한 용도이지만, 종종 의도치 않게 민감한 디렉터리 구조나 숨겨진 엔드포인트(Endpoints)를 노출합니다. 공격자는 이러한 파일을 분석하여 애플리케이션의 공격 표면(Attack Surface)을 열거(Enumerate)하며, 연결되지 않은 관리자 패널, 백업 디렉터리 또는 개발 환경을 가리키는 경우가 많은 "Disallow" 지시문을 식별합니다. 검색 엔진 지침 외에도 `.well-known/` 디렉터리의 파일이나 `humans.txt`와 같은 파일은 기술 스택(Technology Stacks), 개발자 정보 또는 조직의 보안 담당자 연락처를 공개할 수 있습니다. 이러한 파일을 체계적으로 검토하면 테스터는 공격적인 무차별 대입(Brute Force) 탐색을 수행하지 않고도 구조적 및 기술적 통찰력을 얻을 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-03 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 정보성(Informational)* |

> *위험도는 메타파일이 인증되지 않은 관리 인터페이스나 민감한 설정 백업을 노출할 경우 중간(Medium)으로 상향됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `wget`, `Burp Suite`, `FFUF`, `GoBuster`

### `robots.txt` 파일이 존재하며 민감한 디렉터리를 노출하고 있습니까?
- [ ] 아니오 — `robots.txt`가 존재하지 않음  
- [ ] 예 — `robots.txt`가 존재하지만 표준 공용 경로만 포함함  
- [ ] 예 — `robots.txt`가 민감한 디렉터리 경로 또는 관리 인터페이스를 노출함  
- [ ] 예 — `robots.txt`가 주석(Comments)에 자격 증명(Credentials)이나 특정 소프트웨어 버전을 노출함  

### `sitemap.xml` 파일이 존재하며 숨겨진 애플리케이션 구조를 노출하고 있습니까?
- [ ] 아니오 — `sitemap.xml`이 존재하지 않음  
- [ ] 예 — `sitemap.xml`이 존재하지만 공용 URL만 나열함  
- [ ] 예 — `sitemap.xml`이 접근 가능한 비연결 엔드포인트 또는 내부 전용 리소스를 나열함  

### 보안 및 조직 메타파일이 기술적 세부 정보를 노출하고 있습니까?
- [ ] 아니오 — 루트 또는 `.well-known/` 디렉터리에서 메타파일이 발견되지 않음  
- [ ] 예 — `security.txt` 또는 `humans.txt`가 존재하지만 표준 정보만 포함함  
- [ ] 예 — 메타파일이 사회 공학(Social Engineering) 또는 추가 공격에 도움이 될 수 있는 내부 호스트 이름, 개발자 신원 또는 기술 스택 세부 정보를 노출함  

### 레거시(Legacy) 또는 프레임워크 전용 메타파일이 존재합니까?
- [ ] 아니오 — 프레임워크(Framework) 전용 파일(예: `info.php`, `composer.json`, `package.json`)이 발견되지 않음  
- [ ] 예 — `README.md`, `CHANGELOG.txt` 또는 `.DS_Store`와 같은 파일이 존재하지만 삭제(Sanitized)됨  
- [ ] 예 — 프레임워크 또는 버전 전용 파일이 존재하며 정확한 기술 핑거프린팅(Technology Fingerprinting)을 허용함  

---