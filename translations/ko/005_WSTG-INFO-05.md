## WSTG-INFO-05 — 정보 유출을 위한 웹페이지 콘텐츠 리뷰 (Review Webpage Content for Information Leakage)

웹페이지 콘텐츠 리뷰는 사용자에게 의도치 않게 노출된 민감한 정보를 식별하기 위해 HTML, JavaScript 및 CSS 파일을 포함한 애플리케이션 응답을 수동 및 자동으로 검사하는 작업을 포함합니다. 공격자는 개발자 주석, 숨겨진 폼 필드 (Hidden form fields), 내부 서버 경로, 하드코딩된 API 키 (Hardcoded API keys) 및 추가 공격 단계에 도움이 될 수 있는 디버깅 정보 (Debugging information)를 찾기 위해 클라이언트 측 소스 코드 (Client-side source code)를 면밀히 조사합니다. 이러한 정보 유출 (Information Leakage)은 주로 개발자가 운영 환경으로 이전하기 전에 메타데이터나 디버깅 코드를 제거하는 것을 잊었을 때 발생하며, 이는 잠재적으로 로직 결함이나 백엔드 인프라 세부 정보를 드러낼 수 있습니다. 공격자의 관점에서 이는 보안 경고를 발생시키거나 서버의 백엔드 로직과 상호작용하지 않고도 애플리케이션의 내부 구조를 파악하고 간과된 진입점 (Entry points)을 발견할 수 있는 효율적인 방법입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-05 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 낮음(Low) / 중간(Medium)* |

> *민감한 하드코딩된 자격 증명 (Credentials), 개인 키 (Private keys) 또는 내부 환경 변수가 발견될 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Web_Page_Content_for_Information_Leakage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**도구:** `Burp Suite (Target/Proxy)`, `Browser Developer Tools`, `SecretFinder`, `grep`, `truffleHog`, `LinkFinder`

### 소스 코드에 민감한 정보를 포함하는 개발자 주석이 존재합니까?
- [ ] 아니요 — 주석이 없거나 일반적이고 민감하지 않은 주석만 발견됨  
- [ ] 예 — 주석이 존재하지만 민감한 로직이나 데이터는 포함되어 있지 않음  
- [ ] 예 — 주석이 내부 경로, SQL 쿼리 또는 관리 지침을 **드러냄**  
- [ ] 예 — 주석이 자격 증명 또는 민감한 개발자 메모를 **드러냄** *(높음)*  

### 숨겨진 HTML 입력 필드에 민감한 데이터나 상태 정보가 포함되어 있습니까?
- [ ] 아니요 — 숨겨진 필드가 없거나 민감하지 않은 토큰만 포함됨  
- [ ] 예 — 숨겨진 필드가 상태 관리에 사용되지만 **암호화** 또는 **서명**되어 있음  
- [ ] 예 — 숨겨진 필드에 **평문(Plaintext)** 비밀번호, 사용자 역할 또는 내부 ID가 포함되어 있음  

### JavaScript 파일에 하드코딩된 비밀 정보, API 키 또는 내부 IP 주소가 존재합니까?
- [ ] 아니요 — JS 파일에서 민감한 문자열이나 비밀 정보가 발견되지 않음  
- [ ] 예 — JS 파일에 **적절한** 제한 사항이 적용된 공개 API 키가 포함되어 있음  
- [ ] 예 — JS 파일에 **보호되지 않은** API 키, 내부 IP 또는 프라이빗 엔드포인트(Private endpoints)가 포함되어 있음  
- [ ] 예 — JS 파일에 **하드코딩된** 관리자 자격 증명 또는 개인 키가 포함되어 있음 *(심각)*  

### 애플리케이션이 소스에서 프레임워크 버전이나 내부 파일 경로를 드러냅니까?
- [ ] 아니요 — 프레임워크 정보와 파일 경로가 **축소(Minified)** 또는 **난독화(Obfuscated)**되어 있음  
- [ ] 예 — 프레임워크 이름과 버전이 **노출**되어 있으나 패치된 상태임  
- [ ] 예 — 내부 파일 경로 또는 서버 절대 경로가 소스 코드에 **노출**되어 있음  

### 축소(Minification) 또는 난독화(Obfuscation)의 부재로 인해 소스 코드가 유출에 취약합니까?
- [ ] 아니요 — 운영 코드가 **완전히 축소**되었으며 주석이 제거됨  
- [ ] 예 — 코드가 축소되었으나 소스에 주석이 **남아 있음**  
- [ ] 예 — 소스 맵 (`.map` 파일)에 **공개적으로 접근** 가능하여 소스 코드를 완전히 재구성할 수 있음  

---