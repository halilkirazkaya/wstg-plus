## WSTG-INFO-01 — 정보 유출을 위한 검색 엔진 탐색 및 정찰(Search Engine Discovery and Reconnaissance for Information Leakage) 수행

검색 엔진 탐색(Search Engine Discovery)은 공개 검색 엔진, 캐시된 페이지 및 인덱싱 서비스를 활용하여 대상 조직이 의도치 않게 노출한 정보를 식별하는 과정을 포함합니다. 공격자는 고급 검색 연산자인 구글 도크(Google Dorks) 및 서드파티(Third-party) 인덱싱 서비스를 사용하여 공개적으로 접근해서는 안 되는 민감한 파일, 디렉터리, 로그인 포털, 오류 메시지 및 메타데이터를 발견합니다. 이 정찰(Reconnaissance) 단계는 대상과 직접적인 상호작용이 전혀 필요하지 않아 탐지가 사실상 불가능하면서도, 노출된 관리자 패널, 설정 파일, 데이터베이스 덤프(Database Dump), 내부 문서와 같은 가치가 높은 침투 지점을 드러낼 수 있기 때문에 매우 중요합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-01 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  
* https://portswigger.net/web-security/information-disclosure  

**도구:** `Google Dorks`, `theHarvester`, `Shodan`, `Censys`, `Wayback Machine`, `Recon-ng`, `SearchDiggity`

### 검색 엔진에 의해 민감한 파일이나 디렉터리가 인덱싱되어 있습니까?
- [ ] 아니요 — 검색 엔진 결과에서 민감한 콘텐츠가 발견되지 않았습니다.  
- [ ] 예 — 설정 파일, 백업 또는 내부 문서가 인덱싱되어 있습니다. *(치명적)*  

### 구글 도킹(Google Dorking)을 통해 노출된 관리자 또는 로그인 인터페이스가 확인됩니까?
- [ ] 아니요 — 관리자 패널 및 로그인 페이지가 인덱싱되지 않았습니다.  
- [ ] 예 — 관리자 패널이 인덱싱되어 있으나 강력한 다요소 인증이 필요합니다.  
- [ ] 예 — 관리자 패널이 인덱싱되어 있으며 충분한 통제 장치 없이 공개적으로 접근 가능합니다.  

### 현재는 삭제되었으나 페이지의 캐시된 버전이 민감한 데이터를 노출하고 있습니까?
- [ ] 아니요 — 캐시된 스냅샷에서 민감한 데이터가 발견되지 않았습니다.  
- [ ] 예 — 캐시된 페이지에 자격 증명(Credentials), 내부 IP 주소 또는 민감한 정보가 포함되어 있습니다.  

### `robots.txt` 파일이 의도치 않게 민감한 경로를 공개하고 있습니까?
- [ ] 아니요 — `robots.txt`가 민감한 디렉터리 구조를 드러내지 않습니다.  
- [ ] 예 — `robots.txt`가 공격자의 정찰에 도움이 되는 민감한 경로를 나열하고 있습니다.  
- [ ] `robots.txt` 파일이 존재하지 않습니다.  

### 서드파티 서비스(`Shodan`, `Censys`, `Wayback Machine`)가 과거 또는 현재의 노출 정보를 드러내고 있습니까?
- [ ] 아니요 — 서드파티 인덱싱 서비스에서 유의미한 발견 사항이 없습니다.  
- [ ] 예 — 과거 스냅샷 또는 서비스 스캔을 통해 민감한 메타데이터나 서비스 배너(Service Banner)가 확인됩니다.

---

## WSTG-INFO-02 — 웹 서버 핑거프린팅 (Fingerprint Web Server)

웹 서버 핑거프린팅 (Fingerprinting)은 대상 웹 서버의 특정 소프트웨어 유형, 버전 및 기반 운영 체제를 식별하는 프로세스입니다. 공격자는 Apache, Nginx, IIS 또는 기타 서버 기술의 특정 버전에 고유한 패치되지 않은 CVE 또는 구성 취약점과 같이 알려진 취약점을 식별하기 위해 이러한 정찰 활동을 수행합니다. 이는 일반적으로 HTTP 응답 헤더 (HTTP response headers, 예: `Server`, `X-Powered-By`)를 분석하거나, 고유한 프로토콜 동작을 조사하거나, 기본 에러 페이지 및 파일을 통해 유출된 정보를 관찰함으로써 수행됩니다. 성공적인 핑거프린팅을 통해 공격자는 일반적인 스캔 방식에서 벗어나 알려진 아키텍처 결함에 대한 높은 확률의 공격으로 익스플로잇 (Exploitation) 전략을 맞춤화할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-02 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 정보성 (Informational) / 낮음 (Low)* |

> *위험도는 핑거프린팅을 통해 알려진 치명적인 취약점이 있는 구식 또는 수명 종료 (End-of-life, EOL) 서버 버전이 발견될 경우 높음 (High)으로 격상됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/information-disclosure  

**도구:** `curl`, `Nmap`, `WhatWeb`, `Wappalyzer`, `Nikto`, `Netcat`

### HTTP 응답 헤더에 식별 문자열이 존재합니까?
- [ ] 아니요 — `Server` 및 `X-Powered-By` 헤더가 **존재하지 않거나** 일반적인 값을 포함하고 있음  
- [ ] 예 — 헤더가 서버 소프트웨어를 노출하지만 특정 버전 번호는 **포함하지 않음**  
- [ ] 예 — 헤더가 특정 서버 소프트웨어와 **정확한** 버전 번호를 노출함  

### 기본 에러 페이지 또는 시스템 생성 응답이 서버 세부 정보를 유출합니까?
- [ ] 아니요 — 사용자 정의 에러 페이지가 사용되며 서버 정보를 **노출하지 않음**  
- [ ] 예 — 기본 에러 페이지가 서버 소프트웨어 이름 및/또는 버전을 유출함  
- [ ] 예 — 에러 페이지가 서버 세부 정보, 버전 및 **기반 운영 체제**를 유출함  

### 기본 파일 또는 디렉터리 구조를 통해 서버 버전이 노출됩니까?
- [ ] 아니요 — 기본 설치 파일, 매뉴얼 및 테스트 스크립트가 **제거됨**  
- [ ] 예 — 기본 파일(예: `info.php`, `manual/`, `test.html`)이 **존재하며** 버전 데이터를 노출함  

### 고유한 동작 분석을 통해 서버 유형을 정확하게 추론할 수 있습니까?
- [ ] 아니요 — 서버 응답이 정규화되어 있으며 동작 기반 핑거프린팅이 **불가능함**  
- [ ] 예 — 고유한 동작(예: 헤더 순서, 특정 에러 코드 또는 TCP/IP 스택 특이점)을 통해 서버 식별이 **가능함**  

### 식별된 서버 버전이 현재 지원되고 패치되었습니까?
- [ ] 예 — 식별된 버전이 최신이며 벤더 (Vendor)에 의해 **지원됨**  
- [ ] 아니요 — 식별된 버전이 **구식**이거나 **수명 종료 (EOL)** 상태이며 알려진 취약점이 존재함

---

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

## WSTG-INFO-04 — 웹 서버의 애플리케이션 열거 (Enumerate Applications on Webserver)

애플리케이션 열거(Application Enumeration)는 단일 웹 서버 또는 IP 주소에 호스팅된 모든 웹 애플리케이션과 서비스를 식별하는 프로세스입니다. 현대의 웹 서버는 종종 여러 가상 호스트(Virtual Hosts), 레거시 스테이징 환경 또는 비표준 포트 및 경로에 관리자 인터페이스를 호스팅하기 때문에, 이러한 숨겨진 자산을 식별하지 못하면 공격 표면(Attack Surface)의 상당 부분이 테스트되지 않은 상태로 남게 됩니다. 공격자는 DNS 존 전송(DNS Zone Transfers), 가상 호스트 브루트 포싱(Virtual Host Brute-forcing), 역방향 IP 조회(Reverse IP Lookups)와 같은 기법을 활용하여 잊혀졌거나 가려진 진입점을 발견하며, 이러한 지점은 주로 주요 운영 애플리케이션보다 보안이 취약한 경우가 많습니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 정보성(Informational) / 낮음 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**도구:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### 대상 인프라와 연결된 여러 IP 주소 또는 DNS 별칭(DNS Aliases)이 존재합니까?
- [ ] 아니요 — 단일 IP 및 A-record만 존재함  
- [ ] 예 — 여러 IP 주소 또는 별칭이 식별되어 범위가 확장됨  

### 웹 서버가 동일한 IP에서 여러 가상 호스트(vhosts)를 호스팅합니까?
- [ ] 아니요 — 서버에서 기본 도메인만 서비스함  
- [ ] 예 — 추가 가상 호스트가 식별되었으나 접근이 불가능함 (예: 403 Forbidden)  
- [ ] 예 — 숨겨진 개발 또는 스테이징 가상 호스트가 식별되었으며 접근 가능함  

### 비표준 포트에서 애플리케이션이 호스팅되고 있습니까?
- [ ] 아니요 — 표준 포트(80/443)만 열려 있음  
- [ ] 예 — 비표준 포트가 열려 있으나 웹 애플리케이션을 호스팅하지 않음  
- [ ] 예 — 비표준 포트(예: 8080, 8443, 9000)에서 추가 웹 애플리케이션이 식별됨  

### 특정 URI 경로에 별도의 애플리케이션이 매핑되어 있습니까?
- [ ] 아니요 — 단일 애플리케이션이 전체 루트 디렉토리를 서비스함  
- [ ] 예 — 디렉토리 열거(Directory Enumeration)를 통해 여러 애플리케이션이 발견됨 (예: `/api`, `/portal`, `/backup`)  

### 제3자 정찰(Reconnaissance) 서비스를 통해 과거 기록이나 보조 애플리케이션이 확인됩니까?
- [ ] 아니요 — `Shodan`, `Censys` 또는 `Wayback Machine`에서 유의미한 결과가 없음  
- [ ] 예 — 과거 스냅샷 또는 서비스 스캔을 통해 현재 활성화되어 있지만 연결되지 않은 애플리케이션이 확인됨

---

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

## WSTG-INFO-06 — 애플리케이션 진입점 식별 (Identify Application Entry Points)

애플리케이션 진입점 식별(Identify Application Entry Points)은 사용자와 외부 시스템이 애플리케이션과 상호작용할 수 있는 모든 가능한 경로를 열거(Enumeration)하여 전체 공격 표면(Attack Surface)을 매핑하는 과정을 포함합니다. 이 과정에는 모든 URL, API 엔드포인트(API Endpoints), 파라미터(Parameters - GET, POST, 쿠키 기반), HTTP 헤더(HTTP Headers), 그리고 웹소켓(WebSockets)이나 gRPC와 같은 특화된 프로토콜의 발견이 포함됩니다. 침투 테스트 전문가(Penetration Tester)에게 이 단계는 매우 중요한데, 이는 개발 과정에서 간과된 문서화되지 않은 "섀도우 API(Shadow APIs)", 레거시 엔드포인트(Legacy Endpoints) 또는 복잡한 파라미터 구조에서 취약점(Vulnerabilities)이 자주 발견되기 때문입니다. 철저한 열거를 통해 애플리케이션의 어떤 구성 요소도 테스트되지 않은 블랙박스로 남지 않도록 보장하며, 이를 통해 공격자가 시스템으로 들어가는 모니터링되지 않은 경로를 찾는 것을 방지합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-06 |
| **CWE** | CWE-1059 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 정보 제공 (Informational) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Target/Proxy)`, `OWASP ZAP`, `ffuf`, `Arjun`, `LinkFinder`, `GoSpider`, `KiteRunner`

### 애플리케이션 전체에 걸쳐 모든 GET 및 POST 파라미터가 열거되었습니까?
- [ ] 예 — 자동화 및 수동 크롤링(Crawling)을 통한 포괄적인 열거가 완료됨  
- [ ] 예 — 표준 크롤링을 통해 일반적인 파라미터만 식별됨  
- [ ] 아니요 — 파라미터가 대부분 식별되지 않았거나 테스트되지 않음  

### 퍼징(Fuzzing)을 사용하여 문서화되지 않았거나 숨겨진 파라미터(예: debug, admin)를 검색했습니까?
- [ ] 아니요 — 이 특정 범위에서는 숨겨진 파라미터 발견이 필요하지 않음  
- [ ] 예 — 파라미터 퍼징(예: `Arjun` 사용)이 수행되었으며 발견 사항이 없음  
- [ ] 예 — 숨겨진 파라미터가 발견되었으며 추가 테스트를 위해 매핑됨  
- [ ] 아니요 — 숨겨진 파라미터 발견이 수행되지 않음  

### 각 엔드포인트에 대해 지원되는 모든 HTTP 메소드(PUT, DELETE, PATCH 등)가 식별되었습니까?
- [ ] 예 — 모든 발견된 엔드포인트에 대해 메소드 탐색이 적용됨  
- [ ] 예 — 주요 관심 대상 또는 민감한 엔드포인트에 대해서만 메소드 탐색이 적용됨  
- [ ] 아니요 — 표준 GET 및 POST 메소드만 식별됨  

### 웹소켓(WebSockets), gRPC 또는 사용자 정의 헤더와 같은 비표준 진입점이 매핑되었습니까?
- [ ] 아니요 — 애플리케이션이 비표준 프로토콜이나 사용자 정의 헤더를 사용하지 않음  
- [ ] 예 — 모든 프로토콜별 진입점 및 사용자 정의 헤더가 식별됨  
- [ ] 아니요 — 비표준 진입점이 존재하지만 매핑되지 않음  

### API 표면(버전화된 엔드포인트 /v1/ 또는 /v2/ 포함)이 완전히 매핑되었습니까?
- [ ] 아니요 — 애플리케이션이 API를 노출하지 않음  
- [ ] 예 — 모든 API 버전 및 엔드포인트가 식별되고 문서화됨  
- [ ] 예 — 현재 운영 중인 API 버전만 식별됨  
- [ ] 아니요 — API 엔드포인트가 매핑되지 않았거나 일부만 발견됨

---

## WSTG-INFO-07 — 애플리케이션 실행 경로 매핑 (Map Execution Paths Through Application)

실행 경로 매핑은 웹 애플리케이션 내에서 도달 가능한 모든 엔드포인트(Endpoint), 기능 흐름(Functional flow) 및 의사 결정 분기점을 체계적으로 식별하는 과정입니다. 이 프로세스는 레거시 코드(Legacy code), 디버그 인터페이스(Debug interface) 또는 문서화되지 않은 API 경로와 같이 현대적인 보안 제어가 부족하여 보안 검토에서 누락되기 쉬운 "숨겨진(Dark)" 기능이 방치되지 않도록 하는 데 필수적입니다. 공격자는 자동화된 스파이더링(Spidering)과 수동 탐색(Manual exploration)을 결합하여 애플리케이션의 구조를 시각화하며, 이 과정에서 권한이 없는 관리자 접근이나 비즈니스 로직 결함(Business logic flaw)과 같은 취약점을 발견하곤 합니다. 테스터는 입력을 특정 서버 측 응답과 상호 연관시킴으로써, 견고한 점검 범위를 보장하기 위해 타겟팅된 심층 테스트가 필요한 민감한 처리 로직을 정확히 식별할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-07 |
| **CWE** | CWE-200 |
| **테스트 상태** | Not Performed |
| **위험도** | Informational |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite Professional`, `OWASP ZAP`, `Katana`, `FFUF`, `LinkFinder`, `GoBuster`, `ParamMiner`

### 자동 및 수동 크롤링(Crawling)을 통해 애플리케이션이 완전히 인덱싱되었습니까?
- [ ] 예 — 가시적이며 링크된 모든 리소스에 대한 포괄적인 매핑이 **완료되었습니다**  
- [ ] 예 — 자동 크롤링은 **완료되었으나** 수동 탐색은 진행 중입니다  
- [ ] 아니요 — 애플리케이션이 인덱싱되지 **않았습니다**  

### JS 분석 또는 디렉토리 무차별 대입(Directory Brute-forcing)을 통해 숨겨지거나 문서화되지 않은 엔드포인트가 식별되었습니까?
- [ ] 아니요 — 사이드 채널 분석(Side-channel analysis)을 통해 발견된 문서화되지 않은 경로가 없습니다  
- [ ] 예 — 숨겨진 경로가 발견되었으나 권한 없이는 접근할 수 **없습니다**  
- [ ] 예 — 문서화되지 않은 엔드포인트에 접근이 **가능하며** 민감한 기능을 제공합니다 *(Critical / High)*  

### 표준이 아닌 파라미터나 헤더를 통해 실행 경로를 변경할 수 있습니까?
- [ ] 아니요 — `debug`, `test` 또는 `admin`과 같은 파라미터가 실행 흐름에 영향을 주지 **않습니다**  
- [ ] 예 — 파라미터가 출력을 변경하지만 보안 제어를 우회하지는 **않습니다**  
- [ ] 예 — 경로를 변경하는 파라미터가 **적용되어** 의도된 로직을 우회할 수 있습니다  

### 다단계 비즈니스 로직(예: 다요소 인증(MFA), 결제)의 흐름이 완전히 매핑되었습니까?
- [ ] 예 — 다단계 프로세스의 모든 상태와 전이(Transition)가 **식별되었습니다**  
- [ ] 아니요 — 복잡한 상태 머신(State machine) 전이를 완전히 매핑할 수 **없습니다**  

### API 문서 파일(Swagger/OpenAPI) 또는 클라이언트 측 맵(Source Maps)이 노출되어 있습니까?
- [ ] 아니요 — 아키텍처 맵이나 문서 파일이 공개적으로 노출되어 있지 않습니다  
- [ ] 예 — 문서가 존재하지만 인증에 의해 **보호되고 있습니다**  
- [ ] 예 — Swagger UI, OpenAPI JSON 또는 JS Source Maps가 **활성화되어 있으며** 공개적으로 접근 가능합니다

---

## WSTG-INFO-08 — 웹 애플리케이션 프레임워크 핑거프린팅 (Fingerprint Web Application Framework)

웹 애플리케이션 프레임워크 핑거프린팅(Fingerprinting)은 애플리케이션을 구축하고 서비스하는 데 사용된 특정 기술 스택과 소프트웨어 버전을 식별하는 작업을 포함합니다. 공격자는 HTTP 응답 헤더, 프레임워크별 쿠키(Cookies), 예측 가능한 파일 구조, 고유한 자바스크립트 아티팩트(JavaScript Artifacts)를 분석하여 대상이 React, Angular, Django 또는 Spring과 같은 플랫폼에서 실행 중인지 판단합니다. 이 정찰 단계(Reconnaissance Phase)는 테스터가 공격 표면(Attack Surface)을 해당 프레임워크에 특화된 알려진 취약점(CVE) 및 일반적인 설정 약점으로 좁힐 수 있게 해주므로 매우 중요합니다. 기본 아키텍처를 이해함으로써 공격자는 일반적인 테스트에서 고도로 타겟팅된 익스플로잇(Exploit) 전략으로 전환할 수 있습니다.

| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-08 |
| **CWE** | CWE-200 |
| **테스트 상태** | Not Performed |
| **위험도** | Informational |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wappalyzer`, `BuiltWith`, `WhatWeb`, `nmap`, `Burp Suite`, `curl`

### HTTP 응답 헤더가 프레임워크 이름이나 버전을 노출하고 있습니까?
- [ ] 아니요 — `X-Powered-By` 또는 `Server`와 같은 헤더가 존재하지 않거나 정제(Sanitized)되어 있습니다.
- [ ] 예 — 프레임워크 이름이 존재하지만 버전은 숨겨져 있습니다.
- [ ] 예 — 프레임워크 이름과 버전이 모두 노출되어 있습니다.

### 프레임워크 고유의 쿠키 또는 디렉터리 구조를 식별할 수 있습니까?
- [ ] 아니요 — 일반적인 프레임워크 아티팩트(예: `.js` 경로, 쿠키 이름)가 난독화되거나 이름이 변경되었습니다.
- [ ] 예 — 프레임워크 고유의 쿠키(예: `JSESSIONID`, `PHPSESSID`, `csrftoken`)를 식별할 수 있습니다.
- [ ] 예 — 디렉터리 구조(예: `/wp-content/`, `_next/static/`)가 보이며 프레임워크를 확인해 줍니다.

### 오류 메시지나 소스 코드 주석이 프레임워크 세부 정보를 공개합니까?
- [ ] 아니요 — 오류 처리가 일반적이며 주석이 제거되었습니다.
- [ ] 예 — 프레임워크 전용 스택 트레이스(Stack traces)가 활성화되어 응답에서 확인 가능합니다.
- [ ] 예 — HTML 소스 코드에 프레임워크를 식별하는 주석이나 메타데이터 태그가 포함되어 있습니다.

### 식별된 프레임워크 버전과 관련된 알려진 취약점이 있습니까?
- [ ] 아니요 — 식별된 프레임워크가 최신 버전이며 알려진 공개 취약점이 없습니다.
- [ ] 예 — 프레임워크 버전이 오래되었으며 특정 CVE를 식별할 수 있습니다.

---

## WSTG-INFO-09 — 웹 애플리케이션 지문 분석 (Fingerprint Web Application)

웹 애플리케이션 지문 분석 (Fingerprinting)은 특정 프레임워크, 콘텐츠 관리 시스템 (CMS) 또는 기반 기술 스택 (Technology stack) 및 관련 버전을 식별하는 과정입니다. 이 정찰 단계는 식별된 구성 요소를 알려진 취약점 (CVE) 및 공개된 익스플로잇 (Exploit)에 매핑하여 공격 표면 (Attack surface)을 크게 좁힐 수 있게 해주므로 매우 중요합니다. 정보는 일반적으로 HTTP 응답 헤더, 고유한 파일 경로, 쿠키 이름, HTML 소스 코드 메타데이터 및 정적 자산의 암호화 해시를 통해 수집됩니다. 기술 스택을 정확하게 파악함으로써 공격자는 일반적인 자동 스캔에서 버전별 약점을 겨냥한 고도로 정밀한 공격으로 전환할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-09 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 낮음 / 정보성 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wappalyzer`, `WhatWeb`, `BuiltWith`, `CMSmap`, `nmap`, `Nikto`, `Burp Suite`

### HTTP 응답 헤더가 기술 스택이나 버전 번호를 노출하고 있습니까?
- [ ] 아니요 — 민감한 헤더가 제거되었거나 일반적인 값을 포함하고 있습니다.
- [ ] 예 — `X-Powered-By`, `Server`, 또는 `X-AspNet-Version`과 같은 기본 헤더가 존재합니다.
- [ ] 예 — 헤더가 웹 서버 또는 애플리케이션 프레임워크의 구체적이고 오래된 버전을 노출합니다.

### 고유한 파일 경로 또는 명명 규칙을 통해 애플리케이션 프레임워크나 CMS를 식별할 수 있습니까?
- [ ] 아니요 — 파일 경로가 난독화되어 있으며 기반 기술을 드러내지 않습니다.
- [ ] 예 — 일반적인 디렉터리 구조 (예: `/wp-content/`, `/_next/`, `/node_modules/`)가 보입니다.
- [ ] 예 — `robots.txt`, `humans.txt`, 또는 `sitemap.xml`과 같은 고유한 파일에 기술별 메타데이터가 포함되어 있습니다.

### HTML 소스 코드나 정적 자산 내에 특정 버전 번호가 노출되어 있습니까?
- [ ] 아니요 — 주석이나 자산 파라미터에서 버전 정보가 발견되지 않습니다.
- [ ] 예 — HTML 주석이나 메타 태그 (예: `<meta name="generator" content="...">`)가 존재합니다.
- [ ] 예 — CSS/JS 파일명에 버전 문자열이 추가되어 있으며 (예: `jquery.js?v=1.12.4`) 이를 비활성화할 수 없습니다.

### 기본 에러 페이지나 관리 인터페이스가 소프트웨어 환경을 노출합니까?
- [ ] 아니요 — 애플리케이션이 모든 상태 코드에 대해 사용자 정의된 일반 에러 페이지를 사용합니다.
- [ ] 예 — 웹 서버나 프레임워크의 기본 에러 페이지가 활성화되어 있으며 버전 데이터를 유출합니다.
- [ ] 예 — 관리자 로그인 포털 (예: `/wp-admin`, `/phpmyadmin`)에 접근 가능하며 식별할 수 있습니다.

### 쿠키가 세션 관리 프레임워크를 노출하고 있습니까?
- [ ] 아니요 — 세션 쿠키가 일반적이거나 사용자 정의된 이름을 사용합니다.
- [ ] 예 — 기술별 쿠키 이름 (예: `PHPSESSID`, `JSESSIONID`, `ASPSESSIONID`)이 사용됩니다.

---

## WSTG-INFO-10 — 애플리케이션 아키텍처 맵핑 (Map Application Architecture)

애플리케이션 아키텍처를 맵핑하는 것은 웹 애플리케이션(Web Application)을 지원하는 기본 기술, 인프라 구성 요소 및 데이터 흐름 경로를 식별하는 것을 포함합니다. 테스터는 HTTP 응답 헤더(HTTP Response Headers), 오류 메시지, 쿠키(Cookie) 형식 및 파일 확장자를 분석하여 사용 중인 웹 서버, 애플리케이션 서버, 데이터베이스 및 서드파티 통합의 구성도를 재구성할 수 있습니다. 이러한 지식은 플랫폼별 익스플로잇(Exploit)을 선택하고 로드 밸런서(Load Balancer) 및 웹 애플리케이션 방화벽(WAF)과 같은 중개 장치의 잠재적 병목 현상이나 설정 오류를 식별할 수 있게 하므로, 이후의 공격을 맞춤화하는 데 필수적입니다. 공격자는 이러한 구조적 맵을 활용하여 일반적인 스캐닝에서 특정 소프트웨어 스택(Software Stack) 및 상호 연결된 구성 요소에 대한 표적 공격으로 전환합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INFO-10 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 정보성(Informational) / 낮음* |

> *내부 네트워크 토폴로지나 프라이빗 IP 주소가 노출되는 경우 위험도는 '중간(Medium)'으로 격상됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wappalyzer`, `BuiltWith`, `nmap`, `WhatWeb`, `Burp Suite`, `Netcat`, `curl`

### 웹 서버 및 애플리케이션 서버 기술을 정확하게 식별할 수 있습니까?
- [ ] 아니오 — 효과적인 보안 강화(Hardening) 또는 난독화(Obfuscation)로 인해 식별이 불가능함  
- [ ] 예 — 기술 유형은 알려져 있으나 특정 버전은 확인할 수 없음  
- [ ] 예 — 헤더 또는 파일 시그니처를 통해 기술 및 특정 버전이 완전히 노출됨 *(정보성)*  

### 통신 경로에서 중개 장치(WAF, 로드 밸런서, 프록시)를 탐지할 수 있습니까?
- [ ] 아니오 — 중개 장치가 탐지되지 않거나 완전히 투명함  
- [ ] 예 — 타이밍이나 동작을 통해 존재가 의심되지만, 정체는 확인되지 않음  
- [ ] 예 — 고유한 헤더나 동작을 통해 특정 제품(예: Cloudflare, Nginx, F5)이 식별됨  

### 애플리케이션이 내부 디렉토리 구조나 네트워크 토폴로지에 대한 정보를 유출합니까?
- [ ] 아니오 — 내부 경로 및 IP가 노출되지 않음  
- [ ] 예 — 오류 메시지나 스택 트레이스(Stack traces)에서 내부 경로가 유출되지만, 내부 IP는 유출되지 않음  
- [ ] 예 — 내부 디렉토리 구조와 프라이빗 IP 주소가 모두 노출됨  

### 애플리케이션 동작을 통해 백엔드(Backend) 데이터베이스 유형 및 버전을 추론할 수 있습니까?
- [ ] 아니오 — 데이터베이스 세부 정보를 확인할 수 없음  
- [ ] 예 — 오류 메시지나 동작을 통해 데이터베이스 유형(예: PostgreSQL, MSSQL)을 추론함  
- [ ] 예 — 응답 본문이나 헤더에 데이터베이스 유형 및 버전이 명시적으로 노출됨  

### 애플리케이션이 식별 가능한 클라우드 서비스 제공업체나 특정 서드파티 CDN에 호스팅되어 있습니까?
- [ ] 아니오 — 호스팅 인프라를 식별할 수 없음  
- [ ] 예 — 클라우드 제공업체(AWS, Azure, GCP)는 식별되지만 특정 서비스는 식별되지 않음  
- [ ] 예 — 특정 클라우드 서비스(예: S3 버킷, Lambda, Azure Functions)가 맵핑되고 접근 가능함

---

## WSTG-CONF-01 — 네트워크 인프라 설정 점검 (Network Infrastructure Configuration Testing)

네트워크 인프라 설정 점검(Network Infrastructure Configuration Testing)은 웹 애플리케이션을 지원하는 기본 서버 및 네트워크 구성 요소의 취약점을 식별하는 과정입니다. 공격자는 잘못 설정된 서비스, 구형 프로토콜 및 불필요하게 열린 포트를 표적으로 삼아 발판을 마련하거나, 측면 이동(Lateral Movement)을 수행하거나, 정찰(Reconnaissance)을 진행합니다. 주요 발견 사항으로는 취약한 TLS(Transport Layer Security) 버전, 상세한 서비스 배너(Service Banner), 내부 네트워크로 제한되어야 할 노출된 관리 인터페이스 등이 있습니다. 공격자는 이러한 인프라 계층의 결함을 악용하여 전송 중인 데이터의 기밀성을 침해하거나 호스트 운영체제(OS)에 대한 비인가 접근 권한을 획득할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-01 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 낮음 / 중간 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/pentesting-methodology.html  

**도구:** `nmap`, `sslyze`, `testssl.sh`, `Nikto`, `OpenVAS`, `Nessus`

### 애플리케이션 호스트에 불필요하게 열린 포트나 노출된 서비스가 있습니까?
- [ ] 아니요 — 필수 포트(예: 443)만 **접근 가능함**  
- [ ] 예 — 추가 서비스가 노출되어 있으나 **보안** 설정을 따름  
- [ ] 예 — 비필수 서비스(예: FTP, Telnet, SMB)가 공용 네트워크에 **노출됨**  

### 서비스 배너 또는 헤더에서 민감한 버전 정보가 유출됩니까?
- [ ] 아니요 — 배너가 제거되었거나 일반적인 내용임  
- [ ] 예 — 배너가 서버 소프트웨어(예: Apache/nginx)를 노출하지만 버전 번호는 **포함하지 않음**  
- [ ] 예 — 상세한 배너가 특정 소프트웨어 버전을 노출하여 **익스플로잇(Exploit)**을 용이하게 함  

### TLS 설정이 최신 보안 권장 사항(Best Practices)을 따르고 있습니까?
- [ ] 예 — 강력한 암호화 제품군(Cipher Suite)과 함께 TLS 1.2/1.3만 **활성화됨**  
- [ ] 예 — 구형 프로토콜(TLS 1.0/1.1)이 **활성화되어 있음**  
- [ ] 아니요 — 취약한 암호 또는 보안되지 않은 프로토콜(SSLv2/v3)이 **활성화됨**  

### 관리자 또는 매니지먼트 인터페이스가 허가된 네트워크로 제한되어 있습니까?
- [ ] 예 — 인터페이스(예: SSH, RDP, 제어판)가 공용 인터넷에서 **접근 불가능함**  
- [ ] 아니요 — 인터페이스가 외부에서 **접근 가능**하지만 다중 요소 인증(MFA)이 요구됨  
- [ ] 아니요 — 인터페이스가 단일 요소 인증 또는 기본 자격 증명(Default Credentials)을 사용하여 외부에서 **접근 가능함**

---

## WSTG-CONF-02 — 애플리케이션 플랫폼 설정 점검 (Application Platform Configuration Testing)

애플리케이션 플랫폼 설정 점검은 하위 웹 서버, 애플리케이션 서버 및 프레임워크(Framework) 설정을 감사하여 일반적인 익스플로잇(Exploit)에 대해 보안이 강화(Hardened)되었는지 확인하는 과정을 포함합니다. 공격자는 무단 접근 권한을 획득하거나 코드를 실행하기 위해 기본 계정 정보(Default Credentials), 패치되지 않은 플랫폼 취약점, 노출된 관리자 인터페이스(Administrative Interfaces)를 적극적으로 탐색합니다. 설정 오류(Misconfigurations)는 종종 민감한 환경 변수(Environment Variables), 내부 시스템 경로(Internal System Paths) 노출 또는 공격 표면(Attack Surface)을 확장하는 불필요한 샘플 애플리케이션의 존재로 이어집니다. 전문적인 관점에서 볼 때, 잘못 설정된 플랫폼은 대상 환경에 대한 초기 접근(Initial Access)을 달성할 가능성이 높은 진입점 역할을 합니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-02 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *관리자 인터페이스가 기본 계정 정보로 접근 가능하거나 샘플 스크립트로 인해 원격 코드 실행(Remote Code Execution, RCE)이 가능한 경우 위험도는 '높음(High)'으로 분류됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Nmap`, `Nikto`, `WhatWeb`, `Wappalyzer`, `Burp Suite`, `Curl`

### 플랫폼에 기본 계정 정보 또는 계정이 활성화되어 있습니까?
- [ ] 아니오 — 모든 기본 계정이 **비활성화**되었거나 비밀번호가 변경됨  
- [ ] 예 — 기본 계정이 존재하지만 외부 네트워크에서는 **접근할 수 없음**  
- [ ] 예 — 기본 계정이 **활성화**되어 있으며 인터넷을 통해 접근 가능함 *(위험)*  

### 플랫폼이 헤더 또는 에러 페이지를 통해 민감한 버전 정보를 노출하고 있습니까?
- [ ] 아니오 — 버전 문자열이 **숨겨져** 있거나 일반적인 내용임  
- [ ] 예 — 상세한 버전 정보가 HTTP 헤더(예: `Server`, `X-Powered-By`)에 **노출됨**  
- [ ] 예 — 상세 에러 메시지에 전체 스택 트레이스(Stack Traces) 및 내부 시스템 경로가 **노출됨**  

### 관리 또는 경영 인터페이스가 적절히 제한되어 있습니까?
- [ ] 아니오 — 관리자 인터페이스가 **노출되지 않음**  
- [ ] 예 — 인터페이스가 존재하지만 IP 허용 목록(IP Allowlisting) 또는 다중 요소 인증(Multi-Factor Authentication, MFA)에 의해 **제한됨**  
- [ ] 예 — 인터페이스(예: `/admin`, `/manager`, `/console`)가 단일 요소 인증만으로 **공개적으로 접근 가능함**  

### 운영 서버에 샘플 파일, 스크립트 또는 문서가 존재합니까?
- [ ] 아니오 — 불필요한 파일이 모두 **제거됨**  
- [ ] 예 — 샘플 파일 또는 문서가 **존재**하지만 민감한 정보를 유출하지 않음  
- [ ] 예 — 샘플 스크립트(예: `info.php`, `examples/`)가 **존재**하며 직접적인 공격 벡터를 제공함  

### 서버가 취약한 HTTP 메소드(HTTP Methods)를 지원하도록 설정되어 있습니까?
- [ ] 아니오 — `GET` 및 `POST`만 **활성화됨**  
- [ ] 예 — `PUT`, `DELETE` 또는 `TRACE`와 같이 잠재적으로 위험한 메소드가 **활성화**되어 있으나 제한됨  
- [ ] 예 — 위험한 메소드가 **활성화**되어 있으며 무단 파일 수정 또는 계정 정보 탈취가 가능함

---

## WSTG-CONF-03 — 민감한 정보에 대한 파일 확장자 처리 (File Extension Handling) 테스트

파일 확장자 처리 테스트는 웹 서버나 애플리케이션 서버가 제한되거나 실행되어야 할 파일을 제공함으로써 민감한 정보를 노출하는지 여부를 식별하는 과정을 포함합니다. 공격자들은 특정 핸들러 매핑(Handler Mappings)의 부재로 인해 웹 루트(Web Root)에 남겨져 평문(Plain Text)으로 제공될 수 있는 백업 파일, 설정 파일, 소스 코드 조각(예: `.bak`, `.old`, `.env`, `.inc`)을 빈번하게 탐색합니다. 이러한 취약점은 데이터베이스 자격 증명(Credentials), API 키(API Keys), 내부 비즈니스 로직의 노출로 이어져 인프라에 대한 심층적인 공격을 용이하게 하므로 중요합니다. 이러한 노출은 주로 공용 디렉터리, 업로드 폴더 또는 서버의 잘못 설정된 MIME 타입(MIME-type) 설정을 통해 발생합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-03 |
| **CWE** | CWE-552 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *자격 증명을 포함한 설정 파일이나 전체 소스 코드에 접근 가능한 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `ffuf`, `gobuster`, `dirsearch`, `Burp Suite (Intruder)`, `Wfuzz`

### 민감한 백업 또는 임시 파일 확장자(예: `.bak`, `.old`, `.swp`, `~`)에 접근 가능합니까?
- [ ] 아니오 — 서버가 일반적인 백업 확장자에 대해 403 Forbidden 또는 404 Not Found를 반환함  
- [ ] 예 — 서버가 백업 파일 다운로드를 허용하지만, 민감한 데이터를 포함하고 있지 않음  
- [ ] 예 — 서버가 민감한 데이터나 소스 코드가 포함된 백업 파일의 다운로드를 허용함 *(높음)*  

### 서버가 환경 설정 또는 시스템 구성 파일(예: `.env`, `.config`, `.yml`, `.ini`)을 노출합니까?
- [ ] 아니오 — 제한된 설정 파일에 접근할 수 없으며 403/404를 반환함  
- [ ] 예 — 민감한 설정 파일에 접근 가능하며 내부 비밀 정보나 자격 증명이 노출됨  

### 대체 확장자(예: `.php.txt`, `.jsp.old`, `.aspx.bak`)를 추가하여 소스 코드를 추출할 수 있습니까?
- [ ] 아니오 — 확장자 조작과 관계없이 애플리케이션 코드가 평문으로 렌더링되지 않음  
- [ ] 예 — 서버에 추가된 확장자에 대한 핸들러가 없어 소스 코드가 노출됨  

### 관리용 또는 메타데이터 디렉터리(예: `.git/`, `.svn/`, `.DS_Store`)가 외부 접근으로부터 차단되어 있습니까?
- [ ] 아니오 — 해당 디렉터리/파일이 웹 루트에 존재하지 않음  
- [ ] 예 — 서버 측 리라이트 규칙(Rewrite Rules)이나 권한 설정으로 인해 접근이 불가능함  
- [ ] 예 — 메타데이터 디렉터리에 접근 가능하며 이를 통해 전체 소스 코드 복원이 가능함  

### 서버가 다중 확장자(예: `file.php.jpg`)를 가진 파일에 대한 요청을 어떻게 처리합니까?
- [ ] 아니오 — 서버가 내부 확장자의 보안을 올바르게 우선시하거나 요청을 차단함  
- [ ] 예 — 서버가 파일을 첫 번째 확장자로 처리하지만, 실행을 위한 우회(Bypass)는 불가능함  
- [ ] 예 — 서버가 최종 확장자를 무시하여 코드 실행이나 소스 노출이 가능함

---

## WSTG-CONF-04 — 민감한 정보 유출을 방지하기 위한 오래된 백업 및 참조되지 않은 파일 검토 (Review Old Backup and Unreferenced Files for Sensitive Information)

오래된 백업 및 참조되지 않은 파일을 검토하는 작업은 웹 서버에서 공개 접근을 목적으로 하지 않는 방치된 파일, 임시 파일 또는 숨겨진 파일을 식별하는 과정을 포함합니다. 이러한 파일에는 소스 코드 백업(`.zip`, `.bak`), 텍스트 에디터 스왑 파일(`.swp`, `~`), 또는 소스 제어 메타데이터(`.git`, `.svn`)가 포함되는 경우가 많으며, 이는 민감한 정보를 유출할 수 있습니다. 공격자는 자동화된 워드리스트와 발견 도구를 사용하여 일반적인 명명 규칙에 대해 무차별 대입(Brute Force) 공격을 수행하며, 데이터베이스 인증 정보(Credentials), 하드코딩된 API 키 또는 추가적인 익스플로잇(Exploit)을 용이하게 하는 로직을 탈취하는 것을 목표로 합니다. 이러한 취약점은 주로 수동 서버 유지 관리나 운영 환경의 웹 루트(Web Root)를 정리하지 못한 결함이 있는 CI/CD 파이프라인(CI/CD Pipelines)에서 발생합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-04 |
| **CWE** | CWE-530 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 / 높음* |

> *설정 파일, 소스 코드 아카이브 또는 인증 정보가 발견될 경우 위험도는 '높음(High)'으로 분류됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `ffuf`, `dirsearch`, `gobuster`, `GitTools`, `Burp Suite (Engagement Tools)`, `Wfuzz`

### 웹 서버에서 디렉토리 리스팅(Directory Listing)이 활성화되어 있습니까?
- [ ] 아니오 — 디렉토리 리스팅이 **비활성화**되어 있으며 403 Forbidden 또는 커스텀 에러를 반환함  
- [ ] 예 — 민감하지 않은 디렉토리에서 디렉토리 리스팅이 **활성화**되어 있음  
- [ ] 예 — 소스 코드나 민감한 파일이 포함된 디렉토리에서 디렉토리 리스팅이 **활성화**되어 있음 *(치명적)*  

### 일반적인 확장자(예: .bak, .old, .save)를 가진 백업 파일을 찾을 수 있습니까?
- [ ] 아니오 — 일반적인 백업 확장자가 **발견되지 않거나** 서버 설정에 의해 차단됨  
- [ ] 예 — 백업 파일이 존재하지만 민감한 정보를 **포함하지 않음**  
- [ ] 예 — 설정 파일 또는 소스 코드에 대한 백업 파일에 **접근 가능함** *(높음)*  

### 소스 제어 디렉토리(예: .git, .svn) 또는 메타데이터가 노출되어 있습니까?
- [ ] 아니오 — 소스 제어 메타데이터가 **존재하지 않거나** 올바르게 차단됨  
- [ ] 예 — 메타데이터가 존재하지만 전체 저장소를 재구성할 수는 **없음**  
- [ ] 예 — 노출된 메타데이터를 통해 전체 소스 코드 저장소를 **탈취할 수 있음**  

### 애플리케이션의 압축 아카이브(예: .zip, .tar.gz, .rar)가 웹 루트에 존재합니까?
- [ ] 아니오 — 무차별 대입 또는 열거(Enumeration)를 통해 발견된 민감한 아카이브 파일이 없음  
- [ ] 예 — 아카이브가 발견되었으나 비밀번호로 보호되어 있거나 공개 자산만 포함하고 있음  
- [ ] 예 — 애플리케이션 소스나 데이터를 포함하는 암호화되지 않은 아카이브에 **공개적으로 접근 가능함**  

### 애플리케이션에서 텍스트 에디터나 IDE에 의해 생성된 임시 파일이 유출되고 있습니까?
- [ ] 아니오 — `.swp`, `~`, 또는 `.DS_Store`와 같은 임시 파일이 **존재하지 않음**  
- [ ] 예 — 민감하지 않은 임시 파일이 **존재함**  
- [ ] 예 — 임시 파일을 통해 소스 코드 조각이나 내부 경로가 노출됨

---

## WSTG-CONF-05 — 인프라 및 애플리케이션 관리 인터페이스 열거 (Enumerate Infrastructure and Application Admin Interfaces)

관리 인터페이스는 애플리케이션 및 기본 인프라의 관리 제어 평면(Management Control Plane)을 나타내며, 종종 시스템 구성, 사용자 데이터 및 서버 측 작업에 대한 권한 있는 액세스(Privileged Access)를 부여합니다. 공격자는 디렉토리 브루트 포싱(Directory Brute-forcing), `robots.txt` 분석 또는 예측 가능한 URL 패턴 관찰을 통해 이러한 인터페이스를 열거하는 것을 우선순위로 두어, 강력한 인증이 부족하거나 기본 자격 증명(Default Credentials)에 의존할 수 있는 진입점을 찾습니다. 노출된 관리자 패널의 발견은 전체 시스템 침해(System Compromise), 무단 데이터 수정 또는 서비스 중단의 위험을 크게 증가시킵니다. 이러한 인터페이스는 비표준 포트나 서브도메인에서 빈번하게 발견되어 자동화된 스캐너와 수동 정찰(Reconnaissance)의 주요 목표가 됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간(Medium) / 높음(High)* |

> *인터페이스가 다요소 인증(MFA) 없이 시스템 전반의 구성 변경, 사용자 관리 또는 데이터 유출을 허용하는 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### 디렉토리 퍼징(Directory Fuzzing) 또는 정찰을 통해 관리 인터페이스를 발견할 수 있습니까?
- [ ] 아니요 — 노출되었거나 발견 가능한 관리 인터페이스가 없습니다.
- [ ] 예 — 인터페이스를 발견할 수 있으나 액세스가 내부 IP 대역으로 제한됩니다.
- [ ] 예 — 인터페이스를 발견할 수 있으며 공개적으로 액세스 가능합니다.

### 발견된 관리 포털에 인증이 강제 적용됩니까?
- [ ] 예 — 다요소 인증(MFA)이 강제 적용됩니다.
- [ ] 예 — 단일 요소 인증(Single-factor Authentication)이 강제 적용됩니다.
- [ ] 아니요 — 인터페이스 액세스에 인증이 필요하지 않습니다 *(위험: Critical)*.

### 발견을 방지하기 위해 관리 경로가 숨겨져 있거나 비표준적입니까?
- [ ] 예 — 경로가 비표준적이고, 무작위화되었거나 예측 불가능한 명명 규칙을 사용합니다.
- [ ] 아니요 — 경로가 일반적인 명명 규칙(예: `/admin`, `/manager`, `/console`)을 사용하며 쉽게 추측할 수 있습니다.

### 인터페이스가 민감한 시스템 또는 애플리케이션 기능에 대한 액세스를 제공합니까?
- [ ] 아니요 — 인터페이스가 영향이 미미한 "읽기 전용" 상태 대시보드입니다.
- [ ] 예 — 인터페이스가 애플리케이션 구성 또는 사용자 데이터를 수정할 수 있습니다.
- [ ] 예 — 인터페이스가 시스템 수준의 명령을 실행하거나 데이터베이스 관리(Database Management)를 수행할 수 있습니다.

---

## WSTG-CONF-06 — HTTP 메소드 테스트 (Test HTTP Methods)

HTTP 메소드 테스트는 웹 서버 및 애플리케이션에서 지원하는 동사(Verbs)를 식별하여 필요한 기능만 노출되도록 보장하는 과정을 포함합니다. 표준 `GET` 및 `POST` 외에도 서버는 `PUT`, `DELETE` 또는 `TRACE`와 같은 위험한 메소드를 의도치 않게 활성화할 수 있으며, 이는 공격자가 악성 파일을 업로드하거나 기존 콘텐츠를 삭제하고, 또는 교차 사이트 트레이싱(Cross-Site Tracing, XST)을 통해 세션 쿠키를 탈취할 수 있게 합니다. 모의해킹 전문가(Pentester)는 `Allow` 또는 `Public`과 같은 응답 헤더를 조사하고 메소드 오버라이딩(Method Overriding) 기법이나 동사 변조(Verb Tampering)를 사용하여 제한을 우회하고 비인가 기능에 접근을 시도합니다. 이 테스트는 공격 표면(Attack Surface)을 강화하고 서버 침해 또는 무단 데이터 조작으로 이어질 수 있는 설정 오류를 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간* |

> *`PUT` 또는 `DELETE`가 비인가 파일 조작을 허용하거나 `TRACE`가 세션 토큰 탈취를 용이하게 하는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### 애플리케이션 전반에서 `PUT` 또는 `DELETE`와 같은 위험한 HTTP 메소드가 비활성화되어 있습니까?
- [ ] 예 — 안전한 메소드(GET, POST, HEAD)만 **활성화되어 있음**  
- [ ] 아니요 — `PUT` 또는 `DELETE`가 **활성화되어 있으나** 유효한 인증이 필요함  
- [ ] 아니요 — `PUT` 또는 `DELETE`가 **활성화되어 있으며** 인증 없이 접근 가능함 *(치명적)*  

### 교차 사이트 트레이싱(XST)을 방지하기 위해 `TRACE` 메소드가 비활성화되어 있습니까?
- [ ] 예 — `TRACE` 메소드가 **비활성화되었거나** 405 Method Not Allowed를 반환함  
- [ ] 아니요 — `TRACE` 메소드가 **활성화되어 있으나** 민감한 헤더를 반영하지 않음  
- [ ] 아니요 — `TRACE` 메소드가 **활성화되어 있으며** `Cookie` 또는 `Authorization` 헤더를 반영함 *(중간)*  

### 서버에서 HTTP 메소드 오버라이딩(Method Overriding, 예: `X-HTTP-Method-Override`)을 지원합니까?
- [ ] 아니요 — 서버가 메소드 오버라이드 헤더를 **처리하지 않음**  
- [ ] 예 — 서버가 오버라이드 헤더를 처리하지만 접근 제어가 **적용됨**  
- [ ] 예 — 서버가 오버라이드 헤더를 처리하며 접근 제어 우회가 **가능함**  

### 임의의 또는 잘못된 형식의 HTTP 메소드에 대해 서버가 보안상 안전하게 응답합니까?
- [ ] 예 — 서버가 405 Method Not Allowed 또는 501 Not Implemented를 반환함  
- [ ] 아니요 — `JEFF` 또는 `TEST`와 같은 임의의 메소드에 대해 서버가 200 OK 또는 500 Error를 반환함  
- [ ] 아니요 — 임의의 메소드를 사용하여 인증 또는 인가 필터 우회가 가능함  

### 정보 노출을 최소화하도록 `OPTIONS` 메소드가 제한되거나 설정되어 있습니까?
- [ ] 예 — `OPTIONS`가 비활성화되어 있거나 최소한의 `Allow` 헤더만 반환함  
- [ ] 아니요 — `OPTIONS`가 DAV 또는 디버깅 동사를 포함하여 활성화된 광범위한 메소드를 노출함

---

## WSTG-CONF-07 — HTTP 엄격 전송 보안(HTTP Strict Transport Security, HSTS) 테스트

HTTP 엄격 전송 보안(HSTS)은 웹 서버가 브라우저에게 오직 HTTPS를 통해서만 접속해야 함을 알리는 보안 메커니즘으로, 프로토콜 다운그레이드 공격(Protocol Downgrade Attack) 및 쿠키 하이재킹(Cookie Hijacking)을 방지합니다. HSTS는 암호화된 연결을 강제함으로써, 공격자가 초기 비보안 HTTP 요청을 가로채 TLS로의 전환을 방해하는 SSLStrip과 같은 중간자 공격(Man-in-the-Middle, MitM)을 완화합니다. 공격자 관점에서 이 헤더가 없거나 `max-age` 설정 기간이 짧을 경우, 초기 평문(Cleartext) 핸드셰이크(Handshake) 과정에서 민감한 세션 토큰이나 자격 증명을 가로챌 수 있는 기회가 제공됩니다. 이 테스트는 모든 민감한 애플리케이션 엔드포인트에서 `Strict-Transport-Security` 헤더의 존재 여부, 지속 시간 및 범위를 검증하는 데 중점을 둡니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간* |

> *애플리케이션이 개인정보(PII), 세션 토큰 또는 자격 증명을 처리하는 경우, HSTS의 부재가 MitM 공격의 성공률을 높이므로 위험도는 '중간'으로 격상됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### HTTPS 응답에 `Strict-Transport-Security` 헤더가 포함되어 있습니까?
- [ ] 예 — 모든 민감한 엔드포인트에 헤더가 **존재함**  
- [ ] 예 — 헤더가 **존재하나** 랜딩 페이지에만 설정됨  
- [ ] 아니요 — 애플리케이션 전체에서 헤더가 **누락됨**  

### `max-age` 지시문이 충분한 기간으로 설정되어 있습니까?
- [ ] 예 — `max-age`가 1년 이상(예: `31536000`)으로 설정됨  
- [ ] 예 — `max-age`가 설정되어 있으나 기간이 **너무 짧음** (예: 6개월 미만)  
- [ ] 아니요 — `max-age` 지시문이 **누락되었거나** `0`으로 설정됨 *(심각)*  

### HSTS 정책이 모든 서브도메인에 적용됩니까?
- [ ] 예 — `includeSubDomains` 지시문이 **적용됨**  
- [ ] 아니요 — `includeSubDomains` 지시문이 **누락되어** 서브도메인이 다운그레이드 공격에 취약함  
- [ ] 아니요 — 서브도메인이 존재하지 않아 해당 사항 없음  

### 애플리케이션이 HSTS 프리로딩(Preloading)에 등록되어 있습니까?
- [ ] 예 — `preload` 지시문이 **존재하며** 도메인이 HSTS 프리로드 목록에 포함되어 있음  
- [ ] 예 — `preload` 지시문은 **존재하나** 도메인이 아직 프리로드 목록에 포함되지 않음  
- [ ] 아니요 — `preload` 지시문이 **누락되어** 첫 방문 시 MitM 공격의 위험이 있음  

### HSTS 헤더가 평문 HTTP를 통해 잘못 전송되고 있습니까?
- [ ] 아니요 — 헤더가 보안 HTTPS 연결을 통해서만 **전송됨**  
- [ ] 예 — 헤더가 HTTP를 통해 **전송됨** (브라우저에 의해 무시되며 설정 세부 정보가 노출될 수 있음)

---

## WSTG-CONF-08 — RIA 교차 도메인 정책 테스트 (RIA Cross Domain Policy Testing)

리치 인터넷 애플리케이션(Rich Internet Application, RIA) 교차 도메인 정책은 Adobe Flash 및 Microsoft Silverlight와 같은 웹 클라이언트가 교차 출처 요청(Cross-origin requests)을 처리하는 방식을 정의하는 `crossdomain.xml` 및 `clientaccesspolicy.xml`과 같은 파일을 포함합니다. 이 파일들은 동일 출처 정책(Same-Origin Policy, SOP)을 명시적으로 재정의하여 외부 도메인이 애플리케이션과 상호 작용하고 응답을 읽을 수 있도록 허용하는 범위를 결정하므로 보안상 매우 중요합니다. `domain` 속성에 와일드카드(Wildcard)를 사용하는 것과 같이 과도하게 허용적인 설정은 공격자가 제3자 사이트에 악성 RIA 객체를 호스팅하여 민감한 데이터를 탈취(Exfiltrate)하거나 인증된 사용자를 대신해 작업을 수행할 수 있게 합니다. 모의 해킹 전문가(Pentester)는 일반적으로 웹 루트(Web root)에서 이러한 파일을 찾아 제3자 출처에 부여된 신뢰 수준을 평가하고 잠재적인 교차 출처 데이터 유출을 식별합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *애플리케이션이 허용적인 정책을 통해 유출될 수 있는 민감한 사용자 데이터나 세션 식별자를 처리하는 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### 웹 루트에 교차 도메인 정책 파일(`crossdomain.xml` 또는 `clientaccesspolicy.xml`)이 존재합니까?
- [ ] 아니요 — 루트 디렉토리에서 파일을 찾을 수 없음  
- [ ] 예 — `crossdomain.xml` (Flash)이 존재함  
- [ ] 예 — `clientaccesspolicy.xml` (Silverlight)이 존재함  
- [ ] 예 — 두 RIA 정책 파일이 모두 존재함  

### `allow-access-from` 도메인 속성이 신뢰할 수 있는 출처로 제한되어 있습니까?
- [ ] 아니요 — 파일은 존재하지만 `allow-access-from` 항목이 없음  
- [ ] 예 — 특정 신뢰할 수 있는 도메인이 명시적으로 화이트리스트(Whitelist)에 등록되어 있으며 우회할 수 없음  
- [ ] 예 — 도메인이 화이트리스트에 등록되어 있으나 신뢰할 수 없는 제3자 또는 하위 도메인이 포함됨  
- [ ] 예 — 와일드카드 `*`가 사용되어 **모든** 출처에서의 접근을 허용함 *(위험)*  

### 정책이 HTTPS 리소스에 대해 HTTP를 통한 안전하지 않은 통신을 허용합니까?
- [ ] 아니요 — `secure="true"`로 설정되어(또는 기본값) HTTP 출처가 HTTPS 데이터에 접근하는 것을 방지함  
- [ ] 예 — `secure="false"`가 명시적으로 설정되어 중간자 공격(MITM)자가 보안 데이터를 탈취할 수 있음  

### 교차 도메인 설정에서 HTTP 헤더가 과도하게 허용되어 있습니까?
- [ ] 아니요 — `allow-http-request-headers-from`이 제한되어 있거나 활성화되지 않음  
- [ ] 예 — 신뢰할 수 있는 도메인에 대해 특정 헤더가 허용됨  
- [ ] 예 — 헤더에 와일드카드 `*`가 사용되어 공격자가 모든 도메인에서 커스텀 헤더(Custom headers)를 보낼 수 있음  

### `site-control` (마스터 정책) 설정이 제한적입니까?
- [ ] 아니요 — `permitted-cross-domain-policies`가 `none` 또는 `master-only`로 설정됨  
- [ ] 예 — 정책이 서버의 다른 파일이 자체 규칙을 정의하도록 허용함 (`all`)

---

## WSTG-CONF-09 — 파일 권한 테스트 (Test File Permission)

파일 권한(File Permission) 테스트는 웹 서버의 파일 및 디렉토리에 할당된 접근 제어(Access Control) 수준을 감사하여, 민감한 리소스가 권한이 없는 사용자나 프로세스에 과도하게 노출되지 않았는지 확인하는 과정입니다. 부적절하게 구성된 권한은 공격자가 데이터베이스 자격 증명이 포함된 민감한 설정 파일을 읽거나, 독점적인 소스 코드(Source Code)를 열람하거나, 서버 측 스크립트를 수정하여 원격 코드 실행(Remote Code Execution, RCE)을 달성하게 할 수 있습니다. 이러한 취약점은 주로 배포 단계에서 설정 경로, 로그 폴더 또는 업로드 저장소와 같은 중요한 애플리케이션 디렉토리에 '누구나 읽기 가능(World-readable)' 또는 '누구나 쓰기 가능(World-writable)' 플래그가 기본값으로 남겨질 때 발생합니다. 공격자의 관점에서 부적절하게 보안 설정된 파일을 발견하는 것은 종종 권한 상승(Privilege Escalation) 또는 전체 시스템 침해(Full System Compromise)의 일차적인 촉매제가 됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-09 |
| **CWE** | CWE-732 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 중간(Medium) / 높음(High)* |

> *민감한 설정 파일(예: `.env`, `web.config`)을 읽을 수 있거나 웹 루트(Web Root)에 대한 쓰기 권한이 가능한 경우 심각도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission  
* https://hacktricks.wiki/en/linux-hardening/privilege-escalation/index.html  

**도구:** `ls`, `find`, `LinPeas`, `WinPeas`, `ffuf`, `dirsearch`, `Nmap`

### 민감한 애플리케이션 설정 파일이 권한이 없는 접근으로부터 보호되고 있습니까?
- [ ] 예 — 설정 파일(예: `.env`, `config.php`)에 웹 요청을 통해 **접근할 수 없음**  
- [ ] 예 — 파일이 존재하지만 권한이 있는 로컬 사용자로만 **접근이 제한됨**  
- [ ] 아니요 — 민감한 설정 파일에 **접근 가능**하며 자격 증명 또는 비밀 정보가 노출됨  

### 공격자가 웹 루트 내의 파일이나 디렉토리를 수정할 수 있습니까?
- [ ] 아니요 — 모든 웹 루트 디렉토리가 웹 서버 사용자에 대해 **읽기 전용**임  
- [ ] 예 — 쓰기 가능한 디렉토리가 존재하지만 스크립트 실행은 **비활성화됨**  
- [ ] 예 — 누구나 쓰기 가능한 디렉토리가 존재하며 스크립트 업로드 및 실행을 **허용함** *(위험)*  

### 느슨한 권한 설정으로 인해 버전 관리 메타데이터나 백업 파일이 노출되어 있습니까?
- [ ] 아니요 — `.git`, `.svn` 및 백업 파일(예: `.bak`, `~`)이 **존재하지 않거나** 보호됨  
- [ ] 예 — 메타데이터 디렉토리가 존재하지만 디렉토리 리스팅(Directory Listing)은 **비활성화됨**  
- [ ] 예 — 민감한 메타데이터 또는 백업에 **완전히 접근 가능**하여 소스 코드 복구가 가능함  

### 웹 서버 사용자가 최소 권한 원칙(Principle of Least Privilege)에 따라 운영되고 있습니까?
- [ ] 예 — 웹 서버 프로세스가 **전용 저권한** 사용자로 실행됨  
- [ ] 아니요 — 웹 서버 프로세스가 **불필요한 권한**(예: `root` 또는 `Administrator`)으로 실행됨  

### 로그 파일이 권한 없는 읽기 또는 수정으로부터 안전하게 보호되고 있습니까?
- [ ] 예 — 로그 파일 접근이 관리자 사용자로 **제한됨**  
- [ ] 예 — 로그 파일을 읽을 수 있으나 세션 토큰(Session Tokens)이나 개인정보(PII)를 포함하고 있지 **않음**  
- [ ] 아니요 — 로그 파일을 **공개적으로 읽을 수 있으며** 민감한 트랜잭션 데이터나 사용자 정보가 노출됨

---

## WSTG-CONF-10 — 서브도메인 탈취 (Subdomain Takeover)

서브도메인 탈취(Subdomain Takeover)는 DNS 레코드(주로 CNAME이지만, 때로는 A 또는 MX 레코드)가 제3자 클라우드 서비스 제공업체(Third-party cloud provider)나 호스팅 서비스에서 더 이상 사용되지 않거나 존재하지 않는 리소스를 가리킬 때 발생합니다. 공격자는 AWS, GitHub, Azure와 같은 제공업체로부터 리소스가 더 이상 점유되지 않았음을 나타내는 특정 오류 메시지를 찾아 이러한 "연결되지 않은(Dangling)" DNS 레코드를 식별합니다. 공격자는 해당 제공업체의 플랫폼에서 방치된 리소스 이름을 등록함으로써 서브도메인에 대한 전체 제어권을 획득하며, 이를 통해 악성 콘텐츠를 호스팅하거나 기본 도메인(Base domain)으로 범위가 지정된 세션 쿠키(Session cookies)를 탈취하고, 콘텐츠 보안 정책(CSP) 또는 교차 출처 리소스 공유(CORS) 보호를 우회할 수 있습니다. 이 취약점은 조직의 정당한 도메인 구조와 관련된 신뢰를 활용하여 파급력이 큰 피싱(Phishing) 및 데이터 탈취를 용이하게 하기 때문에 특히 위험합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 / 치명적* |

> *서브도메인이 기본 도메인에서 세션 쿠키를 탈취하거나 SSO/OAuth 리다이렉트를 우회하는 데 사용될 수 있는 경우 심각도는 '치명적(Critical)'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**도구:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### 애플리케이션이 서브도메인을 통해 제3자 호스팅 또는 클라우드 서비스를 이용합니까?
- [ ] 아니요 — 모든 서브도메인이 조직에서 제어하는 IP 대역을 가리킵니다.
- [ ] 예 — 서브도메인이 제3자 제공업체(S3, GitHub Pages, Heroku 등)를 가리킵니다.

### 존재하지 않는 리소스를 가리키는 "연결되지 않은(Dangling)" DNS 레코드가 있습니까?
- [ ] 아니요 — 식별된 모든 DNS 레코드가 활성 상태이며 제어 가능한 리소스로 확인됩니다.
- [ ] 예 — 제공업체에 **더 이상 존재하지 않는** 리소스에 대한 CNAME 또는 A 레코드가 존재합니다.
- [ ] 예 — DNS 레코드가 NXDOMAIN 또는 제공업체별 오류 페이지 *(예: "NoSuchBucket")* 로 확인됩니다.

### 식별된 연결되지 않은 리소스를 성공적으로 점유할 수 있습니까?
- [ ] 아니요 — 제공업체가 방치된 이름의 점유를 방지하는 보호 조치를 갖추고 있거나 계정 인증이 필요합니다.
- [ ] 예 — 임의의 사용자가 제3자 플랫폼에서 해당 리소스 이름을 **등록할 수 있습니다**.

### 기본 도메인이 침해될 수 있는 "도메인(Domain)" 범위의 쿠키를 사용합니까?
- [ ] 아니요 — 쿠키의 범위가 특정 서브도메인으로 엄격히 제한되거나 `Host-Only` 플래그를 사용합니다.
- [ ] 예 — 민감한 쿠키(세션 ID, JWT 등)의 범위가 부모 도메인으로 설정되어 있으며 탈취된 서브도메인을 통해 **유출될 수 있습니다**.

### 서브도메인을 보안 헤더 또는 인증 흐름을 우회하는 데 사용할 수 있습니까?
- [ ] 아니요 — 식별된 서브도메인에 대한 보안 의존성이 없습니다.
- [ ] 예 — 서브도메인이 CSP `script-src` 또는 `connect-src` 지시어의 화이트리스트에 포함되어 있습니다.
- [ ] 예 — 서브도메인이 OAuth 또는 SSO 설정의 유효한 리다이렉트 URI입니다.

---

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

## WSTG-CONF-12 — 콘텐츠 보안 정책 (Content Security Policy, CSP)

콘텐츠 보안 정책 (Content Security Policy, CSP)은 크로스 사이트 스크립팅 (Cross-Site Scripting, XSS), 클릭재킹 (Clickjacking) 및 데이터 주입 (Data Injection) 공격과 같은 위험을 완화하기 위해 HTTP 응답 헤더를 통해 구현되는 중요한 심층 방어 (Defense-in-depth) 메커니즘입니다. 허용되는 동적 리소스를 정의함으로써, 잘 구성된 CSP는 공격자가 승인되지 않은 스크립트를 실행하거나 민감한 데이터를 외부 도메인으로 유출 (Exfiltrate)하는 능력을 제한합니다. 모의해킹 전문가 (Pentester)는 과도하게 허용적인 지시문, `'unsafe-inline'`과 같은 안전하지 않은 키워드의 사용, 우회 (Bypass)를 용이하게 할 수 있는 안전하지 않은 CDN에 대한 의존성 등을 기준으로 정책을 평가합니다. 약하거나 누락된 CSP가 직접적으로 취약점을 만드는 것은 아니지만, 보조 보호 계층을 제공하지 못함으로써 주입 공격 성공 시의 영향도를 크게 증가시킵니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 낮음 (Low) / 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**도구:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### 애플리케이션 응답에 콘텐츠 보안 정책 (CSP) 헤더가 존재하는가?
- [ ] 예 — `Content-Security-Policy` 헤더가 **존재**하며 **적용(Enforced)** 중임  
- [ ] 예 — 테스트를 위해 `Content-Security-Policy-Report-Only`가 **존재**함  
- [ ] 아니요 — 응답에 CSP 헤더가 **존재하지 않음**  

### `script-src` 또는 `default-src` 지시문이 적절히 제한되어 있는가?
- [ ] 예 — 지시문이 엄격한 화이트리스트, 넌스 (Nonce) 또는 해시 (Hash)를 사용하며 우회가 **불가능함**  
- [ ] 예 — 지시문이 **적절히 구성**되었으나, 화이트리스트에 우회 가능한 것으로 알려진 CDN이 포함되어 있음  
- [ ] 아니요 — 지시문에 와일드카드 (`*`) 또는 `data:` 스킴을 사용하여 우회가 **가능함**  

### 정책이 안전하지 않은 인라인 스크립트 또는 eval의 실행을 허용하는가?
- [ ] 아니요 — `'unsafe-inline'` 및 `'unsafe-eval'`이 **존재하지 않음**  
- [ ] 예 — `'unsafe-inline'`이 **존재**하지만 `nonce` 또는 `hash`에 의해 보호됨  
- [ ] 예 — `'unsafe-inline'` 또는 `'unsafe-eval'`이 추가적인 보호 조치 없이 **활성화**되어 있음 *(Critical)*  

### 애플리케이션이 `frame-ancestors` 지시문을 통해 클릭재킹으로부터 보호되는가?
- [ ] 예 — `frame-ancestors`가 `'none'` 또는 `'self'`로 설정됨  
- [ ] 예 — `frame-ancestors`가 **활성화**되어 있으며 신뢰할 수 있는 특정 제3자 도메인을 허용함  
- [ ] 아니요 — `frame-ancestors`가 **누락**되었으며, 레거시 `X-Frame-Options`에 의존하거나 보호 조치가 없음  

### CSP 위반 사항을 보고하는 메커니즘이 존재하는가?
- [ ] 예 — `report-uri` 또는 `report-to`가 **구성**되어 있으며 활성화 상태임  
- [ ] 아니요 — 위반 보고가 **비활성화**되어 있거나 **구성되지 않음**

---

## WSTG-CONF-13 — 경로 혼란 (Path Confusion)

경로 혼란 (Path Confusion)은 리버스 프록시 (Reverse Proxies), 로드 밸런서 (Load Balancers), 백엔드 애플리케이션 서버 (Backend Application Servers)와 같은 서로 다른 웹 구성 요소가 URL 경로를 구문 분석하고 해석하는 방식의 불일치로 인해 발생합니다. 공격자는 세미콜론, 인코딩된 슬래시 또는 도트 세그먼트 (Dot-segments)와 같은 특정 문자를 삽입하여 보안 필터를 속이고 제한된 엔드포인트나 정적 리소스에 접근함으로써 이러한 불일치를 악용합니다. 이 취약점은 프런트엔드 (Front-end)가 보안 규칙을 적용하는 경로와 백엔드 (Back-end)가 해석하는 경로가 서로 다를 때 발생하며, 인증 우회 (Authentication Bypass), 비인가 데이터 액세스 (Unauthorized Data Access), 웹 캐시 포이즈닝 (Web Cache Poisoning)과 같은 심각한 보안 실패로 이어질 수 있습니다. 성공적인 공격은 일반적으로 기술 스택 전반에서 요청 라우팅 및 정규화 (Normalization) 로직이 분기되는 아키텍처 경계에서 발생합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-13 |
| **CWE** | CWE-444 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 중간 (Medium) / 높음 (High)* |

> *경로 혼란으로 인해 민감한 관리자 엔드포인트에 대한 인증 또는 접근 제어 우회가 발생하는 경우 심각도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `Dirsearch`, `FFUF`, `ParamMiner`, `Arjun`

### 서로 다른 아키텍처 계층에서 경로 구분 기호(예: `;`, `#`, `?`)를 일관되게 해석합니까?
- [ ] 예 — 모든 계층에서 경로 구분 기호를 동일하게 정규화하고 해석함  
- [ ] 아니요 — 불일치가 존재하지만 제한된 리소스에 접근하는 데 사용될 수 없음  
- [ ] 아니요 — 불일치를 통해 공격자가 프런트엔드 필터를 우회하고 백엔드 로직에 도달하는 것이 **가능함**  

### 경로 탐색 (Path Traversal) 시퀀스나 인코딩된 문자를 사용하여 접근 제어를 우회할 수 있습니까?
- [ ] 아니요 — 보안 점검 전에 정규화가 일관되게 적용됨  
- [ ] 예 — 도트 세그먼트 시퀀스(예: `/admin/..;/`)를 통한 우회가 **가능함**  
- [ ] 예 — URL 인코딩된 문자(예: `%2f`, `%2e%2e%2f`)를 통한 우회가 **가능함**  

### 애플리케이션이 경로 혼란을 통한 웹 캐시 포이즈닝에 취약합니까?
- [ ] 아니요 — 캐싱 로직이 경로의 모호성이나 추가 경로 정보의 영향을 받지 않음  
- [ ] 예 — 매핑 불일치로 인해 악성 콘텐츠가 정상적인 경로에 캐시될 수 있음  
- [ ] 예 — 상대 경로 덮어쓰기 (Relative Path Overwrite - RCD) 기법을 통해 공용 디렉토리에 민감한 정보가 캐시될 수 있음  

### 서버가 "추가 경로 정보" (PathInfo)를 안전하게 처리합니까?
- [ ] 아니요 — 해당 기능이 비활성화되어 있거나 존재하지 않음  
- [ ] 예 — `PathInfo`가 처리되며 보안 필터를 방해하지 않음  
- [ ] 아니요 — `PathInfo`를 통해 공격자가 데이터를 추가하여 애플리케이션의 라우팅 로직을 혼란에 빠뜨릴 수 있음

---

## WSTG-CONF-14 — 기타 HTTP 보안 헤더 설정 오류 테스트

HTTP 보안 헤더 설정 오류(HTTP security header misconfigurations)에 대한 테스트는 일반적인 웹 기반 공격 벡터에 대해 필수적인 보호를 제공하는 방어 헤더의 존재 여부와 올바른 구현을 검증하는 작업을 포함합니다. 이러한 헤더들이 근본적인 코드 취약점을 해결하지는 못하지만, 헤더가 누락될 경우 중간자 공격(Man-in-the-Middle, MITM), MIME 스니핑(MIME-sniffing) 공격, 그리고 리퍼러(Referrers)를 통한 의도치 않은 교차 출처 데이터 유출(Cross-origin data leakage)의 위험이 크게 증가합니다. 공격자의 관점에서 보안 헤더의 누락은 보안 설정이 미흡한 환경임을 나타내는 가시성 높은 지표이며, 이는 종종 세션 하이재킹(Session hijacking)이나 정보 유출(Information exfiltration)과 같은 더 복잡한 익스플로잇(Exploit) 체인을 용이하게 합니다. 이러한 설정은 일반적으로 서버 수준에서 평가되며 API 엔드포인트(API endpoints) 및 정적 리소스(Static resources)를 포함한 모든 응답 유형에 일관되게 적용되어야 합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-14 |
| **CWE** | CWE-16 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/14-Test_Other_HTTP_Security_Header_Misconfigurations  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite (Repeater/Scanner)`, `OWASP ZAP`, `Hardenize`, `securityheaders.com`

### 보안 헤더 존재 여부 감사
| 헤더 | 존재 | 누락 | 권장 설정 |
|--------|:-------:|:-------:|---------------------------|
| `Strict-Transport-Security` |  |  | `max-age=63072000; includeSubDomains; preload` |
| `X-Content-Type-Options` |  |  | `nosniff` |
| `Referrer-Policy` |  |  | `no-referrer` 또는 `strict-origin-when-cross-origin` |
| `Permissions-Policy` |  |  | (기능별 설정, 예: `geolocation=()`) |
| `Cross-Origin-Opener-Policy` |  |  | `same-origin` |

### 애플리케이션 범위 전체에서 보안 헤더가 어떻게 강제되고 있습니까?
- [ ] 예 — 헤더가 중앙 로드 밸런서(Load balancer) 또는 리버스 프록시(Reverse proxy)를 통해 전역적으로 적용되며 재정의할 수 없음  
- [ ] 예 — 헤더가 주요 문서 응답에는 적용되지만, API 응답 또는 정적 자산에서는 누락됨  
- [ ] 아니요 — 헤더가 일관성 없이 적용되거나 전체 애플리케이션 도메인에서 누락됨  

### Strict-Transport-Security (HSTS) 헤더가 올바르게 설정되어 있습니까?
- [ ] 예 — `max-age`가 긴 기간(예: 2년)으로 설정되어 있고 `includeSubDomains`가 활성화됨  
- [ ] 예 — `max-age`는 설정되어 있으나 `includeSubDomains`가 비활성화되어 서브도메인이 취약한 상태임  
- [ ] 아니요 — HSTS 헤더가 존재하지 않거나 `max-age`가 0으로 설정되어 프로토콜 다운그레이드 공격(Protocol downgrade attacks)이 가능함  

### Referrer-Policy가 민감한 URL 파라미터의 유출을 방지합니까?
- [ ] 예 — 정책이 `no-referrer` 또는 `same-origin`으로 설정됨 (가장 안전)  
- [ ] 예 — 정책이 `strict-origin-when-cross-origin`으로 설정됨  
- [ ] 아니요 — 정책이 설정되지 않았거나 `unsafe-url`로 설정되어, 리퍼러 헤더를 통해 토큰이나 민감한 개인 식별 정보(Personally Identifiable Information, PII)가 유출될 가능성이 있음  

### X-Content-Type-Options 헤더가 MIME 스니핑을 방지하고 있습니까?
- [ ] 예 — `nosniff` 지시문이 존재하며 올바르게 구현됨  
- [ ] 아니요 — 헤더가 누락되어 브라우저가 실행 불가능한 파일(예: 이미지)을 스크립트로 실행할 가능성이 있음

---

## WSTG-IDNT-01 — 역할 정의 테스트 (Test Role Definitions)

역할 정의 테스트는 애플리케이션 내에 정의된 다양한 사용자 역할과 권한 수준을 식별하고 문서화하여, 이들이 논리적으로 분리되어 있으며 최소 권한의 원칙 (Principle of Least Privilege)을 준수하는지 확인하는 작업을 포함합니다. 공격자는 애플리케이션을 분석하여 고권한 역할과 일반 사용자 역할을 매핑하며, 권한 경계의 모호함이나 문서화되지 않은 관리 기능을 탐색합니다. 이러한 역할을 식별하는 것은 권한 상승 (Privilege Escalation) 경로를 발견하는 첫 번째 단계입니다. 역할 정의의 불일치는 종종 민감한 데이터나 관리 기능에 대한 무단 액세스로 이어지기 때문입니다. 이 단계는 일반적으로 초기 정찰 (Reconnaissance) 및 비즈니스 로직 매핑 (Business Logic Mapping) 중에 발생하며, 사용자 프로필, 관리 대시보드 및 API 권한 구조 (API Permission Structures)에 집중합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-01 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도 (Severity)** | 낮음 (Low) / 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Compare Site Maps)`, `Postman`, `AuthMatrix`, `Authz`, `Browser Developer Tools`

### 애플리케이션 또는 해당 문서에 역할이 명확하게 정의되고 문서화되어 있습니까?
- [ ] 예 — 역할이 완전히 문서화되어 있으며 **최소 권한의 원칙**을 준수함  
- [ ] 예 — 역할이 문서화되어 있으나 **중복된 권한**이 포함되어 있음  
- [ ] 아니요 — 공식적인 역할 문서 또는 정의가 존재하지 않음  

### 관리 기능과 일반 사용자 기능이 명확하게 분리되어 있습니까?
- [ ] 예 — 관리 기능이 일반 사용자 뷰로부터 **완전히 격리**되어 있음  
- [ ] 예 — 분리가 되어 있으나 관리 엔드포인트가 **예측 가능하거나 추측 가능함**  
- [ ] 아니요 — 동일한 인터페이스 내에 관리 기능과 일반 기능이 **혼재**되어 있음  

### 애플리케이션이 역할 기반 액세스 제어 (RBAC)를 위한 중앙 집중식 메커니즘을 사용합니까?
- [ ] 예 — 중앙 집중식 역할 기반 액세스 제어 (RBAC) 또는 속성 기반 액세스 제어 (ABAC)가 **구현**되어 있으며 일관되게 적용됨  
- [ ] 예 — 분산된 제어 기능이 존재하지만 모듈 전체에 **비일관적으로 적용됨**  
- [ ] 아니요 — 액세스 제어 로직이 개별 페이지나 구성 요소에 **하드코딩 (Hardcoded)**되어 있음  

### 시스템에 문서화되지 않거나 숨겨진 역할이 존재합니까?
- [ ] 아니요 — 발견된 모든 역할이 문서화된 기능과 일치함  
- [ ] 예 — 숨겨진 역할 또는 "섀도우(shadow)" 역할(예: `super_admin`, `support`, `test`)이 **존재함**  

### 낮은 권한의 사용자가 다른 사용자의 권한이나 역할을 열거 (Enumerate)할 수 있습니까?
- [ ] 아니요 — 사용자가 다른 엔티티의 역할/권한을 조회하거나 **열거할 수 없음**  
- [ ] 예 — 사용자가 프로필 페이지, 메타데이터 또는 API 응답을 통해 역할을 **열거할 수 있음** *(중간)*

---

## WSTG-IDNT-02 — 사용자 등록 프로세스 테스트

사용자 등록 프로세스 테스트는 새로운 식별 정보(Identity)가 생성되는 워크플로우를 평가하여, 무단 액세스나 자동화된 익스플로잇(Exploit)에 악용되지 않도록 보장하는 작업을 포함합니다. 이 프로세스의 취약점은 주로 신원 확인(이메일/SMS)의 부재, 봇(Bot)을 통한 대량 계정 생성 가능성, 또는 기존 사용자 이름을 노출하는 상세한 오류 메시지를 통한 정보 노출로 나타납니다. 공격자들은 이러한 결함을 악용하여 사용자 열거(User Enumeration)를 수행하거나, 대규모 스팸 캠페인을 진행하거나, 계정 생성 단계에서 등록 파라미터를 조작하여 권한 상승(Privilege Escalation)을 시도합니다. 이 테스트는 애플리케이션의 무결성을 보호하고 공격자가 시스템에 허위 또는 악성 계정을 대량으로 생성하는 것을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-02 |
| **CWE** | CWE-836 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**Tools:** `Burp Suite`, `OWASP ZAP`, `Python`, `ffuf`, `Cuppa`

### 새로운 계정이 활성화되기 전에 신원 확인이 필요합니까?
- [ ] 예 — 등록 시 이메일 또는 SMS를 통해 전송되는 고유하고 유효 시간이 제한된 토큰이 필요함  
- [ ] 예 — 확인이 필요하지만 토큰이 취약하거나 예측 가능함  
- [ ] 아니요 — 확인 단계 없이 계정이 즉시 활성화됨  

### 등록 프로세스가 사용자 열거(User Enumeration)를 방지합니까?
- [ ] 예 — 애플리케이션이 신규 및 기존 이메일/사용자 이름에 대해 동일한 응답을 반환함  
- [ ] 아니요 — 오류 메시지(예: "이미 사용 중인 이메일입니다")를 통해 공격자가 등록된 사용자를 식별할 수 있음  
- [ ] 아니요 — 서버 응답의 시간 차이로 인해 사용자 존재 여부가 노출됨  

### 대량 등록을 방지하기 위한 자동화 방지 제어(Anti-automation controls)가 구현되어 있습니까?
- [ ] 예 — CAPTCHA 또는 작업 증명(Proof-of-work) 메커니즘이 존재하며 효과적임  
- [ ] 예 — 제어 수단이 존재하지만 API 엔드포인트 또는 헤더 조작을 통해 우회가 가능함  
- [ ] 아니요 — 등록 엔드포인트에 속도 제한(Rate Limiting) 또는 CAPTCHA가 적용되지 않음  

### 권한 상승을 위해 등록 파라미터를 조작할 수 있습니까?
- [ ] 아니요 — 사용자 역할이 서버 측 로직에 의해 엄격하게 할당됨  
- [ ] 예 — 요청 내의 `role`, `admin` 또는 `group_id`와 같은 파라미터를 수정하여 더 높은 권한을 획득할 수 있음  

### 등록 단계에서 비밀번호 정책이 강제됩니까?
- [ ] 예 — 강력한 비밀번호 요구 사항이 서버 측에서 강제됨  
- [ ] 아니요 — 취약한 비밀번호(예: "password123")가 시스템에서 허용됨

---

## WSTG-IDNT-03 — 계정 생성 프로세스 테스트 (Account Provisioning Process)

계정 생성 프로세스(Account Provisioning Process)는 애플리케이션 환경 내에서 새로운 식별 정보(Identities)가 생성, 검증 및 권한이 할당되는 방식을 관리합니다. 공격자는 이 워크플로우를 타겟팅하여 스팸 또는 서비스 거부(Denial-of-Service, DoS)를 위한 자동화된 대량 등록을 수행하거나, 신원 확인 단계를 우회하여 부정 계정을 생성하거나, 가입 단계에서 파라미터를 조작하여 자신에게 높은 권한을 부여합니다. 취약점은 주로 셀프 서비스 등록 양식, 초대 전용 시스템 또는 비즈니스 로직 결함으로 인해 무단 계정 생성이나 권한 상승(Privilege Escalation)이 가능한 관리자용 프로비저닝 콘솔에서 나타납니다. 공격자의 관점에서 프로비저닝 흐름을 침해하는 것은 추가 공격, 데이터 유출 또는 사회 공학(Social Engineering) 공격을 위한 발판으로 사용할 수 있는 합법적인 거점을 확보하는 것과 같습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-03 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 중간(Medium) / 높음(High)* |

> *공격자가 관리자 계정을 생성하거나 전역 등록 제한을 우회할 수 있는 경우 심각도는 심각(Critical)으로 상향됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `FFUF`, `Postman`, `Python`, `MailHog`

### 자체 등록이 엄격하게 제어되거나 인가된 사용자로만 제한됩니까?
- [ ] 예 — 등록 기능이 **비활성화**되어 있거나 관리자의 수동 승인이 필요함  
- [ ] 예 — 등록이 개방되어 있으나 **인가된** 이메일 도메인 또는 초대 토큰(Invitation Token)으로 제한됨  
- [ ] 아니요 — 도메인이나 토큰 제한 없이 모든 사용자에게 등록이 **개방**되어 있음  

### 계정 활성화 전에 강력한 신원 확인 메커니즘(예: 이메일/SMS)이 필요합니까?
- [ ] 예 — 확인이 **필수적**이며 우회할 수 **없음**  
- [ ] 예 — 확인이 **필수적**이지만 파라미터 변조(Parameter Tampering) 또는 예측 가능한 토큰을 통해 우회 **가능함**  
- [ ] 아니요 — 확인 절차 없이 제출 즉시 계정이 **활성화**됨  

### 사용자가 등록 요청 중에 역할 또는 권한 파라미터를 조작할 수 있습니까?
- [ ] 아니요 — 역할이 **서버 측(Server-side)**에서 할당되며 클라이언트 측 파라미터가 존재하지 않음  
- [ ] 예 — 역할 파라미터가 존재하지만 서버 측 검증으로 인해 조작이 **불가능함**  
- [ ] 예 — 역할/권한 파라미터(예: `role=admin`, `is_staff=true`)를 수정하여 높은 권한을 획득할 수 **있음** *(심각, Critical)*  

### 대량의 계정 생성을 방지하기 위한 속도 제한(Rate Limiting) 또는 자동화 방지 제어가 구현되어 있습니까?
- [ ] 예 — 속도 제한 및 CAPTCHA가 **활성화**되어 있으며 효과적임  
- [ ] 예 — 제어 항목이 존재하지만 헤더 변조(Header Spoofing) 또는 CAPTCHA 해결 서비스를 통해 우회가 **가능함**  
- [ ] 아니요 — 속도 제한이나 자동화 방지 제어가 **적용되지 않음**  

### 계정 생성 프로세스에서 기존 계정에 대한 정보가 노출됩니까 (사용자 열거, User Enumeration)?
- [ ] 아니요 — 응답 메시지가 기존 사용자나 존재하지 않는 사용자 모두에게 **동일함**  
- [ ] 예 — 응답 메시지의 차이 또는 응답 시간의 차이로 인해 기존 사용자를 **열거**할 수 있음

---

## WSTG-IDNT-04 — 계정 열거 및 추측 가능한 사용자 계정 테스트

계정 열거 (Account Enumeration)는 애플리케이션이 HTTP 응답, 오류 메시지의 변화 또는 측정 가능한 타이밍 차이 (Timing Difference)를 통해 사용자 계정의 존재 여부를 노출할 때 발생합니다. 공격자는 사용자 이름 목록을 체계적으로 제출하여 유효한 계정을 식별함으로써 이 동작을 악용하며, 이는 이후 크리덴셜 스터핑 (Credential Stuffing), 브루트 포스 (Brute Force) 공격 또는 사회 공학 (Social Engineering)의 표적이 됩니다. 이 취약점은 주로 로그인 포털, 비밀번호 재설정 기능, 가입 양식 및 사용자별 식별자를 처리하는 API 엔드포인트에서 나타납니다. 공격자의 관점에서 성공적인 열거는 무분별한 브루트 포스 시도를 알려진 유효한 ID에 대한 표적 공격으로 전환하여 공격 표면 (Attack Surface)을 크게 좁힙니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-04 |
| **CWE** | CWE-204 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 낮음 / 중간 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Comparer)`, `ffuf`, `Wfuzz`, `GoBuster`, `Hashcat`

### 모든 시나리오에서 인증 오류 메시지가 일관됩니까?
- [ ] 예 — 유효하지 않은 사용자 이름과 유효하지 않은 비밀번호 모두에 대해 일반적인 오류 메시지가 사용됨  
- [ ] 예 — 메시지가 유사하지만, 구두점이나 대소문자에 약간의 차이가 **존재함**  
- [ ] 아니요 — 오류 메시지에 "사용자를 찾을 수 없음" 또는 "잘못된 비밀번호"라고 명시적으로 표시됨 *(취약함)*  

### 비밀번호 재설정 또는 "비밀번호 찾기" 흐름에서 계정 존재 여부가 유출됩니까?
- [ ] 아니요 — 이메일/사용자 이름의 존재 여부와 관계없이 애플리케이션이 일반적인 메시지를 반환함  
- [ ] 예 — 제어 장치가 마련되어 있으나, 응답 길이나 상태 코드 (Status Code)를 통해 우회 **가능함**  
- [ ] 예 — 애플리케이션이 재설정 이메일 전송 여부 또는 계정이 **존재하지 않음**을 명시적으로 확인해줌  

### 가입 프로세스가 사용자 열거로부터 보호됩니까?
- [ ] 아니요 — 가입 과정에서 정보가 유출되지 않거나 캡차 (CAPTCHA) 또는 속도 제한 (Rate Limiting)을 사용하여 대량 확인을 방지함  
- [ ] 예 — 제어 장치가 마련되어 있으나, 타이밍 또는 API 응답 차이를 통해 우회 **가능함**  
- [ ] 예 — 사용자 이름 또는 이메일이 **이미 사용 중**인 경우 애플리케이션이 사용자에게 즉시 알림  

### 유효한 사용자 이름과 유효하지 않은 사용자 이름을 비교할 때 측정 가능한 타이밍 차이가 있습니까?
- [ ] 아니요 — 상수 시간 비교 (Constant-time comparison)로 인해 계정 유효성에 관계없이 응답 시간이 일정함  
- [ ] 예 — 타이밍 차이가 존재하지만 네트워크를 통해 안정적으로 측정하기에는 너무 작음  
- [ ] 예 — 측정 가능한 타이밍 차이가 **존재함** (예: 유효한 사용자에 대해서만 비밀번호 해싱 (Hashing)이 실행됨)  

### 애플리케이션이 계정에 대해 예측 가능하거나 추측 가능한 명명 규칙을 사용합니까?
- [ ] 아니요 — 계정 식별자가 무작위(UUID)이거나 사용자 정의이며 복잡함  
- [ ] 예 — 계정 식별자가 패턴(예: `emp001`, `emp002`)을 따르며 열거가 **가능함**  
- [ ] 예 — 식별자가 순차적 정수이므로 전체 데이터베이스 열거가 **가능함**

---

## WSTG-IDNT-05 — 취약하거나 강제되지 않은 사용자 이름 정책 테스트 (Testing for Weak or Unenforced Username Policy)

취약하거나 강제되지 않은 사용자 이름 정책(Username Policy) 테스트는 계정 식별자 생성 규칙과 이들이 열거(Enumeration) 또는 스푸핑(Spoofing)에 취약한지 여부에 집중합니다. 취약한 정책은 종종 예측 가능한 시퀀스, 일반적인 사전 단어 또는 내부 직원 ID를 모방한 식별자를 허용하며, 이는 무작위 대입 공격(Brute Force) 및 크리덴셜 스터핑(Credential Stuffing) 공격의 탐색 범위를 크게 줄입니다. 공격자 관점에서 이러한 패턴을 식별하는 것은 대규모 계정 발견의 첫 번째 단계이며, 주로 가입 오류 메시지, 비밀번호 재설정 동작 또는 공개 프로필의 메타데이터(Metadata) 분석을 통해 수행됩니다. 견고한 정책을 보장하면 공격자가 체계적으로 사용자 기반을 매핑하는 것을 방지하고 표적 피싱(Phishing) 및 자동화된 인증 우회(Authentication Bypass) 시도에 대한 방어를 용이하게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-IDNT-05 |
| **CWE** | CWE-521 |
| **테스트 상태** | Not Performed |
| **심각도** | Low / Medium |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder)`, `ffuf`, `Custom Python Scripts`, `theHarvester`

### 사용자 이름이 매우 예측 가능하거나 순차적인 패턴을 기반으로 합니까?
- [ ] 아니오 — 사용자 이름이 무작위이거나, 사용자 정의 또는 높은 엔트로피(Entropy)를 가진 문자열입니다.  
- [ ] 예 — 사용자 이름이 예측 가능한 형식(예: `user1001`, `user1002`)을 따르지만 인증이 견고합니다.  
- [ ] 예 — 사용자 이름이 **엄격하게 순차적**이거나 알려진 기업 형식을 따르며, 용이한 열거를 가능하게 합니다.  

### 애플리케이션이 사용자 이름에 대해 최소 길이 및 복잡성 요구 사항을 강제합니까?
- [ ] 예 — 가입 시 엄격한 길이 및 문자 집합 정책이 **강제됩니다**.  
- [ ] 예 — 정책이 존재하지만 매우 짧은(예: 1-2자) 또는 지나치게 단순한 사용자 이름을 허용합니다.  
- [ ] 아니오 — 사용자 이름 길이 또는 문자 유형에 관한 **정책이 강제되지 않습니다**.  

### 애플리케이션 응답을 통해 유효한 사용자 이름을 열거할 수 있습니까?
- [ ] 아니오 — 애플리케이션이 일반적인 오류 메시지를 반환하고 모든 시도에 대해 일관된 타이밍을 유지합니다.  
- [ ] 예 — 애플리케이션이 별도의 오류 메시지(예: "이미 사용 중인 사용자 이름입니다")를 반환하지만 전송률 제한(Rate Limiting)이 **적용됩니다**.  
- [ ] 예 — 애플리케이션이 가입 오류, 비밀번호 재설정 또는 타이밍 차이를 통해 유효한 사용자 이름을 **유출**합니다.  

### 정책이 일반적이거나 예약된 관리자용 사용자 이름의 가입을 방지합니까?
- [ ] 예 — 애플리케이션이 일반적인 이름(예: `admin`, `root`, `support`) 및 시스템 예약어를 차단합니다.  
- [ ] 아니오 — 사용자가 민감하거나 관리자 권한을 나타내는 이름으로 계정을 **등록할 수 있습니다**.  

### 사용자 이름 정책이 모든 진입점(API, Mobile, Web)에서 일관되게 적용됩니까?
- [ ] 예 — 모든 인터페이스와 버전에서 정책이 일관되게 **적용됩니다**.  
- [ ] 아니오 — 레거시 엔드포인트 또는 특정 API 버전에서 동일한 사용자 이름 제약 조건을 **강제하지 않습니다**.

---

## WSTG-ATHN-01 — 암호화된 채널을 통한 자격 증명(Credentials) 전송 테스트

암호화된 채널을 통한 자격 증명 전송 여부를 테스트함으로써 사용자 이름, 비밀번호 및 세션 토큰(Session Tokens)과 같은 민감한 인증 데이터가 전송 중에 가로채기(Interception)로부터 보호되는지 확인합니다. 공용 Wi-Fi에서의 중간자 공격(Man-in-the-Middle, MitM) 또는 침해된 내부 네트워크와 같이 네트워크에 위치한 공격자는 TLS/SSL이 적절하게 강제되지 않을 경우 패킷 스니핑(Packet Sniffing) 도구를 사용하여 평문(Cleartext) 자격 증명을 캡처할 수 있습니다. 이 취약점은 일반적으로 애플리케이션이 HTTPS를 의무화하지 않거나 HTTP에서 부적절하게 리다이렉트(Redirect)하는 로그인 엔드포인트, 비밀번호 재설정 양식 및 API 인증 헤더에서 발생합니다. 성공적인 공격은 공격자에게 사용자 계정에 대한 전체 액세스 권한을 부여하여 사용자 데이터의 완전한 침해 및 애플리케이션 내에서의 잠재적인 측면 이동(Lateral Movement)으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### 전체 인증 프로세스에 HTTPS가 강제됩니까?
- [ ] 예 — HSTS가 활성화(Enabled)되어 있으며 모든 트래픽이 HTTPS를 통해 엄격하게 강제됩니다.
- [ ] 예 — HTTPS로의 리다이렉트가 발생하지만, HSTS는 활성화되지 않았습니다.
- [ ] 아니요 — 자격 증명이 암호화되지 않은 HTTP를 통해 제출될 수 있습니다.

### 로그인 페이지와 양식이 암호화된 채널을 통해 제공됩니까?
- [ ] 예 — 로그인 양식을 포함하는 페이지가 HTTPS를 통해 전달됩니다.
- [ ] 아니요 — 로그인 페이지가 HTTP를 통해 제공되어 폼 액션 하이재킹(Form-action Hijacking) 또는 자격 증명 스니핑이 가능합니다.

### 모든 민감한 세션 쿠키에 `Secure` 플래그가 적용되어 있습니까?
- [ ] 예 — 모든 인증 및 세션 쿠키에 `Secure` 플래그가 적용되어 있습니다.
- [ ] 예 — 일부 쿠키에만 `Secure` 플래그가 적용되어 있습니다.
- [ ] 아니요 — `Secure` 플래그가 적용되지 않아 암호화되지 않은 요청을 통해 쿠키가 유출될 수 있습니다.

### 서버가 취약한 TLS 버전이나 안전하지 않은 암호화 스위트(Cipher Suites)를 지원합니까?
- [ ] 아니요 — 현대적인 TLS(1.2+) 및 강력한 암호화 알고리즘만 활성화되어 있습니다.
- [ ] 예 — 레거시 버전(TLS 1.0/1.1) 또는 취약한 암호화 알고리즘(RC4, DES, CBC)이 지원됩니다.

### 애플리케이션이 암호화되지 않은 채널을 통해 서드파티(Third-party) 도메인으로 자격 증명이 전송되는 것을 방지합니까?
- [ ] 예 — 외부 도메인으로 민감한 데이터가 전송되지 않거나 모든 외부 호출이 HTTPS를 통해 강제됩니다.
- [ ] 아니요 — 인증 헤더 또는 자격 증명이 HTTP를 통해 서드파티 엔드포인트(예: 분석 도구 또는 SSO)로 전송됩니다.

---

## WSTG-ATHN-02 — 기본 자격 증명 점검 (Testing for Default Credentials)

기본 자격 증명 점검(Testing for Default Credentials)은 공장 출고 시 설정되거나 벤더가 제공한 사용자 이름 및 비밀번호를 여전히 사용 중인 관리자 인터페이스, 서비스 또는 애플리케이션 구성 요소를 식별하는 작업을 포함합니다. 이 취약점은 공격자에게 최소한의 노력으로 시스템 전체 장악, 관리자 권한 획득 또는 원격 코드 실행(Remote Code Execution, RCE)으로 이어지는 즉각적인 경로를 제공하기 때문에 우선순위가 높은 대상입니다. 이는 일반적으로 상용 소프트웨어(Off-the-shelf software), CMS 플랫폼, 데이터베이스 관리 도구, 그리고 IoT 기기나 네트워크 어플라이언스(Network appliances)와 같은 통합 하드웨어에서 발생합니다. 취약점 공격(Exploitation)은 보통 초기 정찰(Reconnaissance) 및 공격 단계에서 식별된 소프트웨어 버전을 공개된 기본 비밀번호 데이터베이스와 대조하거나 자동화된 자격 증명 스터핑(Credential stuffing) 도구를 사용하여 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-02 |
| **CWE** | CWE-1392 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각(Critical) / 높음(High) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/02-Testing_for_Default_Credentials  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Hydra`, `Medusa`, `Burp Suite (Intruder)`, `Nmap`, `Metasploit`, `DefaultPassword.com`

### 관리자 또는 관리 인터페이스가 인터넷이나 신뢰할 수 없는 네트워크 세그먼트에 노출되어 있습니까?
- [ ] 아니요 — 관리 인터페이스에 접근할 수 없습니다  
- [ ] 예 — 인터페이스가 존재하지만 네트워크 수준의 제어(예: VPN, IP 허용 목록(Allowlist))에 의해 제한됩니다  
- [ ] 예 — 인터페이스가 공개적으로 접근 가능하며 인증이 필요합니다  

### 식별된 모든 구성 요소에 대해 벤더가 제공한 기본 자격 증명이 변경되었습니까?
- [ ] 예 — 모든 구성 요소에서 모든 기본 자격 증명이 변경되었습니다 *(가장 안전)*  
- [ ] 예 — 주요 애플리케이션의 자격 증명은 변경되었으나, 보조 구성 요소(예: CMS, DB)에는 기본값이 남아 있습니다  
- [ ] 아니요 — 하나 이상의 식별 가능한 인터페이스에서 기본 자격 증명이 활성화되어 있습니다 *(심각)*  

### 애플리케이션이 신규 또는 관리자 계정의 첫 로그인 시 비밀번호 변경을 강제합니까?
- [ ] 예 — 어떠한 관리 작업도 수행하기 전에 비밀번호 변경이 필수적으로 요구됩니다  
- [ ] 아니요 — 애플리케이션이 기본 자격 증명을 무기한으로 사용할 수 있도록 허용합니다  

### 자동화된 기본 자격 증명 테스트를 방지하기 위한 계정 잠금 메커니즘이나 속도 제한(Rate Limiting) 제어가 활성화되어 있습니까?
- [ ] 예 — 강력한 속도 제한 또는 계정 잠금이 적용됩니다  
- [ ] 예 — 제어 장치가 존재하지만 헤더 조작이나 IP 순환(Rotation)을 통해 우회할 수 있습니다  
- [ ] 아니요 — 속도 제한이나 잠금 메커니즘이 활성화되어 있지 않습니다  

### 타사 플러그인, 미들웨어 또는 관리 도구(예: phpMyAdmin, Tomcat Manager)가 고유한 자격 증명을 사용합니까?
- [ ] 아니요 — 환경에 타사 구성 요소가 존재하지 않습니다  
- [ ] 예 — 모든 타사 구성 요소가 기본값이 아닌 고유한 자격 증명을 사용합니다  
- [ ] 아니요 — 적어도 하나의 타사 구성 요소를 알려진 기본 자격 증명을 통해 접근할 수 있습니다

---

## WSTG-ATHN-03 — 취약한 계정 잠금 메커니즘 테스트 (Testing for Weak Lock Out Mechanism)

계정 잠금 메커니즘(Account Lockout Mechanism)은 지정된 횟수의 인증 시도 실패 후 계정을 비활성화함으로써 무차별 대입(Brute Force) 및 크리덴셜 스터핑(Credential Stuffing) 공격을 완화하도록 설계된 중요한 심층 방어(Defense-in-depth) 통제 항목입니다. 강력한 잠금 정책이 없으면 공격자는 자동화된 도구를 사용하여 여러 계정에 대해 체계적으로 비밀번호를 추측(패스워드 스프레이, Password Spraying)하거나 단일 고가치 계정이 성공할 때까지 공격할 수 있습니다. 이러한 취약점은 일반적으로 로그인 포털, 비밀번호 재설정 양식 및 속도 제한(Rate Limiting) 또는 계정 수준의 보호가 부족한 API 엔드포인트(API Endpoints)에서 나타납니다. 공격자의 관점에서 취약하거나 없는 잠금 메커니즘은 무한한 비밀번호 시도를 허용하는 반면, 지나치게 공격적인 잠금 메커니즘은 의도적으로 계정을 잠금으로써 정당한 사용자에 대한 분산 서비스 거부(Denial of Service, DoS)를 유발하는 데 악용될 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-03 |
| **CWE** | CWE-307 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *애플리케이션이 민감한 개인 식별 정보(Personally Identifiable Information, PII), 금융 데이터를 처리하거나 보조 통제 수단 없이 관리자 계정이 무차별 대입에 취약한 경우 위험도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite Intruder`, `THC-Hydra`, `ffuf`, `Patator`, `wfuzz`

### 정해진 횟수의 인증 시도 실패 후 계정 잠금 메커니즘이 강제 적용됩니까?
- [ ] 예 — 정의된 합리적인 임계값(예: 5-10회 시도) 후에 계정이 잠깁니다.
- [ ] 예 — 계정이 잠기지만 시도 횟수가 지나치게 높습니다.
- [ ] 아니요 — 계정을 잠글 수 없으며 무한한 무차별 대입을 허용합니다.

### 일반적인 우회 기법을 사용하여 잠금 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 잠금은 서버 측에서 강제 적용되며 클라이언트 측 변경 사항의 영향을 받지 않습니다.
- [ ] 예 — IP 주소를 교체하거나 프록시 풀을 사용하여 잠금을 우회할 수 있습니다.
- [ ] 예 — `X-Forwarded-For` 또는 `User-Agent`와 같은 HTTP 헤더를 조작하여 잠금을 우회할 수 있습니다.
- [ ] 예 — 쿠키를 삭제하거나 새 세션을 시작하여 잠금을 우회할 수 있습니다.

### 잠금 메커니즘이 계정 복구 또는 잠금 해제 경로를 제공합니까?
- [ ] 예 — 일정 기간이 지난 후 또는 이메일 인증을 통해 계정이 자동으로 잠금 해제됩니다.
- [ ] 예 — 계정 잠금을 해제하려면 관리자의 개입이 필요합니다.
- [ ] 아니요 — 명확한 복구 경로 없이 잠금이 영구적이어서 DoS 위험이 증가합니다.

### 애플리케이션이 잠금 발생 시 유효한 사용자 이름과 유효하지 않은 사용자 이름을 구분합니까?
- [ ] 아니요 — 기존 사용자와 존재하지 않는 사용자 모두에 대해 애플리케이션 응답이 동일합니다.
- [ ] 예 — 애플리케이션이 계정 잠금 여부를 드러내어 사용자 이름 열거(Username Enumeration)를 허용합니다.

### 잠금 메커니즘이 서비스 거부(DoS) 공격에 취약합니까?
- [ ] 아니요 — CAPTCHA 또는 IP 기반 속도 제한과 같은 통제 수단이 대규모 계정 잠금을 방지합니다.
- [ ] 예 — 공격자가 잘못된 비밀번호를 제공하여 알려진 모든 사용자를 체계적으로 잠글 수 있습니다.

---

## WSTG-ATHN-04 — 인증 스키마 우회 테스트 (Testing for Bypassing Authentication Schema)

인증 스키마 우회(Bypassing Authentication Schema)는 유효한 자격 증명(Credentials)을 제공하지 않고도 공격자가 보호된 리소스에 접근할 수 있게 하는 애플리케이션 로직의 결함을 식별하고 악용하는 과정을 포함합니다. 이 취약점은 일반적으로 애플리케이션이 클라이언트 측 제어에 의존하거나, 모든 요청에 대해 서버 측 권한 부여(Server-side authorization)를 강제하지 못하거나, 관리자용 "백도어(Backdoors)" 및 디버그 엔드포인트(Debug endpoints)가 운영 환경에 노출되어 있을 때 발생합니다. 공격자는 민감한 URL에 대한 강제 브라우징(Forced Browsing)을 수행하거나, `authenticated=true`와 같은 HTTP 파라미터를 조작하거나, 헤더를 스푸핑(Spoofing)하여 세션(Session)이 이미 수립된 것으로 애플리케이션을 속임으로써 이러한 약점을 악용합니다. 성공적인 공격은 인증 경계의 완전한 붕괴를 초래하며, 잠재적으로 무단 데이터 유출이나 전체 시스템 침해로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-04 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 위험 (Critical) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/04-Testing_for_Bypassing_Authentication_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite`, `Ffuf`, `Gobuster`, `Dirsearch`, `Postman`

### 활성 세션 없이 민감한 내부 페이지에 직접 접근할 수 있습니까?
- [ ] 아니오 — 직접 접근 시 로그인 페이지로 리다이렉션되거나 403/404 응답이 반환됨  
- [ ] 예 — 일부 민감한 페이지에 접근 가능하지만 제한적이거나 민감하지 않은 데이터가 제공됨  
- [ ] 예 — 민감한 관리자용 또는 사용자 전용 페이지가 인증 없이 완전히 접근 가능함 *(Critical)*  

### 애플리케이션이 인증 상태를 결정하기 위해 클라이언트 측 플래그나 파라미터에 의존합니까?
- [ ] 아니오 — 인증 상태는 서버 측 세션 상태를 통해 엄격하게 관리됨  
- [ ] 예 — 클라이언트 측 플래그가 존재하지만 이를 수정해도 보호된 영역에 대한 접근 권한이 부여되지 않음  
- [ ] 예 — 파라미터(예: `is_authenticated=true`, `user_role=admin`)를 수정하여 인증 우회가 가능함  

### 특정 HTTP 헤더를 스푸핑하거나 조작하여 인증을 우회할 수 있습니까?
- [ ] 아니오 — `X-Forwarded-For`, `Referer` 또는 사용자 정의 헤더와 같은 헤더가 인증에 영향을 미치지 않음  
- [ ] 예 — 헤더(예: `X-Custom-IP-Authorization`, `X-Remote-User`)를 조작하는 것이 가능하며 무단 접근 권한을 얻을 수 있음  

### 표준 로그인 흐름을 우회하는 대체 경로 또는 개발 아티팩트(Artifacts)가 존재합니까?
- [ ] 아니오 — 모든 보호된 리소스는 중앙화된 운영 수준의 인증 확인을 거침  
- [ ] 예 — 레거시 엔드포인트, 디버그 경로(예: `/debug/login`) 또는 잊혀진 API 버전이 자격 증명 없이 접근 가능함  

### 애플리케이션이 높은 권한의 작업에 대해 재인증을 수행하거나 세션을 검증하지 못합니까?
- [ ] 아니오 — 높은 권한이 필요하거나 민감한 작업은 새로운 또는 유효한 세션 토큰을 요구함  
- [ ] 예 — 한 엔드포인트에서 우회 방법이 발견되면, 이를 사용하여 애플리케이션 전반에서 관리자 작업을 수행할 수 있음

---

## WSTG-ATHN-05 — 취약한 암호 기억 기능 테스트 (Testing for Vulnerable Remember Password)

"암호 기억(Remember Me)" 또는 지속적 로그인(Persistent Login) 기능은 클라이언트 측에 토큰(Token)이나 자격 증명(Credential)을 저장하여 브라우저 재시작 후에도 사용자의 인증 상태를 유지하도록 설계되었습니다. 이 기능이 평문 암호(Plaintext Password), 취약한 해시(Weak Hash), 또는 낮은 엔트로피(Low-entropy) 토큰을 쿠키에 저장하는 등 부적절하게 구현된 경우, 사용자는 교차 사이트 스크립팅(XSS), 물리적 접근 또는 네트워크 가로채기를 통한 계정 탈취(Account Takeover) 위험에 노출됩니다. 모의 침투 테스터는 이러한 지속성 토큰의 엔트로피, 저장 메커니즘에 적용된 보안 플래그, 세션의 생명주기 등을 평가하여 지속적 접근의 편의성이 다요소 인증(MFA)이나 세션 무효화(Session Invalidation)와 같은 중요한 보안 통제를 우회(Bypass)하지 않는지 확인해야 합니다. 취약점 공격은 주로 이러한 장기 생존 토큰을 수집하여 원래의 자격 증명 없이 계정에 무단으로 접근하는 방식으로 이루어집니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-05 |
| **CWE** | CWE-522 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *지속적 쿠키에 평문 자격 증명 또는 솔트가 없는 해시(Unsalted Hash)가 저장되어 있거나, 토큰이 다요소 인증(MFA)을 우회하는 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/05-Testing_for_Vulnerable_Remember_Password  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  
* https://portswigger.net/web-security/authentication  

**도구:** `Burp Suite (Cookie Editor/Sequencer)`, `Browser Developer Tools`, `EditThisCookie`

### 애플리케이션이 "암호 기억" 또는 "로그인 상태 유지" 기능을 구현하고 있습니까?
- [ ] 아니오 — 해당 기능이 애플리케이션에 존재하지 않음  
- [ ] 예 — 기능이 존재하며 암호학적으로 강력하고 예측 불가능한 토큰을 사용함  
- [ ] 예 — 기능이 존재하지만 예측 가능하거나 낮은 엔트로피 토큰을 사용함 *(중간)*  
- [ ] 예 — 기능이 존재하며 평문 자격 증명 또는 Base64로 인코딩된 암호를 저장함 *(높음)*  

### 지속적 쿠키에 보안 플래그가 올바르게 적용되어 있습니까?
- [ ] 예 — `HttpOnly`, `Secure`, `SameSite` 플래그가 **적용됨**  
- [ ] 예 — 일부 플래그가 적용되었으나 `HttpOnly` 플래그가 **누락됨**  
- [ ] 예 — 일부 플래그가 적용되었으나 HTTPS 환경에서 `Secure` 플래그가 **누락됨**  
- [ ] 아니오 — 지속적 쿠키에 보안 플래그가 **전혀 적용되지 않음**  

### 수동 로그아웃 후에도 "암호 기억" 토큰이 유효하게 유지됩니까?
- [ ] 아니오 — 로그아웃 즉시 서버 측에서 토큰이 무효화됨  
- [ ] 예 — 토큰이 유효하게 유지되지만 민감한 작업 수행 시 암호 입력을 요구함  
- [ ] 예 — 토큰이 유효하게 유지되며 재인증 **없이** 전체 계정 권한에 접근 가능함 *(중간)*  

### 암호 변경 시 지속적 세션이 종료됩니까?
- [ ] 예 — 사용자가 암호를 변경할 때 모든 지속적 토큰이 폐기됨  
- [ ] 아니오 — 암호 재설정/변경이 **수행된 후에도** "암호 기억" 토큰이 유효하게 유지됨 *(높음)*  

### 이후 방문 시 지속적 토큰이 다요소 인증(MFA)을 우회합니까?
- [ ] 아니오 — "암호 기억" 토큰이 있더라도 MFA가 요구됨  
- [ ] 예 — MFA를 우회하지만, 토큰이 특정 기기 지문(Device Fingerprint)에 결합되어 있음  
- [ ] 예 — 어떤 기기에서든 지속적 쿠키만으로 MFA를 **완전히 우회**함 *(높음)*

---

## WSTG-ATHN-06 — 브라우저 캐시 취약점 (Browser Cache Weakness) 테스트

브라우저 캐시 취약점(Browser Cache Weakness)은 민감한 정보가 로컬 브라우저 캐시에 저장되어, 동일한 물리적 장치에 접근할 수 있는 권한이 없는 사용자가 해당 정보를 조회할 수 있을 때 발생합니다. 적절한 캐시 제어 헤더(Cache Control Headers)가 부족하면 사용자가 로그아웃하거나 세션을 종료한 후에도 개인 정보, 계정 세부 정보 또는 세션 식별자(Session Identifiers)와 같은 잠재적으로 민감한 데이터가 그대로 남게 됩니다. 공격자나 공용 터미널(Shared or Public Terminals)의 다음 사용자는 브라우저 기록을 탐색하거나, "뒤로 가기" 버튼을 사용하거나, 로컬 캐시 파일을 조사하여 캐싱된 응답을 탈취(Exfiltrate)함으로써 이를 악용할 수 있습니다. 인증된 콘텐츠를 처리하는 모든 애플리케이션은 민감한 데이터가 디스크에 기록되지 않도록 `Cache-Control: no-store` 및 `Pragma: no-cache`와 같은 엄격한 지시어를 구현하는 것이 중요합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 낮음 (Low) / 중간 (Medium)* |

> *애플리케이션이 공용 키오스크에서 자주 사용되거나 매우 민감한 개인정보(PII)/보호대상 건강정보(PHI) 데이터를 포함하는 경우 심각도는 '중간(Medium)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**도구:** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### 인증되었거나 민감한 페이지에 적절한 캐시 제어 헤더가 존재합니까?
- [ ] 예 — `Cache-Control: no-store, no-cache` 및 `Pragma: no-cache`가 **존재**하며 **올바르게 구성**되어 있음  
- [ ] 예 — 일부 헤더가 존재하지만 `no-store`가 **누락**되어 디스크 캐싱이 허용됨  
- [ ] 아니요 — 캐시 관련 헤더가 **적용되지 않아** 브라우저의 기본 동작에 의존함  

### 애플리케이션이 사용자별 데이터에 대해 `private` 지시어를 사용합니까?
- [ ] 예 — 모든 개인화된 콘텐츠에 대해 `Cache-Control: private`이 **활성화**되어 프록시 캐싱(Proxy Caching)을 방지함  
- [ ] 아니요 — 콘텐츠가 `public`으로 표시되어 있거나 `private` 지시어가 부족하여 중간 프록시에 의해 **캐싱될 수 있음**  

### 성공적인 로그아웃 후 브라우저의 "뒤로 가기" 버튼을 통해 민감한 정보에 접근할 수 있습니까?
- [ ] 아니요 — 브라우저가 재인증을 요청하거나 로컬 캐시에서 페이지를 **불러오지 않음**  
- [ ] 예 — 세션이 종료된 후에도 뒤로 가기 버튼을 통해 민감한 정보가 **여전히 표시됨**  

### 민감한 비 HTML 파일(예: PDF, CSV 내보내기, JSON API 응답)이 캐싱되지 않도록 보호되고 있습니까?
- [ ] 아니요 — 애플리케이션에서 민감한 비 HTML 파일이 생성되거나 처리되지 않음  
- [ ] 예 — 모든 민감한 파일 다운로드 및 API 엔드포인트(API Endpoints)에 엄격한 캐시 제어 헤더가 **적용됨**  
- [ ] 아니요 — 민감한 다운로드 또는 API 응답이 로컬 브라우저 캐시 또는 임시 디렉토리에 **저장됨**  

### 애플리케이션이 오래된 데이터 사용을 방지하기 위해 `Expires: 0` 또는 과거의 날짜를 사용합니까?
- [ ] 예 — `Expires` 헤더가 `0` 또는 과거 날짜로 설정되어 브라우저가 콘텐츠를 즉시 만료된 것으로 **처리하도록 보장함**  
- [ ] 아니요 — 민감한 페이지에 `Expires` 헤더가 **누락**되었거나 미래의 날짜로 설정되어 있음

---

## WSTG-ATHN-07 — 취약한 비밀번호 정책 테스트 (Testing for Weak Password Policy)

취약한 비밀번호 정책 테스트는 계정 생성 및 자격 증명(Credential) 업데이트 중에 애플리케이션이 강제하는 복잡성, 길이 및 엔트로피(Entropy) 요구 사항을 평가하는 것을 포함합니다. 불충분한 정책은 사용자 계정을 사전 공격(Dictionary Attack), 무차별 대입 공격(Brute-force), 그리고 자격 증명 스터핑(Credential Stuffing)에 취약하게 만들어 기밀성을 직접적으로 침해합니다. 이러한 취약점은 일반적으로 등록 엔드포인트(Registration Endpoint), 비밀번호 재설정 워크플로우 및 관리자 사용자 관리 패널에 존재합니다. 공격자 관점에서 관대한 정책은 일반적인 워드리스트(Wordlist)나 패턴 기반 크래킹(Pattern-based Cracking)을 사용하여 자격 증명을 성공적으로 탈취하는 데 필요한 계산 비용과 시간을 크게 줄여줍니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-07 |
| **CWE** | CWE-521 |
| **테스트 상태** | Not Performed |
| **심각도** | Medium / Low* |

> *심각도는 애플리케이션에 자동화된 추측을 방지하기 위한 계정 잠금(Account Lockout) 또는 속도 제한(Rate Limiting) 메커니즘이 부족한 경우 High가 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/07-Testing_for_Weak_Authentication_Methods  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**도구:** `Burp Suite (Intruder)`, `ZAP (Fuzzer)`, `Hydra`, `Hashcat`, `Cewl`

### 애플리케이션에서 최소 비밀번호 길이를 강제합니까?
- [ ] 예 — 최소 길이는 12자 이상입니다 *(가장 안전함)*  
- [ ] 예 — 최소 길이는 8자에서 11자 사이입니다  
- [ ] 예 — 최소 길이는 8자 **미만**입니다  
- [ ] 아니요 — 최소 길이가 강제되지 않습니다  

### 복잡성 요구 사항(대문자, 소문자, 숫자, 특수문자)이 강제됩니까?
- [ ] 예 — 여러 문자 클래스가 필수이며 우회(Bypass)가 **불가능**합니다  
- [ ] 예 — 문자 클래스가 권장되지만 강제되지는 **않습니다**  
- [ ] 아니요 — 복잡성 요구 사항이 적용되지 않습니다  

### 애플리케이션이 흔하거나 취약한 비밀번호 또는 사용자 관련 데이터가 포함된 비밀번호를 차단합니까?
- [ ] 예 — 알려진 취약한 비밀번호(예: "password123") 및 사용자 이름 기반 비밀번호를 사용할 수 **없습니다**  
- [ ] 예 — 흔한 비밀번호는 차단되지만, 사용자 관련 데이터(예: 사용자 이름, 이메일)는 사용될 수 **있습니다**  
- [ ] 아니요 — 길이/복잡성 요구 사항을 충족하는 모든 문자열이 허용됩니다  

### 클라이언트 측(Client-side) 수정을 통해 복잡성 또는 길이 요구 사항을 우회할 수 있습니까?
- [ ] 아니요 — 정책이 **서버 측(Server-side)**에서 엄격하게 강제됩니다  
- [ ] 예 — 정책이 JavaScript를 통해 강제되며, 요청을 가로채서 우회할 수 **있습니다**  

### 구현 세부 정보의 유출 없이 비밀번호 정책이 사용자에게 명확하게 전달됩니까?
- [ ] 예 — 입력 중에 정책이 표시되며 일반적인 실패 메시지를 제공합니다  
- [ ] 예 — 정책이 표시되지만 실패 메시지에 특정 정규 표현식(Regex) 또는 로직의 허점이 드러납니다  
- [ ] 아니요 — 정책이 표시되지 않아 시행착오를 통해 찾아내야 합니다

---

## WSTG-ATHN-08 — 취약한 보안 질문 답변 테스트 (Testing for Weak Security Question Answer)

취약한 보안 질문 답변 테스트는 비밀번호 복구 또는 다요소 인증(Multi-Factor Authentication, MFA) 흐름 중에 사용자의 신원을 확인하는 데 사용되는 지식 기반 인증(Knowledge-Based Authentication, KBA) 메커니즘을 평가하는 것을 포함합니다. 이는 보안 질문이 공개 출처 정보(Open-Source Intelligence, OSINT) 또는 사회 공학(Social Engineering)을 통해 쉽게 발견될 수 있는 정적이고 비밀이 아닌 정보에 의존하여 무단 계정 탈취(Account Takeover)로 이어질 수 있기 때문에 중요합니다. 이러한 취약점은 일반적으로 사용자가 미리 설정된 질문에 대한 답변을 제공하는 "비밀번호 찾기" 또는 "계정 잠금 해제" 모듈에서 발생합니다. 공격자의 관점에서 이는 무차별 대입(Brute-forcing) 또는 표적 조사의 주요 대상입니다. 많은 사용자가 "어머니의 성함은?" 또는 "졸업한 고등학교는?"과 같은 일반적인 질문에 대해 예측 가능하거나 공개적으로 확인 가능한 답변을 제공하기 때문입니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *추가적인 확인 절차 없이 보안 질문이 전체 비밀번호 재설정 및 계정 탈취를 위한 유일한 요구 사항인 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**도구:** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### 애플리케이션이 본인 확인 또는 비밀번호 복구를 위해 보안 질문을 구현하고 있습니까?
- [ ] 아니요 — 보안 질문이 인증 또는 복구에 **사용되지 않음**  
- [ ] 예 — 보안 질문이 선택적인 2차 요소로 **사용됨**  
- [ ] 예 — 보안 질문이 기본/유일한 복구 메커니즘으로 **사용됨** *(높은 위험)*  

### 보안 질문이 고정된 목록에서 선택됩니까, 아니면 사용자 정의 방식입니까?
- [ ] 아니요 — 질문이 전적으로 사용자 정의이며 높은 엔트로피(Entropy)를 요구함  
- [ ] 예 — 질문이 고정되어 있지만 OSINT로 발견 불가능한 옵션을 포함함  
- [ ] 예 — 질문이 일반적이고 OSINT로 발견 가능한 주제의 고정 목록임 *(취약함)*  

### 보안 질문 답변 시도에 대해 속도 제한(Rate Limiting) 또는 계정 잠금이 적용됩니까?
- [ ] 예 — 무차별 대입을 방지하기 위해 엄격한 속도 제한 및 계정 잠금이 **적용됨**  
- [ ] 예 — 속도 제한이 **적용되나**, IP 교체 또는 헤더 조작을 통해 우회가 **가능함**  
- [ ] 아니요 — 답변 시도에 대해 **속도 제한이 적용되지 않음**  

### 애플리케이션이 답변 내용에 대해 복잡성 또는 검증을 강제합니까?
- [ ] 예 — 검증을 통해 일반적인 단어를 방지하고 답변의 복잡성/고유성을 보장함  
- [ ] 예 — 기본적인 검증은 존재하나 일반적인 단어 목록 또는 사전 공격(Dictionary Attack)으로 **우회할 수 있음**  
- [ ] 아니요 — **검증이 수행되지 않음**; 단순하거나 단일 문자인 답변이 **허용됨**  

### 논리적 결함을 통해 보안 질문 챌린지를 우회할 수 있습니까?
- [ ] 아니요 — 복구 프로세스를 진행하기 위해 반드시 유효한 답변이 필요함  
- [ ] 예 — HTTP 응답 코드 또는 세션 매개변수 조작을 통해 복구 프로세스를 **우회할 수 있음**  
- [ ] 예 — 답변이 페이지의 소스 코드나 숨겨진 필드(Hidden fields)에 반영되어 있음

---

## WSTG-ATHN-09 — 취약한 비밀번호 변경 또는 초기화 기능 테스트

비밀번호 변경 및 초기화 메커니즘(Password change and reset mechanisms)은 인증 관리의 핵심 구성 요소로, 부적절하게 구현될 경우 공격자가 사용자 계정을 탈취할 수 있게 합니다. 이러한 취약점은 일반적으로 예측 가능한 초기화 토큰, 브루트 포스(Brute Force) 공격 시 계정 잠금 부재, 안전하지 않은 토큰 전달, 또는 현재 비밀번호를 확인하지 않고 비밀번호를 변경할 수 있는 기능 등과 관련이 있습니다. 공격자는 복구 링크를 가로채거나 추측하고, 호스트 헤더 인젝션(Host Header Injection)을 수행하여 초기화 이메일을 리다이렉트하거나, 세션 고정(Session Fixation)을 활용해 비밀번호 변경 후에도 액세스 권한을 유지하는 방식으로 이러한 결함을 악용합니다. 이러한 프로세스가 강력한 검증을 요구하고 암호학적 무작위성(Cryptographic randomness)을 활용하도록 보장하는 것은 계정 무결성을 유지하고 무단 액세스를 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-09 |
| **CWE** | CWE-640 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 높음(High) / 치명적(Critical) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/09-Testing_for_Weak_Password_Change_or_Reset_Functionalities  
* https://hacktricks.wiki/en/pentesting-web/reset-password.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `Python (Custom Scripts)`, `CyberChef`, `OAST (Interactsh/Burp Collaborator)`

### 표준 변경 중에 새 비밀번호를 설정하기 위해 현재 비밀번호가 요구됩니까?
- [ ] 예 — 현재 비밀번호가 **요구되며** 검증됨  
- [ ] 예 — 현재 비밀번호가 **요구되나** 파라미터 오염(Parameter Pollution) 또는 조작을 통해 우회 가능함  
- [ ] 아니요 — 현재 비밀번호가 **요청되지 않아** 세션 하이재킹(Session Hijacking)을 통한 계정 탈취가 가능함 *(High)*  

### 비밀번호 초기화 토큰이 암호학적으로 안전하고 예측 불가능합니까?
- [ ] 예 — 토큰이 높은 엔트로피(Entropy), 무작위성을 가지며 **예측 불가능함**  
- [ ] 예 — 토큰이 생성되지만 낮은 엔트로피를 보이거나 예측 가능한 패턴(예: 타임스탬프 기반)이 있음  
- [ ] 아니요 — 토큰이 **예측 가능하거나** Base64 인코딩된 이메일/ID와 같은 정적 사용자 데이터를 기반으로 함 *(Critical)*  

### 비밀번호 초기화 링크가 호스트 헤더 인젝션을 통해 리다이렉트될 수 있습니까?
- [ ] 아니요 — 애플리케이션이 링크 생성을 위해 `Host` 헤더를 무시하거나 검증함  
- [ ] 예 — 애플리케이션이 링크를 구성하기 위해 `Host` 또는 `X-Forwarded-Host` 헤더를 사용하지만, 검증이 **존재함**  
- [ ] 예 — 헤더 조작을 통해 공격자가 제어하는 도메인으로 링크를 **리다이렉트할 수 있음** *(High)*  

### 초기화 토큰이 제한된 수명을 가지며 즉시 무효화됩니까?
- [ ] 예 — 토큰이 빠르게 만료되며 사용 또는 비밀번호 변경 직후 **즉시** 무효화됨  
- [ ] 예 — 토큰이 긴 기간(예: 24시간 이상) 후에 만료되거나 **여러 번 사용**을 허용함  
- [ ] 아니요 — 토큰이 **만료되지 않거나** 비밀번호가 성공적으로 변경된 후에도 유효함  

### 비밀번호 초기화 및 변경 엔드포인트에 속도 제한(Rate Limiting) 또는 잠금 설정이 있습니까?
- [ ] 예 — 열거(Enumeration) 및 브루트 포스를 방지하기 위해 엄격한 속도 제한 및 CAPTCHA가 **적용됨**  
- [ ] 예 — 제한적인 제어가 존재하지만 IP 로테이션 또는 헤더 스푸핑(Header Spoofing)을 통해 우회가 **가능함**  
- [ ] 아니요 — 속도 제한이 **적용되지 않아** 대량의 계정 열거 또는 토큰 브루트 포싱이 가능함

---

## WSTG-ATHN-10 — 대체 채널의 취약한 인증 테스트 (Testing for Weaker Authentication in Alternative Channel)

대체 채널의 취약한 인증 테스트(Testing for weaker authentication in alternative channels)는 계정 액세스를 위한 보조 경로—모바일 API(Mobile API), 비밀번호 복구 워크플로(Password recovery workflows), 헬프 데스크 인터페이스(Help desk interfaces) 또는 레거시 서브도메인(Legacy subdomains) 등—를 식별하고 분석하는 작업을 포함합니다. 이러한 경로는 기본 웹 포털만큼의 보안 엄격성을 갖추지 못할 수 있습니다. 이러한 대체 채널은 종종 강력한 다요소 인증(Multi-factor authentication, MFA), 엄격한 전송 속도 제한(Rate Limiting) 또는 복잡한 비밀번호 요구 사항이 결여되어 전체 인증 시스템을 약화시키는 "가장 취약한 고리"가 됩니다. 공격자는 특히 이러한 간과된 진입점을 표적으로 삼아 크리덴셜 스터핑(Credential Stuffing)을 수행하거나, 서로 다른 인터페이스가 신원 확인을 처리하는 방식의 차이를 악용하여 MFA를 우회(Bypass)합니다. 모든 인증 표면에서 보안 동등성(Security parity)을 보장하는 것은 무단 액세스를 방지하고 사용자 세션 및 데이터의 무결성을 유지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-10 |
| **CWE** | CWE-287 |
| **테스트 상태** | 미실시 (Not Performed) |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/10-Testing_for_Weaker_Authentication_in_Alternative_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `Postman`, `cURL`, `MobSF`, `Ffuf`

### 대체 인증 채널이 존재하며 식별되었습니까?
- [ ] 아니오 — 단일 인증 채널만 존재함  
- [ ] 예 — 다중 채널이 식별됨 (예: 모바일 앱 API, 데스크톱 클라이언트, 레거시 포털, SOAP 서비스)  

### 대체 채널이 기본 채널과 동일한 수준의 보안을 강제합니까?
- [ ] 예 — 모든 채널이 **동일한** 비밀번호 복잡성 및 MFA 요구 사항을 강제함  
- [ ] 예 — 비밀번호 복잡성은 강제하지만, MFA가 **요구되지 않거나** 우회할 수 있음  
- [ ] 아니오 — 대체 채널이 **현저히 취약한** 인증 요구 사항을 가지고 있음  

### 전송 속도 제한 및 계정 잠금이 모든 채널에서 일관되게 적용됩니까?
- [ ] 예 — 전송 속도 제한 및 계정 잠금(Account lockout)이 모든 엔드포인트(Endpoint)에서 **일관되게 강제됨**  
- [ ] 예 — 통제 수단이 존재하지만 특정 채널(예: 모바일 API)에서 **우회가 가능함**  
- [ ] 아니오 — 보조 인증 경로에 전송 속도 제한 또는 계정 잠금이 **적용되지 않음**  

### 대체 채널로 전환하여 MFA를 우회할 수 있습니까?
- [ ] 아니오 — MFA가 필수이며 진입점에 관계없이 **우회할 수 없음**  
- [ ] 예 — 웹 포털에서는 MFA가 요구되지만 모바일 또는 레거시 API에서는 **강제되지 않음**  

### 비밀번호 복구 또는 "비밀번호 찾기" 흐름이 표준 로그인보다 취약합니까?
- [ ] 아니오 — 복구 흐름이 강력한 대역 외(Out-of-band) 인증을 사용하며 정보를 유출하지 않음  
- [ ] 예 — 복구 흐름이 쉽게 추측하거나 조사할 수 있는 취약한 보안 질문을 사용함  
- [ ] 예 — 복구 흐름이 열거(Enumeration) 공격에 취약하거나 표준 로그인과 **동일한 수준의** 증명을 요구하지 않음

---

## WSTG-ATHN-11 — 다요소 인증(Multi-Factor Authentication, MFA) 테스트

다요소 인증(MFA) 테스트는 기본 인증 정보가 유출된 경우에도 무단 액세스를 방지하기 위해 설계된 보조 보안 계층의 견고성을 평가합니다. 공격자는 주로 레거시 API(API) 버전, 모바일 백엔드 또는 비밀번호 재설정 워크플로와 같이 MFA가 일관되게 적용되지 않는 엔드포인트(Endpoint)를 식별하여 MFA를 우회하려고 시도합니다. 취약점 공격에는 서버 응답 조작, 수명이 짧은 일회용 비밀번호(OTP)에 대한 브루트 포스(Brute Force), 또는 사용자가 두 번째 요소를 제공하지 않고도 인증된 상태로 전환할 수 있도록 허용하는 레이스 컨디션(Race Condition) 및 세션 관리 결함을 악용하는 행위가 포함됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHN-11 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 높음 (High) / 치명적 (Critical) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/11-Testing_Multi-Factor_Authentication  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Repeater/Turbo Intruder)`, `Postman`, `Mitproxy`

### 모든 인증 포털에서 MFA가 일관되게 강제 적용됩니까?
- [ ] 예 — 모든 웹, 모바일 및 API 기반 로그인 시도에 MFA가 요구됩니다  
- [ ] 예 — 하지만 특정 엔드포인트(예: 레거시 `/api/v1/login` 또는 모바일 전용 포털)에서는 MFA가 **강제되지 않습니다**  
- [ ] 아니요 — 애플리케이션에 MFA가 **구현되지 않았습니다** *(치명적)*  

### URL 직접 탐색 또는 응답 조작을 통해 MFA 검증 단계를 우회할 수 있습니까?
- [ ] 아니요 — 직접 탐색 또는 파라미터 변조로 챌린지(Challenge)를 **우회할 수 없습니다**  
- [ ] 예 — MFA 챌린지를 완료하지 않고 내부 대시보드 URL로 직접 탐색이 **가능합니다**  
- [ ] 예 — 액세스 권한을 얻기 위해 서버 응답을 조작(예: `401 Unauthorized`를 `200 OK`로 변경)하는 것이 **가능합니다**  

### MFA 코드 검증 프로세스가 브루트 포스 공격으로부터 보호됩니까?
- [ ] 예 — 여러 번의 OTP 시도 실패 후 엄격한 속도 제한(Rate Limiting) 또는 계정 잠금이 **적용됩니다**  
- [ ] 예 — 속도 제한이 존재하지만 IP 로테이션 또는 헤더 조작을 통해 우회가 **가능합니다**  
- [ ] 아니요 — 속도 제한이 강제되지 않으며, 코드에 대한 자동화된 브루트 포스가 **가능합니다**  

### 애플리케이션이 MFA 전환 중에 보안 세션 상태를 유지합니까?
- [ ] 예 — 높은 권한의 세션 토큰은 MFA가 성공적으로 완료된 **후에만** 발행됩니다  
- [ ] 아니요 — MFA가 완료되기 **전에** 기능이 온전한 세션 쿠키가 발행되어 일부 기능에 대한 액세스를 허용합니다  
- [ ] 아니요 — 애플리케이션이 탈취 가능한 정적 식별자 또는 예측 가능한 세션 전환을 사용합니다  

### 대체 MFA 수단(SMS, 이메일, 백업 코드)이 취약점 공격에 노출되어 있습니까?
- [ ] 아니요 — 모든 수단이 안전하고 예측 불가능하며 시간 제한이 있는 값을 사용합니다  
- [ ] 예 — 백업 코드가 예측 가능하거나 열거(Enumerate) **가능합니다**  
- [ ] 예 — "코드 재전송" 기능을 악용하여 SMS/이메일 플러딩(Flooding)을 수행하거나 연락처 세부 정보의 일부를 노출할 수 있습니다

---

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

## WSTG-ATHZ-02 — 권한 부여 스키마 우회 테스트 (Testing for Bypassing Authorization Schema)

권한 부여 스키마 우회(Bypassing Authorization Schema)는 애플리케이션이 액세스 제어(Access Control)를 제대로 시행하지 못하여, 공격자가 의도된 권한을 벗어나 리소스나 기능에 접근할 수 있을 때 발생합니다. 이 취약점은 일반적으로 공격자가 동일한 등급의 다른 사용자 데이터에 접근하는 수평적 권한 상승(Horizontal Privilege Escalation)이나, 낮은 권한의 사용자가 관리자 권한을 획득하는 수직적 권한 상승(Vertical Privilege Escalation)으로 나타납니다. 공격자는 리소스 ID(Resource ID)와 같은 요청 파라미터를 조작하거나, 세션 토큰(Session Token)을 수정하고, `X-Original-URL`과 같은 HTTP 헤더(HTTP Header)를 활용하여 경로 기반 제한을 우회함으로써 이러한 결함을 악용합니다. 강력한 권한 부여를 보장하는 것은 데이터 기밀성을 유지하고 전체 시스템을 손상시킬 수 있는 무단 관리 작업을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-02 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음(High) / 심각(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/02-Testing_for_Bypassing_Authorization_Schema  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite (Autorize extension)`, `Burp Suite (Repeater/Intruder)`, `Postman`, `cURL`, `OWASP ZAP`

### 동일한 역할을 가진 사용자 간의 수평적 권한 상승이 차단되어 있습니까?
- [ ] 예 — 모든 리소스 요청에 권한 부여 검사가 **적용되며** 우회가 **불가능함**  
- [ ] 예 — 권한 부여 검사가 **적용되지만** IDOR 또는 파라미터 변조(Parameter Tampering)를 통해 우회 가능함  
- [ ] 아니요 — 사용자가 단순히 리소스 ID를 변경함으로써 다른 사용자의 데이터에 접근할 수 **있음** *(High)*  

### 낮은 권한의 사용자에 대한 수직적 권한 상승이 차단되어 있습니까?
- [ ] 아니요 — 애플리케이션에 단 하나의 권한 레벨만 존재함  
- [ ] 예 — 낮은 권한의 사용자가 관리 기능에 접근할 수 **없음**  
- [ ] 예 — 관리 기능이 UI에서는 숨겨져 있으나 직접 URL을 통해 접근할 수 **있음**  
- [ ] 아니요 — 낮은 권한의 사용자가 역할 또는 권한을 조작하여 관리자 작업을 수행할 수 **있음** *(Critical)*  

### HTTP 메소드 변조(HTTP Verb Tampering)를 사용하여 권한 부여를 우회할 수 있습니까?
- [ ] 아니요 — 사용된 HTTP 메소드에 관계없이 액세스 제어가 시행됨  
- [ ] 예 — 메소드를 변경(예: `GET`에서 `POST` 또는 `PUT`으로)하면 권한 부여 검사가 **우회됨**  

### 인증 없이 관리자 또는 제한된 엔드포인트(Endpoint)에 접근할 수 있습니까?
- [ ] 아니요 — 모든 민감한 엔드포인트에 유효한 세션과 적절한 권한이 필요함  
- [ ] 예 — 공격자가 직접 URL을 아는 경우 일부 관리자 엔드포인트에 접근 가능함  
- [ ] 예 — 민감한 기능에 대한 비인증 접근이 **가능함** *(Critical)*  

### HTTP 헤더를 통해 경로 기반 권한 부여 우회가 가능합니까?
- [ ] 아니요 — 애플리케이션이 헤더 기반 우회에 취약하지 **않음**  
- [ ] 예 — `X-Original-URL`, `X-Rewrite-URL`, 또는 `X-Forwarded-For`와 같은 헤더를 사용하여 액세스 제어를 **우회할 수 있음**

---

## WSTG-ATHZ-03 — 권한 상승(Privilege Escalation) 테스트

권한 상승 (Privilege Escalation)은 공격자가 취약점을 악용하여 더 높은 권한 수준이나 다른 ID를 가진 사용자에게 허용된 리소스 또는 기능에 액세스할 때 발생합니다. 수직적 권한 상승 (Vertical Escalation)에서는 일반 사용자가 관리자 기능에 액세스하려고 시도하는 반면, 수평적 권한 상승 (Horizontal Escalation)은 동일한 권한 수준을 가진 다른 사용자의 데이터에 액세스하는 것을 포함합니다. 이러한 결함은 일반적으로 제대로 구현되지 않은 접근 제어 목록 (ACL), 부적절한 직접 객체 참조 (IDOR), 또는 세션이나 ID 토큰 내의 파라미터 변조 (Parameter Tampering)에서 나타납니다. 공격자의 관점에서는 숫자 ID를 조작하거나, 쿠키 또는 JWT의 역할 기반 파라미터를 수정하거나, 서버 측 권한 검증(Authorization)이 결여된 숨겨진 API 엔드포인트(API Endpoint)를 직접 호출함으로써 이를 달성하는 경우가 많습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-03 |
| **CWE** | CWE-269 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/03-Testing_for_Privilege_Escalation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite`, `Authorize`, `AuthMatrix`, `Authz`, `Postman`, `Ffuf`

### 애플리케이션이 다중 권한 수준 또는 멀티 테넌시(Multi-tenancy)를 구현하고 있습니까?
- [ ] 아니요 — 애플리케이션이 단일 사용자 또는 단일 역할 시스템입니다.  
- [ ] 예 — 다중 역할 또는 테넌트가 정의되어 있으며 권한 검증 테스트가 필요합니다.  

### 수평적 권한 상승(동일 수준 액세스)이 가능합니까?
- [ ] 아니요 — 모든 리소스 식별자 및 세션 소유자에 대해 권한 검증이 적용됩니다.  
- [ ] 예 — IDOR 또는 파라미터 변조를 통해 다른 사용자의 데이터에 액세스할 수 있습니다.  
- [ ] 예 — 데이터 유출은 가능하지만 다른 사용자의 데이터 수정은 불가능합니다.  

### 수직적 권한 상승(낮은 권한에서 높은 권한으로)이 가능합니까?
- [ ] 아니요 — 관리 엔드포인트가 역할 기반 접근 제어(RBAC)를 엄격하게 적용합니다.  
- [ ] 예 — 직접 URL 접속을 통해 낮은 권한의 사용자가 관리 기능에 액세스할 수 있습니다.  
- [ ] 예 — 역할 관련 헤더, 쿠키 또는 JWT 클레임을 조작하여 관리 기능에 액세스할 수 있습니다.  

### 매스 어사인먼트(Mass Assignment) 또는 파라미터 오염(Parameter Pollution)을 통해 사용자 역할이나 권한을 수정할 수 있습니까?
- [ ] 아니요 — 역할 기반 필드는 엄격하게 읽기 전용이며 사용자가 수정할 수 없습니다.  
- [ ] 예 — 프로필 업데이트 또는 회원가입 시 숨겨진 파라미터(예: `role=admin`)를 포함하여 사용자 권한을 상승시킬 수 있습니다.  

### 모든 API 버전 및 HTTP 메서드에 대해 권한 검증이 일관되게 적용됩니까?
- [ ] 예 — 모든 버전 및 메서드에 대해 통제 항목이 일관되게 적용됩니다.  
- [ ] 아니요 — 레거시 API 버전(예: `/v1/`) 또는 특정 HTTP 메서드(예: `PUT`, `DELETE`, `PATCH`)에서 권한 검증을 우회할 수 있습니다.  
- [ ] 아니요 — 관리 기능이 UI에서는 숨겨져 있지만 API 수준에서는 모든 사용자에 대해 활성화되어 있습니다.

---

## WSTG-ATHZ-04 — 비인가 직접 객체 참조(Insecure Direct Object References, IDOR) 테스트

비인가 직접 객체 참조(Insecure Direct Object References, IDOR)는 애플리케이션이 사용자 제공 입력에 기반하여 객체에 대한 직접적인 접근을 허용하면서, 요청자의 권한을 확인하는 권한 부여(Authorization) 검토를 수행하지 않을 때 발생합니다. 이 취약점은 공격자가 다른 사용자 또는 시스템에 속한 데이터를 조회, 수정 또는 삭제할 수 있게 하므로 기밀성과 무결성에 중대한 영향을 미칩니다. 일반적으로 내부 데이터베이스 키, 파일 이름 또는 계정 식별자를 참조하는 URL 파라미터, POST 바디 데이터 또는 JSON 키에서 나타납니다. 공격자 관점에서 이 취약점의 익스플로잇(Exploit)은 정수 값을 증가시키거나 UUID를 퍼징(Fuzzing)하는 등 식별자를 조작하여 자신의 권한 범위를 벗어난 리소스에 접근하는 방식으로 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-04 |
| **CWE** | CWE-639 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/access-control  

**도구:** `Burp Suite (Autorize extension)`, `Caido`, `Postman`, `ffuf`, `Intruder`

### 애플리케이션이 요청 파라미터에서 직접 객체 식별자를 사용합니까?
- [ ] 아니오 — 애플리케이션이 간접 참조 또는 암호화된 맵을 사용함 *(가장 안전)*  
- [ ] 예 — 애플리케이션이 예측 불가능한 식별자(예: 긴 UUID/해시)를 사용함  
- [ ] 예 — 애플리케이션이 예측 가능한 식별자(예: 순차적 정수 또는 사용자 이름)를 사용함  

### 객체 식별자가 포함된 모든 요청에 대해 서버 측 권한 부여가 수행됩니까?
- [ ] 예 — 어떤 식별자에 대해서도 서버 측 검증을 우회할 수 없음  
- [ ] 예 — 서버 측 검증이 존재하지만 파라미터 오염(Parameter Pollution) 또는 헤더 조작을 통해 우회가 가능함  
- [ ] 아니오 — 요청된 객체의 소유권을 확인하기 위한 권한 부여 검증이 적용되지 않음  

### 공격자가 다른 사용자에게 속한 객체에 접근하거나 이를 수정할 수 있습니까? (수평적 IDOR)
- [ ] 아니오 — 다른 사용자의 데이터에 접근할 수 없음  
- [ ] 예 — 다른 사용자의 데이터를 조회할 수 있으나 수정은 불가능함  
- [ ] 예 — 다른 사용자의 데이터를 조회 및 수정할 수 있음 *(치명적)*  

### 관리자 또는 시스템 수준의 객체에 접근할 수 있습니까? (수직적 IDOR)
- [ ] 아니오 — 더 높은 권한의 객체에 접근할 수 없음  
- [ ] 예 — 관리 설정 또는 시스템 파일에 접근할 수 있음  

### 애플리케이션이 검색, 메타데이터 또는 기타 엔드포인트를 통해 객체 식별자를 유출합니까?
- [ ] 아니오 — 식별자가 의도치 않게 노출되지 않음  
- [ ] 예 — 보조 엔드포인트 또는 공개 프로필을 통해 다른 사용자/객체의 식별자를 열거할 수 있음

---

## WSTG-ATHZ-05 — OAuth 취약점 테스트 (Testing for OAuth Weaknesses)

OAuth 취약점은 OAuth 2.0 또는 OpenID Connect (OIDC) 프로토콜 구현 시 발생하는 다양한 결함을 포괄하며, 이는 흔히 완전한 계정 탈취 (Account Takeover)나 미인가 데이터 접근으로 이어집니다. 이러한 취약점은 일반적으로 인가 흐름 (Authorization Flow) 중에 나타나며, 특히 `redirect_uri`의 검증, `state` 파라미터의 엔트로피 (Entropy) 및 검증, 그리고 액세스 (Access) 또는 ID 토큰의 보안 처리에 관한 것입니다. 공격자는 인가 코드를 가로채거나, 오픈 리다이렉트 (Open Redirects)를 통해 공격자가 제어하는 도메인으로 토큰을 리다이렉트하거나, 사이트 간 요청 위조 (CSRF)를 수행하여 피해자의 세션을 공격자의 계정에 연결함으로써 이를 악용합니다. OAuth는 종종 주요 인증 메커니즘으로 사용되기 때문에, 단 하나의 설정 오류만으로도 전체 사용자 식별 시스템의 무결성이 손상될 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ATHZ-05 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) / 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/05-Testing_for_OAuth_Weaknesses  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/oauth  

**도구:** `Burp Suite (Repeater/Collaborator)`, `CyberChef`, `OIDC-Checker`, `OAuth-Scanner`

### CSRF 방지를 위해 `state` 파라미터가 구현 및 검증되었는가?
- [ ] 예 — `state`가 필수이며, 세션당 고유하고 서버에서 엄격하게 검증됨  
- [ ] 예 — `state`가 존재하지만 여러 세션에 걸쳐 고정되어 있거나 예측 가능함  
- [ ] 예 — `state`가 존재하지만 애플리케이션이 콜백 시 이를 검증하지 않음  
- [ ] 아니요 — 인가 요청에서 `state` 파라미터가 사용되지 않음 *(심각)*  

### `redirect_uri`가 화이트리스트 (Whitelist)를 기준으로 엄격하게 검증되는가?
- [ ] 예 — 미리 등록된 화이트리스트와 정확히 일치하는 경우에만 허용됨  
- [ ] 예 — 검증이 적용되지만 경로 탐색 (Path Traversal) 또는 서브도메인 조작을 통해 우회가 가능함  
- [ ] 예 — 검증이 적용되지만 파라미터 오염 (Parameter Pollution) 또는 프래그먼트 주입 (Fragment Injection)을 통해 우회가 가능함  
- [ ] 아니요 — 애플리케이션이 `redirect_uri` 파라미터에 임의의 URL을 허용함 *(심각)*  

### 인가 흐름을 변경하기 위해 `response_type`을 조작할 수 있는가?
- [ ] 아니요 — 애플리케이션이 의도된 흐름(예: `code`)만 수락하고 나머지는 거부함  
- [ ] 예 — `code`에서 `token` (암시적 흐름, Implicit flow)으로 전환이 가능하며 URL에서 토큰이 유출됨  
- [ ] 예 — 하이브리드 흐름(Hybrid flows) 또는 허가되지 않은 `response_type` 값이 활성화되어 처리됨  

### Referer 헤더 또는 브라우저 기록을 통해 액세스 토큰이나 인가 코드가 유출되는가?
- [ ] 아니요 — 토큰/코드가 안전하게 처리되며 민감한 페이지에서 적절한 `Referrer-Policy`를 사용함  
- [ ] 예 — `Referer` 헤더를 통해 제3자 도메인으로 토큰/코드가 유출됨  
- [ ] 예 — 안전하지 않은 캐싱 또는 GET 요청으로 인해 브라우저 기록에서 토큰/코드가 확인됨  

### 애플리케이션이 OAuth 스코프 (Scopes)에 대해 "최소 권한 (Least Privilege)" 원칙을 강제하는가?
- [ ] 예 — 애플리케이션이 기능 수행에 필요한 최소한의 스코프만 요청함  
- [ ] 아니요 — 애플리케이션이 기본적으로 과도하거나 관리자 권한의 스코프를 요청함  
- [ ] 아니요 — 사용자가 동의 단계에서 특정 스코프를 검토하거나 제외할 수 없음

---

## WSTG-SESS-01 — 세션 관리 스키마 테스트 (Testing for Session Management Schema)

세션 관리 스키마 테스트는 애플리케이션이 세션 식별자(Session Identifiers)의 생명 주기와 구조를 어떻게 처리하는지 분석하여, 예측 및 하이재킹(Hijacking) 시도에 대해 충분히 견고한지 확인하는 데 중점을 둡니다. 공격자는 통계적 분석을 수행하기 위해 여러 세션 토큰(Session Tokens)을 수집하며, 이를 통해 패턴, 낮은 엔트로피(Entropy), 또는 예측 가능한 증가치를 찾아 다른 사용자의 유효한 세션을 위조할 수 있습니다. 스키마의 취약점은 주로 세션 고정(Session Fixation) 취약점이나 충분히 무작위적이지 않은 값의 사용으로 나타나며, 이는 일반적으로 HTTP 쿠키, URL 파라미터 또는 사용자 정의 헤더 구현에서 발견됩니다. 세션 스키마를 탈취하는 것은 공격자가 피해자의 기본 인증 정보(Credentials) 없이도 인증된 상태에 완전히 접근할 수 있게 하는 영향력이 큰 공격입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-01 |
| **CWE** | CWE-330 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/01-Testing_for_Session_Management_Schema  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Sequencer)`, `OWASP ZAP`, `Python (pandas/numpy for entropy analysis)`, `Cookie Editor`

### 세션 식별자가 충분히 복잡하고 무작위적입니까?
- [ ] 예 — 토큰이 통계적 무작위성 테스트를 통과하며(높은 엔트로피), 우회가 **불가능합니다**  
- [ ] 예 — 토큰의 길이는 길지만 예측 가능한 패턴이나 정적인 세그먼트를 포함하고 있습니다  
- [ ] 아니요 — 토큰이 순차적이거나 예측 가능한 타임스탬프(Timestamps)를 기반으로 하며, 엔트로피가 **심각하게 낮습니다** *(Critical)*  

### 세션 식별자가 어디에서 전송되고 저장됩니까?
- [ ] 식별자가 `Secure` 및 `HttpOnly` 쿠키에 저장됩니다 *(가장 안전함)*  
- [ ] 식별자가 쿠키에 저장되지만 `Secure` 또는 `HttpOnly` 플래그가 누락되었습니다  
- [ ] 식별자가 URL 파라미터 또는 `GET` 요청을 통해서만 전송됩니다 *(높은 위험)*  

### 애플리케이션이 인증 성공 시 새로운 세션 식별자를 발급합니까?
- [ ] 예 — 로그인 직후 새로운 고유 세션 ID가 **생성됩니다**  
- [ ] 아니요 — 로그인 전후의 세션 ID가 동일하게 유지됩니다 *(세션 고정 가능)*  

### 재사용 방지를 위해 세션 식별자가 특정 IP 주소나 User-Agent에 결합되어 있습니까?
- [ ] 예 — 클라이언트의 핑거프린트(Fingerprint)가 크게 변경되면 세션이 무효화됩니다  
- [ ] 아니요 — 추가 검증 없이 서로 다른 IP 주소나 기기에서 세션을 재사용할 **수 있습니다**  

### 로그아웃 또는 타임아웃 후에 서버 측에서 세션 식별자가 적절히 무효화됩니까?
- [ ] 예 — 로그아웃 시 서버 측 세션 상태가 **완전히 파기됩니다**  
- [ ] 아니요 — 클라이언트 측 쿠키가 삭제되더라도 서버에서 세션이 유효하게 유지됩니다  
- [ ] 아니요 — 세션이 **만료되지 않거나** 타임아웃(Timeout) 기간이 지나치게 깁니다

---

## WSTG-SESS-02 — 쿠키 속성 테스트 (Testing for Cookies Attributes)

세션 관리(Session Management)는 종종 상태를 유지하기 위해 HTTP 쿠키(HTTP Cookies)에 의존하므로, 이러한 쿠키의 보안 속성(Security attributes)은 공격자의 주요 목표가 됩니다. `HttpOnly`, `Secure`, `SameSite`와 같은 속성을 누락하거나 잘못 설정할 경우, 애플리케이션은 교차 사이트 스크립팅(Cross-Site Scripting, XSS), 암호화되지 않은 채널을 통한 가로채기 또는 교차 사이트 요청 위조(Cross-Site Request Forgery, CSRF) 공격을 통해 세션 토큰이 탈취될 위험에 노출됩니다. 모의해킹 전문가(Pentesters)는 이러한 플래그를 검토하여 공격자가 브라우저의 문서 객체 모델(Document Object Model, DOM)에서 세션 식별자를 유출하거나 브라우저가 교차 출처 요청(Cross-origin requests) 시 세션 식별자를 강제로 전송하게 할 수 있는지 확인합니다. 적절한 속성 설정은 민감한 토큰이 승인된 보안 컨텍스트 내에만 유지되도록 보장하는 기본적인 심층 방어(Defense-in-depth) 조치입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### 쿠키 플래그 구현 분석 (Analysis of Cookie Flag Implementation)
| 속성 | 적용됨 | 누락됨 |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### 민감한 세션 쿠키에 `HttpOnly` 플래그가 적용되어 있습니까?
- [ ] 예 — **모든** 민감한 세션 쿠키에 적용됨 *(가장 안전)*  
- [ ] 예 — 일부 세션 쿠키에만 적용됨  
- [ ] 아니요 — 플래그가 **적용되지 않아**, 클라이언트 측 스크립트를 통한 접근이 허용됨 *(치명적)*  

### HTTPS를 통해 전송되는 쿠키에 `Secure` 플래그가 강제 적용됩니까?
- [ ] 예 — 전송 계층 보안(TLS)을 통해 전송되는 **모든** 쿠키에 적용됨  
- [ ] 아니요 — 플래그가 **적용되지 않아**, 평문 HTTP를 통해 쿠키가 전송될 수 있음  

### CSRF 위험을 완화하기 위해 `SameSite` 속성이 사용됩니까?
- [ ] 예 — 세션 쿠키에 대해 `Strict` 또는 `Lax`로 설정됨  
- [ ] 예 — `None`으로 설정되었으나 `Secure` 플래그가 존재함  
- [ ] 아니요 — 속성이 **누락**되었거나 `Secure` 플래그 **없이** `None`으로 설정됨  

### `Domain` 및 `Path` 속성이 필요한 최소 범위로 제한되어 있습니까?
- [ ] 예 — **특정** 서브도메인 및 애플리케이션 경로로 범위가 제한됨  
- [ ] 아니요 — 범위가 지나치게 **넓게** 설정되어 (예: 상위 도메인 또는 루트 `/` 설정), 공격 표면이 증가함  

### `Expires` 또는 `Max-Age` 속성을 통해 민감한 세션 쿠키가 적절히 만료됩니까?
- [ ] 예 — 적절한 만료 날짜가 설정됨  
- [ ] 아니요 — 쿠키 수명이 과도하게 **길게** 설정되어 지속됨  
- [ ] 아니요 — 쿠키가 세션 전용이지만 서버 측 타임아웃 강제가 **누락**됨

---

## WSTG-SESS-03 — 세션 고정 (Session Fixation)

세션 고정(Session Fixation)은 사용자가 성공적으로 인증된 후에도 애플리케이션이 세션 식별자(Session Identifier)를 무효화하거나 갱신(Rotate)하지 못할 때 발생하며, 공격자가 피해자에게 알려진 세션 토큰(Session Token)을 강제로 사용하게 할 수 있습니다. 애플리케이션이 로그인 후에도 인증 전의 세션 ID를 유지하는 경우, 공격자는 특정 세션 ID가 포함된 특수하게 제작된 링크를 피해자에게 제공하고 이후 인증된 세션을 탈취(Hijack)할 수 있습니다. 이 취약점은 주로 로그인 양식이나 URL 파라미터 및 쿠키를 통해 수락되는 세션 식별자에서 나타납니다. 공격자 관점에서 이 익스플로잇(Exploit)은 악성 링크나 헤더 주입(Header Injection) 취약점을 통해 세션을 "고정"하고 피해자가 자격 증명(Credentials)을 입력하기를 기다리는 방식이며, 이는 실제 토큰을 훔칠 필요 없이 인증을 우회(Bypass)할 수 있게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-03 |
| **CWE** | CWE-384 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/03-Testing_for_Session_Fixation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Proxy/Repeater)`, `OWASP ZAP`, `EditThisCookie`, `Curl`

### 성공적인 인증 후 세션 식별자가 변경됩니까?
- [ ] 예 — 새로운 세션 식별자가 **발급되며** 기존 식별자는 무효화됩니다 *(가장 안전)*  
- [ ] 예 — 새로운 세션 식별자가 **발급되지만** 기존 식별자가 **유효하게 유지됩니다**  
- [ ] 아니요 — 인증 전후에 세션 식별자가 **동일하게 유지됩니다** *(치명적)*  

### 애플리케이션이 URL 파라미터를 통해 제공된 세션 식별자를 수락합니까?
- [ ] 아니요 — 세션 ID는 **쿠키를 통해서만** 관리됩니다  
- [ ] 예 — URL에서 세션 ID를 수락하지만 로그인 시 **무시되거나 갱신**됩니다  
- [ ] 예 — URL의 세션 ID가 **수락되며** 인증 후에도 **지속됩니다**  

### 공격자가 정의한 세션 ID를 애플리케이션에 강제할 수 있습니까?
- [ ] 아니요 — 애플리케이션은 사용자가 제공한 유효하지 않거나 존재하지 않는 세션 ID를 **거부합니다**  
- [ ] 예 — 애플리케이션은 쿠키에 제공된 임의의 ID를 사용하여 세션을 **수락하고 초기화합니다**  
- [ ] 예 — 애플리케이션은 URL 파라미터에 제공된 임의의 ID를 사용하여 세션을 **수락하고 초기화합니다**  

### 인증 전 세션이 올바르게 종료됩니까?
- [ ] 예 — 로그인 이벤트 발생 시 익명 세션이 **완전히 파괴됩니다**  
- [ ] 아니요 — 익명 세션이 **파괴되지 않아** 잠재적인 세션 하베스팅(Session Harvesting)이 가능합니다  

### 세션 쿠키가 클라이언트 측 주입으로부터 보호됩니까?
- [ ] 예 — 스크립트 기반의 고정을 방지하기 위해 `HttpOnly` 및 `Secure` 플래그가 **적용되어 있습니다**  
- [ ] 아니요 — 플래그가 **적용되지 않아** 크로스 사이트 스크립팅(Cross-Site Scripting, XSS)을 통해 세션 ID를 설정할 수 있습니다

---

## WSTG-SESS-04 — 노출된 세션 변수 테스트 (Testing for Exposed Session Variables)

노출된 세션 변수는 민감한 세션 식별자(Session Identifier)나 상태 관련 데이터가 URL 쿼리 스트링(Query String), Referer 헤더 또는 시스템 로그와 같은 비보안 채널을 통해 전송될 때 발생합니다. 이러한 노출은 식별자가 브라우저 기록에 캐시되거나 중간 프록시에 로깅될 수 있으며, Referer 헤더를 통해 제3자 사이트로 유출될 수 있기 때문에 세션 하이재킹(Session Hijacking)의 위험을 크게 증가시킵니다. 모의해킹 전문가(Pentesters)는 모든 아웃바운드 요청을 모니터링하고 애플리케이션 로그나 서버 설정을 검사하여 의도치 않은 데이터 유출이 있는지 확인함으로써 이러한 노출을 식별합니다. 실제 상황에서 공격자는 공용 PC에서 유효한 세션 토큰(Session Token)을 수집하거나 트래픽 패턴을 분석하여 인증을 우회하고 사용자 계정에 대한 무단 액세스 권한을 획득함으로써 이러한 유출을 악용합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-04 |
| **CWE** | CWE-598 |
| **테스트 상태** | 수행되지 않음 |
| **위험도(Severity)** | 중간 / 높음* |

> *노출된 변수가 즉각적인 계정 탈취를 허용하는 세션 토큰인 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/04-Testing_for_Exposed_Session_Variables  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Proxy/Logger)`, `OWASP ZAP`, `Browser DevTools`, `grep`

### 세션 식별자가 URL 쿼리 스트링으로 전송됩니까?
- [ ] 아니요 — URL 쿼리 스트링에 세션 식별자가 존재하지 않습니다.
- [ ] 예 — 식별자가 존재하지만 세션의 수명이 짧거나 위험도가 낮습니다.
- [ ] 예 — 고유한 세션 ID가 URL 쿼리 스트링에 존재합니다 *(Critical)*.

### 애플리케이션이 Referer 헤더를 통해 세션 토큰을 유출합니까?
- [ ] 아니요 — `Referrer-Policy`가 유출을 방지하거나 제3자 링크가 존재하지 않습니다.
- [ ] 예 — 세션 토큰이 Referer 헤더를 통해 제3자 도메인으로 전송됩니다.

### 세션 변수가 서버 측 또는 애플리케이션 로그에 저장됩니까?
- [ ] 아니요 — 세션 변수가 마스킹 처리되거나 로그에서 제외됩니다.
- [ ] 예 — 세션 변수가 애플리케이션 또는 웹 서버 로그에 평문(Plain Text)으로 기록됩니다.

### 애플리케이션이 상태 변경 작업에 GET 요청을 사용합니까?
- [ ] 아니요 — 애플리케이션이 민감한 작업에 `POST` 또는 기타 비멱등(Non-idempotent) 메서드를 사용합니다.
- [ ] 예 — `GET` 메서드가 사용되어 세션 데이터가 브라우저 기록 및 중간 프록시에 캐시됩니다.

### 세션 관련 데이터의 캐싱을 방지하도록 `Cache-Control` 헤더가 설정되어 있습니까?
- [ ] 예 — 민감한 응답에 `Cache-Control: no-store`(또는 이와 유사한 설정)가 적용되어 있습니다.
- [ ] 예 — 제어 항목이 적용되어 있으나 브라우저 예외 사례를 통해 우회가 가능합니다.
- [ ] 아니요 — 세션 데이터를 포함하는 민감한 응답이 브라우저에 의해 캐시될 수 있습니다.

---

## WSTG-SESS-05 — 사이트 간 요청 위조(Cross-Site Request Forgery) 테스트

사이트 간 요청 위조(Cross-Site Request Forgery, CSRF)는 공격자가 피해자의 브라우저를 속여, 피해자가 현재 인증된 상태인 다른 웹사이트에서 원치 않는 작업을 수행하도록 유도하는 취약점입니다. 이 공격 방식은 세션 쿠키(Session cookies)나 권한 부여(Authorization) 헤더와 같은 "주변 자격 증명(Ambient credentials)"을 나가는 요청에 자동으로 포함시키는 브라우저의 동작 방식을 악용합니다. 공격자는 일반적으로 제3자 사이트에 악성 스크립트나 숨겨진 폼(Form)을 호스팅하여 비밀번호 변경, 이메일 주소 업데이트 또는 금융 송금 실행과 같은 상태 변경 작업(State-changing operations)을 표적으로 삼습니다. 성공적인 공격은 사용자의 인지나 동의 없이 완전한 계정 탈취(Account takeover) 또는 승인되지 않은 데이터 수정으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-05 |
| **CWE** | CWE-352 |
| **테스트 상태** | 수행되지 않음 |
| **위험도 (Severity)** | 중간(Medium) / 높음(High)* |

> *취약한 동작이 계정 탈취, 권한 상승(Privilege escalation) 또는 승인되지 않은 금융 거래를 허용할 경우 위험도는 '높음'으로 분류됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/05-Testing_for_Cross_Site_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/csrf-cross-site-request-forgery.html  
* https://portswigger.net/web-security/csrf  

**도구:** `Burp Suite Professional (CSRF PoC Generator)`, `OWASP ZAP`, `CSRFTester`, `python3 (SimpleHTTPServer for PoC hosting)`

### 상태 변경 요청에 대해 Anti-CSRF 토큰이 구현되어 있습니까?
- [ ] 예 — 모든 상태 변경 작업에 고유하고 암호학적으로 강력한 토큰이 요구됨  
- [ ] 예 — 토큰이 존재하지만 세션별로 고유하지 않거나 예측 가능함  
- [ ] 아니요 — Anti-CSRF 토큰이 구현되지 않음  

### Anti-CSRF 토큰에 대한 서버 측 검증(Server-side validation)이 견고합니까?
- [ ] 예 — 서버가 토큰을 엄격하게 검증하며 우회(Bypass)가 불가능함  
- [ ] 예 — 검증이 수행되지만 토큰 파라미터를 제거함으로써 우회 가능함  
- [ ] 예 — 검증이 수행되지만 빈 값이나 더미(Dummy) 토큰을 제공하여 우회 가능함  
- [ ] 예 — 검증이 수행되지만 토큰이 사용자의 세션과 연동되어 있지 않음  

### 애플리케이션이 쉽게 우회 가능한 CSRF 방어 방식에 의존하고 있습니까?
- [ ] 아니요 — 애플리케이션이 약한 헤더나 Origin 체크에만 의존하지 않음  
- [ ] 예 — 방어 기재가 스푸핑(Spoofing)되거나 제거될 수 있는 `Referer` 또는 `Origin` 헤더에만 의존함  
- [ ] 예 — `X-Requested-With` 헤더 확인에 의존하며, 이는 CORS 설정 오류(CORS misconfigurations)를 통해 우회 가능함  

### 세션 쿠키에 `SameSite` 속성이 설정되어 있습니까?
- [ ] 예 — 모든 세션 관련 쿠키에 `SameSite` 속성이 `Strict` 또는 `Lax`로 설정됨  
- [ ] 아니요 — `SameSite` 속성이 누락되어 브라우저별 기본 동작에 의존함  
- [ ] 아니요 — `SameSite`가 `None`으로 명시적으로 설정되어 있으며, `Secure` 플래그나 추가적인 완화 조치가 없음  

### HTTP 요청 메서드를 변경하여 CSRF 방어를 우회할 수 있습니까?
- [ ] 아니요 — 사용된 HTTP 메서드에 관계없이 방어가 강제됨  
- [ ] 예 — 토큰이 `POST` 요청에서만 검증되지만, `GET`을 통해 해당 작업을 수행할 수 있음  
- [ ] 예 — 메서드 변경(예: `PUT` 또는 `DELETE`로 변경)을 통해 토큰 확인을 우회할 수 있음

---

## WSTG-SESS-06 — 로그아웃 기능 테스트(Testing for Logout Functionality)

로그아웃 기능(Logout functionality)은 사용자의 세션을 종료하고 클라이언트와 서버 양측에서 연결된 세션 식별자(Session Identifier)를 무효화하도록 설계된 중요한 보안 통제 항목입니다. 로그아웃을 제대로 구현하지 못하면 세션 고정(Session Fixation) 또는 세션 하이재킹(Session Hijacking) 공격이 발생할 수 있습니다. 사용자가 "로그아웃"한 후 머신에 접근한 공격자가 여전히 활성 상태인 세션 토큰(Session Token)을 재사용할 수 있기 때문입니다. 모의 침투 테스터(Pentester)는 브라우저에서 세션 쿠키(Session Cookie)가 삭제되는지, 서버 측 세션 상태가 명시적으로 파괴되는지, 그리고 뒤로 가기 버튼 탐색을 통해 캐시된 민감한 데이터에 접근할 수 있는지 확인하여 이를 평가합니다. 안전한 로그아웃 구현은 해당 동작이 트리거된 후, 이전 세션 토큰을 사용한 어떠한 후속 요청도 애플리케이션에서 승인되지 않도록 보장합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-06 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 보통(Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/06-Testing_for_Logout_Functionality  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite (Repeater/Proxy)`, `Browser Developer Tools`, `OWASP ZAP`

### 기능적인 로그아웃 메커니즘이 존재하며 접근 가능한가?
- [ ] 예 — 로그아웃 버튼이 존재하며 세션 종료 요청을 트리거함  
- [ ] 아니요 — 로그아웃 버튼이 **없거나** 종료 이벤트를 **트리거하지 않음**  

### 서버 측에서 세션 식별자가 무효화되는가?
- [ ] 예 — 서버가 이전 세션 토큰을 사용한 모든 후속 요청을 거부함  
- [ ] 아니요 — 서버가 로그아웃 후에도 이전 세션 토큰을 **계속 수락함**  

### 로그아웃 시 애플리케이션이 브라우저의 세션 쿠키를 삭제하는가?
- [ ] 예 — 쿠키가 만료된 날짜 또는 빈 값으로 덮어쓰여짐  
- [ ] 예 — 쿠키는 남아 있으나 서버에서 **더 이상 유효하지 않음**  
- [ ] 아니요 — 쿠키가 브라우저에 남아 있으며 원래 값을 **유지함**  

### 로그아웃 후 브라우저의 '뒤로 가기' 버튼을 통해 인증된 민감한 콘텐츠에 접근할 수 있는가?
- [ ] 아니요 — `Cache-Control: no-store` 또는 유사한 헤더가 로그아웃 후 민감한 페이지의 열람을 방지함  
- [ ] 예 — 민감한 페이지가 캐시되어 세션이 종료된 후 탐색을 통해 열람 **가능함**

---

## WSTG-SESS-07 — 세션 타임아웃 테스트 (Testing Session Timeout)

세션 타임아웃 테스트(Session timeout testing)는 애플리케이션이 미리 정의된 비활성 기간 또는 총 지속 시간 이후에 사용자의 세션을 효과적으로 종료하는지 평가합니다. 이 통제 항목은 특히 공용 워크스테이션이나 공격자가 네트워크 가로채기 또는 교차 사이트 스크립팅(XSS)을 통해 세션 식별자(Session identifier)를 탈취하는 시나리오에서 세션 하이재킹(Session Hijacking) 위험을 완화하는 데 필수적입니다. 모의해킹 전문가(Pentester)는 일정 기간 비활성 상태일 때 발생하는 유휴 타임아웃(Idle timeout)과 활동 여부와 관계없이 세션의 전체 수명을 제한하는 절대 타임아웃(Absolute timeout)을 모두 분석합니다. 공격자의 관점에서 무기한 또는 지나치게 긴 세션은 비인가 접근을 유지하고 재인증(Re-authentication)의 필요성을 우회할 수 있는 연장된 기회의 창을 제공합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-07 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/07-Testing_Session_Timeout  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Browser DevTools`

### 애플리케이션에 유휴 세션 타임아웃(Idle session timeout)이 구현되어 있습니까?
- [ ] 예 — 일정 기간(예: 15-30분) 비활성 후 세션이 만료됨  
- [ ] 예 — 세션이 만료되지만 타임아웃 기간이 지나치게 김 (예: 24시간 초과)  
- [ ] 아니요 — 비활성 중에도 세션이 무기한 활성 상태로 유지됨  

### 세션 타임아웃이 서버 측(Server-side)에서 강제됩니까?
- [ ] 예 — 클라이언트 측 상태와 관계없이 서버가 만료된 토큰의 요청을 거부함  
- [ ] 아니요 — 타임아웃이 클라이언트 측 자바스크립트(JavaScript) 또는 meta-refresh를 통해서만 강제됨  

### 애플리케이션에 절대 세션 타임아웃(Absolute session timeout)이 구현되어 있습니까?
- [ ] 예 — 활동 여부와 관계없이 고정된 최대 지속 시간 이후 세션이 종료됨  
- [ ] 아니요 — 지속적인 활동이 있는 한 세션이 무기한 연장될 수 있음  

### 타임아웃 시 서버에서 세션 식별자가 무효화됩니까?
- [ ] 예 — 타임아웃 임계값에 도달하면 세션 토큰을 재사용할 수 없음  
- [ ] 아니요 — 클라이언트가 로그인 페이지로 리다이렉트된 후에도 서버에서 세션 토큰이 여전히 유효함  

### 자동화된 하트비트(Heartbeat) 요청을 사용하여 세션을 무기한 유지할 수 있습니까?
- [ ] 아니요 — 절대 타임아웃 또는 보조 통제 항목이 무한한 세션 연장을 방지함  
- [ ] 예 — 주기적인 백그라운드 요청(예: AJAX)이 세션을 무기한 유지할 수 있음

---

## WSTG-SESS-08 — Session Puzzling

세션 퍼즐링(Session Puzzling)은 세션 변수 오버로딩(Session Variable Overloading)으로도 알려져 있으며, 애플리케이션이 서로 다른 모듈에서 동일한 세션 변수를 여러 용도로 사용하거나 워크플로우 전환 중에 세션 상태를 적절히 초기화하지 않을 때 발생합니다. 공격자는 인증되지 않은 프로필 미리보기나 다단계 등록 과정과 같은 특정 컨텍스트(Context)에서 세션 변수를 채운 다음, 애플리케이션이 해당 변수를 잘못 신뢰하는 보호된 영역으로 이동하여 이 동작을 악용합니다. 이는 애플리케이션을 속여 세션이 실제보다 더 높은 권한 상태에 있다고 믿게 만듦으로써 인증 우회(Authentication Bypass), 권한 상승(Privilege Escalation) 또는 민감한 데이터에 대한 무단 접근과 같은 심각한 영향을 초래할 수 있습니다. 이 취약점은 세션 관리(Session Management) 로직이 서로 다른 기능적 구성 요소에 일관성 없이 적용되는 복잡한 상태 유지 애플리케이션에서 주로 발생합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-08 |
| **CWE** | CWE-621 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 높음 (High) / 치명적 (Critical) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/08-Testing_for_Session_Puzzling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Comparator)`, `OWASP ZAP`, `Postman`, `Cookie Editor`

### 애플리케이션이 서로 다른 모듈에서 동일한 세션 변수 이름을 다른 용도로 사용합니까?
- [ ] 아니요 — 변수 이름이 고유하거나 범위(Scope)가 격리되어 있음  
- [ ] 예 — 공유 변수 이름이 존재하지만 모듈 간에 상태가 초기화됨  
- [ ] 예 — 공유 변수 이름이 존재하며 상태가 **초기화되지 않음**  

### 인증되지 않은 동작이 인증된 컨텍스트에서 사용되는 세션 변수에 영향을 미칠 수 있습니까?
- [ ] 아니요 — 세션 변수는 성공적인 인증 **이후에만** 초기화됨  
- [ ] 예 — 로그인 전에 세션 변수를 설정할 수 있으나 로그인 후 상태에는 영향을 미치지 **않음**  
- [ ] 예 — 인증 전 단계에서 설정된 세션 변수가 인증 후에도 신뢰됨 *(Critical)*  

### 권한 수준이 변경될 때 애플리케이션이 세션 식별자 및 관련 변수를 다시 초기화합니까?
- [ ] 예 — 세션이 완전히 재설정되고 새로운 ID가 **발급됨**  
- [ ] 부분적 — 새로운 ID가 발급되지만 일부 세션 변수가 **유지됨**  
- [ ] 아니요 — 세션 ID와 모든 변수가 변경되지 않고 **유지됨**  

### 권한이 없는 계정으로 세션 변수를 조작하여 관리자 권한을 획득할 수 있습니까?
- [ ] 아니요 — 사용자가 권한 관련 변수를 수정할 수 **없음**  
- [ ] 예 — 변수를 수정할 수 있지만 애플리케이션이 백엔드 데이터베이스를 통해 이를 **검증함**  
- [ ] 예 — 변수를 수정할 수 있으며 애플리케이션이 세션 상태를 암묵적으로 **신뢰함** *(Critical)*  

### 특정 워크플로우(예: 비밀번호 재설정, 결제) 완료 즉시 세션 상태가 초기화됩니까?
- [ ] 예 — 워크플로우 완료 시 관련 세션 변수가 파괴됨  
- [ ] 아니요 — 워크플로우 변수가 **유지되어** 애플리케이션의 다른 영역에서 재사용될 수 있음

---

## WSTG-SESS-09 — 세션 하이재킹(Session Hijacking) 테스트

세션 하이재킹(Session Hijacking)은 공격자가 유효한 세션 식별자(Session Identifier, SID)를 탈취, 예측 또는 고정하여 사용자의 활성 세션에 대한 무단 액세스 권한을 획득할 때 발생합니다. 이 취약점은 일반적으로 불충분한 전송 계층 보안, 쿠키 보안 플래그 부재, 또는 공격자가 인증을 완전히 우회할 수 있도록 하는 예측 가능한 SID 생성 알고리즘을 통해 나타납니다. 공격자의 관점에서 성공적인 익스플로잇(Exploit)은 자격 증명 없이 피해자를 완전히 사칭할 수 있게 하며, 이는 주로 네트워크 스니핑(Network Sniffing), 교차 사이트 스크립팅(Cross-Site Scripting, XSS) 또는 세션 고정(Session Fixation) 공격을 통해 이루어집니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-09 |
| **CWE** | CWE-287 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 치명적 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/09-Testing_for_Session_Hijacking  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**도구:** `Burp Suite`, `OWASP ZAP`, `EditThisCookie`, `Wireshark`, `Bettercap`

### 전송 중에 세션 식별자가 보호됩니까?
- [ ] 예 — `Secure` 플래그가 **활성화**되어 있으며 HSTS가 엄격하게 적용됨  
- [ ] 예 — `Secure` 플래그가 **활성화**되어 있으나 HSTS가 **비활성화**됨  
- [ ] 아니요 — `Secure` 플래그가 **적용되지 않아**, 암호화되지 않은 채널을 통한 SID 가로채기가 가능함 *(높음)*  

### 세션 식별자가 클라이언트 측 스크립트 액세스로부터 보호됩니까?
- [ ] 예 — 모든 세션 관련 쿠키에 `HttpOnly` 플래그가 **적용됨**  
- [ ] 아니요 — `HttpOnly` 플래그가 **적용되지 않아**, XSS를 통한 SID 유출이 가능함 *(치명적)*  

### 애플리케이션이 세션을 클라이언트 속성에 바인딩(Binding)합니까?
- [ ] 예 — 세션이 클라이언트 속성(IP/User-Agent)에 바인딩되어 있으며 재사용이 **불가능함**  
- [ ] 예 — 세션 바인딩이 존재하지만 헤더 스푸핑(Header Spoofing)을 통해 우회가 **가능함**  
- [ ] 아니요 — 세션 바인딩이 존재하지 않음; SID가 어떤 소스에서 사용되더라도 **유효함**  

### 세션 식별자가 예측을 방지할 수 있을 만큼 충분히 무작위입니까?
- [ ] 예 — SID가 암호학적으로 안전한 의사 난수 생성기(Cryptographically Secure Pseudo-Random Number Generator, CSPRNG)를 사용함  
- [ ] 아니요 — SID 길이가 불충분하거나 **예측 가능한** 시퀀스/패턴을 따름  

### 공격 노출 시간을 제한하기 위해 동시 세션(Concurrent Sessions)이 관리됩니까?
- [ ] 아니요 — 애플리케이션이 사용자당 단일 활성 세션을 엄격하게 강제함  
- [ ] 예 — 다중 동시 세션이 **활성화**되어 있으나 모니터링됨  
- [ ] 예 — 서로 다른 기기/위치에서 무제한 동시 세션이 **가능함**

---

## WSTG-SESS-10 — JSON 웹 토큰(JWT) 테스트

JSON 웹 토큰 (JSON Web Tokens, JWT)은 무상태 세션 관리(Stateless session management) 및 ID 전파(Identity propagation)를 위해 흔히 사용되지만, 그 보안성은 서명 알고리즘(Signing algorithms)의 올바른 구현과 엄격한 클레임 검증(Claim verification)에 전적으로 의존합니다. 공격자들은 "none" 알고리즘 결함을 악용하거나, 취약한 HS256 비밀키를 브루트포스(Brute-force)하거나, 비대칭 공개키(Asymmetric public key)를 대칭 HMAC 비밀키(Symmetric HMAC secret)로 사용하는 알고리즘 전환 공격(Algorithm-switching attacks)을 통해 인증 우회를 자주 시도합니다. 이러한 취약점은 주로 세션 쿠키나 인증(Authorization) 헤더에서 나타나며, 공격자가 토큰의 암호학적 무결성(Cryptographic integrity)을 손상시키지 않으면서 `role` 또는 `user_id`와 같은 클레임을 수정하여 권한을 상승(Escalate privileges)시키거나 다른 사용자를 사칭(Impersonate)할 수 있게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-10 |
| **CWE** | CWE-347 |
| **테스트 상태** | Not Performed |
| **심각도** | High / Critical |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/10-Testing_JSON_Web_Tokens  
* https://hacktricks.wiki/en/pentesting-web/hacking-jwt-json-web-tokens.html  
* https://portswigger.net/web-security/jwt  

**도구:** `jwt_tool`, `Burp Suite (JWT Editor Extension)`, `hashcat`, `john`, `jose`

### 서버가 "none" 알고리즘을 허용합니까?
- [ ] 아니요 — 서버가 헤더에서 `alg: none`을 사용하는 토큰을 **거부함**  
- [ ] 예 — 서버가 `alg: none` 및 빈 서명이 포함된 토큰을 **허용함** *(Critical)*  

### JWT 서명이 어떻게 검증되며 브루트포스 공격으로부터 보호됩니까?
- [ ] 예 — 강력한 비대칭 키(RS256/ES256) 또는 높은 엔트로피(Entropy)를 가진 HMAC 비밀키가 사용됨  
- [ ] 예 — HS256이 사용되지만 낮은 엔트로피 또는 기본값으로 인해 비밀키가 **브루트포스될 수 있음**  
- [ ] 아니요 — 서명 검증이 **적용되지 않거나** 알고리즘 전환(RS256에서 HS256으로)을 통해 **우회될 수 있음**  

### 백엔드에서 표준 클레임(exp, nbf, iat)이 엄격하게 강제됩니까?
- [ ] 예 — `exp`(만료)가 존재하며 우회할 수 **없음**  
- [ ] 예 — `exp`가 존재하지만 서버가 검증 중에 이를 강제하지 **않음**  
- [ ] 아니요 — 만료 클레임이 **누락되었거나** 무시되어 무기한 세션 재전송(Replay)이 가능함  

### 구현 시 헤더 기반 키 주입(kid, jku, x5u)을 방지합니까?
- [ ] 예 — 헤더가 살균(Sanitize)되며 오직 신뢰할 수 있는 내부 키/소스만 **허용됨**  
- [ ] 아니요 — `kid`(Key ID) 헤더가 디렉토리 탐색(Directory traversal) 또는 SQL Injection에 취약하여 알려진 파일을 가리킬 수 있음  
- [ ] 아니요 — `jku` 또는 `x5u` 헤더가 공격자가 제어하는 URL을 가리켜 악성 키를 로드할 수 **있음**

---

## WSTG-SESS-11 — 동시 세션 테스트 (Testing for Concurrent Sessions)

동시 세션 테스트는 애플리케이션이 단일 사용자 계정에 대해 서로 다른 브라우저, 장치 또는 IP 주소에서 여러 개의 동시 활성 세션(Concurrent Sessions)을 유지할 수 있도록 허용하는지 여부를 평가합니다. 동시 세션을 제한하지 못하면 공격자가 탈취한 세션 토큰(Session Tokens)이나 유출된 자격 증명(Compromised Credentials)을 사용하여 기존 세션을 종료하거나 정당한 사용자에게 알리지 않고도 공격을 수행할 수 있는 기회의 창이 넓어집니다. 이러한 동작은 일반적으로 여러 소스에서 인증을 시도하고 세션 안정성을 모니터링하여 평가하며, 이는 종종 세션 관리 로직의 취약성이나 보안 중심의 비즈니스 규칙 부재를 드러냅니다. 공격자의 관점에서 동시성 제어(Concurrency Control)의 부재는 초기 침해 후 은밀하게 지속성(Persistence)을 유지할 수 있게 하며 병렬 자동화 공격을 용이하게 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-SESS-11 |
| **CWE** | CWE-613 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 낮음 (Low) / 중간 (Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/11-Testing_for_Concurrent_Sessions  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Containers)`, `Firefox Multi-Account Containers`, `Postman`, `cURL`

### 애플리케이션 정책에 의해 동시 세션이 제한됩니까?
- [ ] 예 — 사용자당 단 하나의 세션만 **허용됩니다**. 새로운 로그인은 기존 로그인을 무효화합니다 *(가장 안전함)*  
- [ ] 예 — 고정된 수의 동시 세션이 **강제 적용되며** 이를 초과할 수 없습니다  
- [ ] 아니요 — 제한 없는 수의 동시 세션이 **가능합니다**  

### 세션 제한에 도달했을 때 애플리케이션은 어떻게 응답합니까?
- [ ] 가장 오래된 활성 세션이 **자동으로 종료됩니다** (세션 고정(Session Fixation)/하이재킹(Hijacking) 완화)  
- [ ] 새로운 세션이 승인되기 전에 사용자에게 기존 세션을 종료하도록 **요청합니다**  
- [ ] 다른 장치에서 수동 로그아웃이 발생할 때까지 새로운 로그인 시도가 **차단됩니다**  
- [ ] 아무런 조치도 취하지 않습니다 — 여러 세션이 동시에 **활성화된** 상태로 유지됩니다  

### 애플리케이션이 사용자를 위한 세션 관리 인터페이스를 제공합니까?
- [ ] 예 — 사용자가 활성 세션(IP, 장치, 시간)을 확인하고 원격으로 **종료할 수 있습니다**  
- [ ] 예 — 사용자가 활성 세션을 확인할 수 있지만 원격으로 **종료할 수는 없습니다**  
- [ ] 아니요 — 사용자는 자신의 계정과 연결된 다른 활성 세션을 확인하거나 관리할 수 **없습니다**  

### 동시 로그인 활동이 감지될 때 사용자에게 알림이 전송됩니까?
- [ ] 예 — 새로운 장치나 위치에서의 로그인에 대해 알림(이메일, SMS 또는 앱 내 알림)이 **트리거됩니다**  
- [ ] 아니요 — 동시 세션이 수립될 때 아무런 알림도 **제공되지 않습니다**

---

## WSTG-INPV-01 — 반사형 크로스 사이트 스크립팅 (Reflected Cross Site Scripting, XSS)

반사형 크로스 사이트 스크립팅(Reflected Cross Site Scripting, XSS)은 애플리케이션이 충분한 검증이나 인코딩 없이 신뢰할 수 없는 데이터를 HTTP 응답에 포함시켜, 페이로드(Payload)가 피해자의 브라우저 컨텍스트 내에서 실행될 때 발생합니다. 공격자는 사회공학(Social Engineering) 기법을 통해 특수하게 제작된 URL이나 양식을 전달하며, 이를 통해 사용자 세션(User Session) 탈취, 민감한 쿠키(Cookie) 유출 또는 사용자를 대신한 무단 행위를 수행합니다. 이 취약점은 주로 검색 매개변수, 에러 메시지 및 입력 값을 사용자 인터페이스로 직접 반사하는 모든 엔드포인트(Endpoint)에서 발견됩니다. 공격 성공 여부는 피해자가 악성 링크와 상호작용하는지에 달려 있으며, 이는 비지속적이지만 특정 관리자나 인증된 사용자를 타겟팅하는 데 매우 효과적인 공격 벡터입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-01 |
| **CWE** | CWE-79 |
| **테스트 상태** | Not Performed |
| **심각도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/01-Testing_for_Reflected_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/cross-site-scripting  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `XSStrike`, `KXS`, `dalfox`

### 사용자 입력 값이 응답 본문에 반사됩니까?
- [ ] 아니요 — 입력 값이 사용자에게 절대 반사되지 않음  
- [ ] 예 — 입력 값이 반사되지만 적절히 인코딩되어 우회가 불가능함  
- [ ] 예 — 입력 값이 인코딩이나 필터링 없이 그대로 반사됨  

### 컨텍스트 인식 출력 인코딩(Context-aware output encoding)이 구현되어 있습니까?
- [ ] 예 — 특정 반사 컨텍스트에 따라 적절한 인코딩(HTML, 속성, 자바스크립트)이 적용됨  
- [ ] 예 — 인코딩이 적용되었으나 특정 컨텍스트(예: `<script>` 태그 내 HTML 인코딩)에 대해 불충분함  
- [ ] 아니요 — 인코딩이 적용되지 않음  

### 입력 값 검증 또는 웹 애플리케이션 방화벽(Web Application Firewall, WAF) 필터를 우회할 수 있습니까?
- [ ] 아니요 — 필터가 모든 일반적인 페이로드 및 난독화된 XSS 페이로드를 효과적으로 차단함  
- [ ] 예 — 필터가 존재하지만 문자 인코딩, 비표준 태그 또는 폴리글랏(Polyglots)을 사용하여 우회가 가능함  
- [ ] 아니요 — 필터나 검증 메커니즘이 전혀 존재하지 않음  

### 반사된 입력 값의 실행 컨텍스트는 무엇입니까?
- [ ] 안전함 — 입력 값이 실행 불가능한 위치(예: 일반적인 `<div>` 또는 `<span>` 태그 내)에 반사됨  
- [ ] 위험함 — 입력 값이 HTML 속성 내부 또는 태그의 `src`/`href` 내부에 반사됨  
- [ ] 심각함 — 입력 값이 `<script>` 블록, 이벤트 핸들러 또는 템플릿 리터럴 내부에 직접 반사됨  

### XSS 영향을 완화하기 위한 현대적인 브라우저 보안 헤더가 존재합니까?
- [ ] 예 — `Content-Security-Policy` (CSP)가 엄격한 script-src와 함께 활성화되어 있음  
- [ ] 예 — `Content-Security-Policy` (CSP)가 활성화되어 있으나 `unsafe-inline` 또는 취약한 화이트리스트를 포함하고 있음  
- [ ] 아니요 — CSP 또는 기존의 `X-XSS-Protection` 헤더가 존재하지 않음

---

## WSTG-INPV-02 — 저장형 크로스 사이트 스크립팅 (Stored Cross Site Scripting, XSS)

저장형 크로스 사이트 스크립팅 (Stored Cross Site Scripting, XSS)은 지속성 XSS (Persistent XSS)라고도 하며, 애플리케이션이 사용자로부터 데이터를 수신하여 적절한 검증 (Validation) 또는 인코딩 (Encoding) 없이 영구적인 데이터베이스, 파일 시스템 또는 기타 저장 매체에 저장할 때 발생합니다. 이후 희생자가 이 정화되지 않은 데이터를 검색하여 표시하는 페이지로 이동하면, 애플리케이션의 오리진 (Origin) 하에 브라우저 컨텍스트 (Browser context) 내에서 악성 스크립트가 실행됩니다. 이 취약점은 희생자가 특수하게 조작된 링크를 클릭할 필요가 없기 때문에 특히 위험합니다. 해당 페이지를 열람하는 모든 사용자가 공격 대상이 되며, 이는 세션 하이재킹 (Session hijacking), 무단 행위 또는 자격 증명 수집 (Credential harvesting)으로 이어질 수 있습니다. 공격자는 주로 댓글 섹션, 사용자 프로필, 메시지 게시판, 그리고 입력 값이 다른 사용자나 관리자에게 일관되게 렌더링되는 관리자 로그를 목표로 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-02 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/02-Testing_for_Stored_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/cross-site-scripting  

**도구:** `Burp Suite`, `XSStrike`, `BeEF`, `KXSStrike`, `Sleepy Puppy`

### 다른 사용자에게 나중에 표시하기 위해 사용자 입력을 저장하는 진입점이 존재합니까?
- [ ] 아니요 — 애플리케이션이 추후 렌더링을 위해 사용자 제어 입력을 저장하지 않음  
- [ ] 예 — 입력이 저장되지만 (예: 프로필, 댓글), HTML 컨텍스트 내에서 렌더링되지 않음  
- [ ] 예 — 입력이 저장되며 다른 사용자나 관리자에게 렌더링됨  

### 저장된 데이터가 렌더링될 때 출력 인코딩 또는 정화 (Sanitization)가 적용됩니까?
- [ ] 예 — 컨텍스트 인지 출력 인코딩이 **적용되었으며** 우회 (Bypass)가 **불가능함** *(가장 안전)*  
- [ ] 예 — 정화 (예: `DOMPurify`)가 **적용되었으나** 설정 오류를 통해 우회가 **가능함**  
- [ ] 아니요 — 인코딩이나 정화 **없이** 데이터가 DOM에 직접 렌더링됨 *(높음)*  

### 대체 페이로드(Payload)를 사용하여 입력 값 검증 또는 정화를 우회할 수 있습니까?
- [ ] 아니요 — 필터가 다양한 인코딩, 비표준 태그 및 이벤트 핸들러를 안전하게 처리함  
- [ ] 예 — HTML 엔티티 인코딩 또는 특정 태그 (예: `<svg>`, `<img>`)를 사용하여 우회가 **가능함**  
- [ ] 예 — 폴리글로트 페이로드 (Polyglot payloads) 또는 변이 기반 XSS (mXSS)를 통해 우회가 **가능함**  

### 콘텐츠 보안 정책 (Content Security Policy, CSP)이 보조 방어 계층을 제공합니까?
- [ ] 예 — 엄격한 CSP가 **활성화되어 있으며** 인라인 스크립트 및 승인되지 않은 소스의 실행을 방지함  
- [ ] 예 — CSP가 **활성화되어 있으나** 잘못 설정됨 (예: `unsafe-inline` 또는 광범위한 `script-src`)  
- [ ] 아니요 — 애플리케이션에 CSP가 **활성화되지 않음**  

### "저장형 XSS를 통한 RCE" 또는 기타 고영향 체인 (블라인드 XSS)이 가능합니까?
- [ ] 아니요 — 영향이 클라이언트 측 브라우저 컨텍스트로 제한됨  
- [ ] 예 — 저장형 XSS가 내부 패널의 관리자를 대상으로 하는 블라인드 XSS (Blind XSS)에 사용될 수 **있음**  
- [ ] 예 — 저장형 XSS가 다른 취약점 (예: CSRF, 민감한 데이터 유출)과 연결될 수 **있음**

---

## WSTG-INPV-03 — HTTP 동사 변조(HTTP Verb Tampering) 테스트

HTTP 동사 변조(HTTP Verb Tampering)는 사용된 HTTP 메서드(HTTP Method)를 기반으로 특정 리소스에 대한 액세스 권한을 부여하는 웹 서버 및 애플리케이션 프레임워크의 취약점을 악용합니다. 공격자는 `GET` 또는 `POST`와 같은 표준 메서드 대신 `HEAD`, `PUT`, `OPTIONS` 또는 백엔드에서 일관되지 않게 처리될 수 있는 임의의 비표준 문자열로 대체하여 보안 제약을 우회하려고 시도합니다. 이 취약점은 일반적으로 Java EE web.xml 필터나 .NET 권한 부여 규칙과 같은 보안 설정에서 허용된 메서드만 명시적으로 나열하고 다른 메서드를 고려하지 못했거나, "모두 거부(deny-all)" 접근 방식 대신 "메서드별 거부(deny-by-method)" 방식을 사용했을 때 발생합니다. 성공적인 공격은 관리 기능에 대한 무단 액세스, 데이터 수정 또는 서버의 내부 구성에 관한 정보 노출로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-03 |
| **CWE** | CWE-288 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *인증 없이 관리 기능 수행 또는 데이터 수정(`PUT`/`DELETE`)이 가능한 경우 위험도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `curl`, `nmap`, `ffuf`

### 애플리케이션이 비표준 또는 임의의 HTTP 메서드에 응답합니까?
- [ ] 아니요 — 애플리케이션이 명시적으로 필요한 메서드를 제외한 모든 메서드를 거부합니다.
- [ ] 예 — 애플리케이션이 표준 메서드(OPTIONS, TRACE)에 응답하지만 보안을 우회할 수는 **없습니다**.
- [ ] 예 — 애플리케이션이 임의의 문자열(예: FOO, TEST)을 허용하고 이를 `GET` 또는 `POST` 요청으로 처리합니다.

### HTTP 메서드를 변경하여 인증/권한 부여를 우회할 수 있습니까?
- [ ] 아니요 — HTTP 동사에 관계없이 보안 제어가 전역적으로 적용됩니다.
- [ ] 예 — 제어 장치가 마련되어 있으나 `HEAD` 메서드를 사용하여 우회가 **가능합니다**.
- [ ] 예 — 대체 메서드에 보안 제어가 **적용되지 않아**, 제한된 엔드포인트에 대한 무단 액세스가 허용됩니다.

### 서버에 `PUT` 또는 `DELETE`와 같은 위험한 메서드가 활성화되어 있습니까?
- [ ] 아니요 — 위험한 메서드가 **비활성화**되어 있거나 405 Method Not Allowed를 반환합니다.
- [ ] 예 — 메서드가 활성화되어 있지만 유효한 고권한 인증이 필요합니다.
- [ ] 예 — 메서드가 **활성화**되어 있으며 무단 파일 업로드 또는 리소스 삭제가 가능합니다 *(심각)*.

### `OPTIONS` 메서드가 허용된 동사에 대한 민감한 정보를 노출합니까?
- [ ] 아니요 — `OPTIONS`가 **비활성화**되어 있거나 일반적인 응답을 반환합니다.
- [ ] 예 — `OPTIONS`가 `Allow` 헤더를 반환하지만 제한된 내부 메서드는 노출하지 않습니다.
- [ ] 예 — `OPTIONS`가 추가 공격에 도움이 되는 내부 전용 또는 관리 메서드를 노출합니다.

### `TRACE` 메서드가 활성화되어 있어 교차 사이트 트레이싱(Cross-Site Tracing, XST)이 가능합니까?
- [ ] 아니요 — `TRACE` 및 `TRACK` 메서드가 **비활성화**되어 있습니다.
- [ ] 예 — `TRACE`가 **활성화**되어 있으며 민감한 쿠키/토큰을 포함한 HTTP 헤더를 그대로 반환합니다.

---

## WSTG-INPV-04 — HTTP 매개변수 오염(HTTP Parameter Pollution, HPP) 테스트

HTTP 매개변수 오염(HTTP Parameter Pollution, HPP)은 애플리케이션이 동일한 이름의 여러 HTTP 매개변수를 수신하고 이를 일관되지 않거나 안전하지 않은 방식으로 처리할 때 발생합니다. 공격자는 중복된 매개변수를 제공함으로써 애플리케이션의 내부 로직을 조작하여 웹 애플리케이션 방화벽(Web Application Firewall, WAF), 입력값 검증 필터 또는 접근 제어 메커니즘을 우회할 수 있습니다. 이 취약점은 기반 웹 서버 또는 애플리케이션 프레임워크에 크게 의존하며, 프레임워크는 반복된 매개변수 중 첫 번째, 마지막 또는 연결된 버전을 선택할 수 있습니다. 공격은 일반적으로 매개변수를 백엔드(Back-end) API, 데이터베이스 쿼리 또는 URL 리다이렉션 메커니즘으로 전달하는 엔드포인트(Endpoints)를 대상으로 하며, 교차 사이트 스크립팅(Cross-Site Scripting, XSS)에서 무단 데이터 유출에 이르는 영향을 미칩니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-04 |
| **CWE** | CWE-235 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `Arjun`, `HPP Finder`, `Wfuzz`

### 애플리케이션이 동일한 이름의 여러 HTTP 매개변수를 어떻게 처리합니까?
- [ ] 아니요 — 중복된 매개변수는 서버에 의해 거부되거나 무시됩니다.
- [ ] 예 — 서버가 일관되게 첫 번째 또는 마지막 항목만 선택합니다.
- [ ] 예 — 서버가 값을 연결(concatenate)하며, 이는 잠재적인 인젝션(Injection)을 허용할 수 있습니다.

### HPP를 활용하여 WAF 또는 입력 필터와 같은 보안 통제를 우회할 수 있습니까?
- [ ] 아니요 — 보안 통제가 매개변수의 모든 항목을 올바르게 검사합니다.
- [ ] 예 — WAF 또는 필터가 첫 번째 항목만 검사하여, 악성 두 번째 항목이 통과될 수 있습니다.
- [ ] 예 — 연결 방식을 통해 공격자가 탐지를 피하기 위해 여러 매개변수에 걸쳐 페이로드(Payload)를 분할할 수 있습니다.

### 애플리케이션이 적절한 새니타이제이션(Sanitization) 없이 응답에 오염된 매개변수를 반영합니까?
- [ ] 아니요 — 매개변수가 UI에 반영되기 전에 새니타이제이션되거나 인코딩됩니다.
- [ ] 예 — 오염으로 인해 콘텐츠가 반영되지만, 새니타이제이션이 적용됩니다.
- [ ] 예 — 오염으로 인해 콘텐츠가 반영되고 새니타이제이션이 적용되지 않아, XSS 또는 로직 오류가 발생합니다.

### 내부 API 호출 또는 백엔드 시스템으로 전달되는 매개변수가 오염에 취약합니까?
- [ ] 아니요 — 백엔드 요청이 안전하고 비동적인 구성 방식을 사용합니다.
- [ ] 예 — HPP를 통해 공격자가 백엔드 요청 구조에서 매개변수를 주입하거나 덮어쓸 수 있습니다.

### 애플리케이션이 GET 요청과 POST 요청에서 매개변수가 오염되었을 때 서로 다른 동작을 보입니까?
- [ ] 아니요 — 모든 HTTP 메서드에서 동작이 일관됩니다.
- [ ] 예 — GET 매개변수만 오염에 취약합니다.
- [ ] 예 — POST(본문) 매개변수만 오염에 취약합니다.

---

## WSTG-INPV-05 — SQL 삽입 (SQL Injection)

SQL 삽입(SQL Injection)은 사용자 입력값이 적절한 매개변수화(Parameterization) 또는 필터링(Sanitization) 없이 SQL 쿼리에 포함되어 공격자가 쿼리 로직을 조작할 수 있을 때 발생합니다. 공격에 성공할 경우 무단 데이터 유출(Data Exfiltration), 인증 우회(Authentication Bypass), 데이터 수정이 가능하며, 일부 경우에는 `xp_cmdshell`이나 `LOAD_FILE()`과 같은 데이터베이스 기능을 통해 원격 코드 실행(Remote Code Execution, RCE)으로 이어질 수 있습니다. 이 취약점은 일반적으로 로그인 양식, 검색 기능, REST API 매개변수, HTTP 헤더 및 사용자 제어 입력을 통해 동적 SQL 쿼리를 생성하는 모든 엔드포인트(Endpoint)에서 발생합니다. 공격자 관점에서 취약점 지점(Injection Point)을 식별하는 것은 데이터베이스 전체 장악 및 인프라 내부에서의 측면 이동(Lateral Movement)을 위한 주요 관문이 됩니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-05 |
| **CWE** | CWE-89 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html  
* https://portswigger.net/web-security/sql-injection  
* https://portswigger.net/web-security/nosql-injection  

**도구:** `sqlmap`, `Burp Suite (Intruder/Repeater)`, `Ghauri`, `jSQL Injection`, `BBQSQL`

### 사용자 제어 매개변수가 안전한 데이터베이스 액세스 방식을 통해 처리됩니까?
- [ ] 예 — 애플리케이션이 ORM 또는 매개변수화된 쿼리(Parameterized queries)만을 사용하며 우회가 **불가능함**  
- [ ] 예 — 매개변수화된 쿼리가 사용되지만, 예외적인 경우(예: `ORDER BY` 절)를 통해 우회가 **가능함**  
- [ ] 아니요 — 매개변수화 **없이** 원시 문자열 연결(Raw string concatenation)을 사용하여 SQL 쿼리를 구성함  

### SQL 삽입을 통해 인증 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 로그인 매개변수에 삽입 취약점이 **없음**  
- [ ] 예 — 타우톨로지(Tautology) 기반 페이로드(Payload)(예: `' OR 1=1 --`)를 사용하여 인증 우회가 **가능함**  

### 인밴드(In-band) 또는 아웃오브밴드(Out-of-band) 기술을 통한 데이터 유출이 가능합니까?
- [ ] 아니요 — 식별된 데이터 유출 경로가 없음  
- [ ] 예 — UNION 기반 또는 오류 기반(Error-based) 유출이 **가능함**  
- [ ] 예 — 블라인드(Blind)(불리언 또는 시간 기반) 유출이 **가능함**  
- [ ] 예 — 아웃오브밴드(DNS/HTTP) 유출이 **가능함**  

### 애플리케이션 기능에서 2차 SQL 삽입(Second-order SQL Injection) 취약점이 나타납니까?
- [ ] 아니요 — 저장된 데이터를 이후 쿼리에서 호출할 때 안전하게 처리함  
- [ ] 예 — 사용자 입력이 저장된 후, 검증 **없이** 안전하지 않은 SQL 쿼리에 나중에 사용됨  

### 데이터베이스 서비스 계정 권한이 필요한 최소한으로 제한되어 있습니까?
- [ ] 예 — 서비스 계정이 **최소 권한**(예: 특정 테이블/작업으로 제한됨)을 보유함  
- [ ] 아니요 — 서비스 계정이 높은 권한을 보유함 (예: `DROP TABLE`, `FILE` 권한 또는 `xp_cmdshell`이 **활성화됨**)

---

## WSTG-INPV-06 — LDAP 인젝션(LDAP Injection) 테스트

LDAP 인젝션(LDAP Injection)은 애플리케이션이 적절한 정리(Sanitization)나 이스케이프(Escaping) 처리 없이 사용자 제공 데이터를 LDAP(Lightweight Directory Access Protocol) 필터에 포함할 때 발생하며, 이를 통해 공격자가 쿼리 로직을 조작할 수 있게 합니다. 별표(*), 괄호, 논리 연산자와 같은 특수 문자를 주입함으로써, 공격자는 검색 필터를 수정하여 인증 메커니즘을 우회하거나 사용자 이름, 그룹 멤버십, 조직 속성을 포함한 민감한 디렉토리 정보를 유출할 수 있습니다. 이 취약점은 주로 Active Directory 또는 OpenLDAP과 연동되는 기업 디렉토리 검색, 직원 포털, 또는 싱글 사인온(SSO, Single Sign-On) 시스템과 같은 기능에서 나타납니다. 공격자의 관점에서, 직접적인 쿼리 결과 출력이 애플리케이션에 의해 제한될 경우, 성공적인 익스플로잇(Exploit)을 위해 디렉토리 구조나 속성 값을 비트 단위로 열거하는 블라인드(Blind) 기법이 자주 사용됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-06 |
| **CWE** | CWE-90 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/06-Testing_for_LDAP_Injection  
* https://hacktricks.wiki/en/pentesting-web/ldap-injection.html  

**도구:** `Burp Suite (Intruder/Repeater)`, `ldapsearch`, `JNDIExploit`, `Wfuzz`, `nmap`

### 애플리케이션이 인증 또는 디렉토리 검색을 위해 LDAP을 사용합니까?
- [ ] 아니요 — 애플리케이션 아키텍처에서 LDAP이 사용되지 않음  
- [ ] 예 — LDAP이 사용되며 모든 입력이 엄격하게 정리되거나 매개변수화됨  
- [ ] 예 — LDAP이 사용되며 사용자 입력이 적절한 이스케이프 처리 없이 필터에 결합됨  

### 사용자 입력에서 LDAP 메타 문자가 적절히 이스케이프되거나 필터링됩니까?
- [ ] 예 — `(`, `)`, `&`, `|`, `*`, `\`와 같은 문자가 허용되지 않거나 올바르게 이스케이프됨  
- [ ] 아니요 — 메타 문자를 주입하여 LDAP 필터 구조를 변경할 수 있음  

### 인젝션을 통해 인증 메커니즘을 우회할 수 있습니까?
- [ ] 아니요 — 로그인 로직이 쿼리 조작에 취약하지 않음  
- [ ] 예 — 사용자 이름 또는 비밀번호 필드에서 논리 OR(`|`) 또는 와일드카드(`*`) 인젝션을 사용하여 인증을 우회할 수 있음  

### 디렉토리 서비스에서 블라인드 데이터 유출이 가능합니까?
- [ ] 아니요 — 검색 결과가 제한적이며 시간차 또는 불리언(Boolean) 차이가 관찰되지 않음  
- [ ] 예 — 불리언 기반 블라인드 인젝션(Boolean-based blind injection) 기법을 통해 디렉토리 속성을 비트 단위로 열거할 수 있음  
- [ ] 예 — 쿼리 조작으로 인해 전체 디렉토리 레코드가 애플리케이션 응답에 반영됨  

### 보조 애플리케이션 구성 요소(예: 메일러, SSO)가 LDAP 기반 속성 주입에 취약합니까?
- [ ] 아니요 — 모든 구성 요소에서 LDAP 속성이 안전하게 처리됨  
- [ ] 예 — 공격자가 인젝션을 통해 자신의 속성(예: 이메일, 그룹 멤버십)을 수정하여 권한을 상승시키거나 통신을 리다이렉트할 수 있음

---

## WSTG-INPV-07 — XML 인젝션 (XML Injection)

XML 인젝션(XML Injection)은 애플리케이션이 XML 메타 문자에 대한 적절한 검증, 정제(Sanitization) 또는 이스케이프(Escaping) 처리 없이 사용자 제공 데이터를 XML 문서나 스트림에 포함할 때 발생합니다. 공격자는 특정 XML 태그나 구조적 요소를 주입함으로써 문서의 로직을 조작하거나, 인증 메커니즘을 우회하고, 백엔드에서 처리되는 데이터의 무결성을 방해할 수 있습니다. 이 취약점은 SOAP 기반 웹 서비스, XML을 수용하는 REST API, SAML 어설션(SAML assertions), 그리고 XML 설정 파일이나 보고서를 동적으로 생성하는 애플리케이션에서 흔히 발견됩니다. 공격자의 관점에서 목표는 의도된 데이터 컨텍스트를 벗어나 문서 구조를 변경하여, 잠재적인 권한 상승(Privilege Escalation)이나 의도하지 않은 비즈니스 로직을 실행하는 것입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**도구:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### 애플리케이션이 사용자로부터 XML 형식의 입력을 수락하고 처리합니까?
- [ ] 아니오 — 애플리케이션이 XML 입력을 처리하지 않음  
- [ ] 예 — API (SOAP/REST)를 통해 XML 입력이 처리됨  
- [ ] 예 — 파일 업로드(SVG, DOCX 등)를 통해 XML이 처리됨  

### XML 메타 문자(예: `<`, `>`, `&`, `'`, `"`)가 적절히 이스케이프 또는 정제 처리됩니까?
- [ ] 예 — 모든 메타 문자가 **적절히** 이스케이프 또는 정제됨 *(가장 안전)*  
- [ ] 예 — 일부 필터링이 적용되나 인코딩을 사용하여 **우회가 가능함**  
- [ ] 아니오 — 정제 **없이** 원본 입력이 XML 구조에 직접 포함됨  

### 모든 XML 입력에 대해 서버 측 스키마 검증(XSD/DTD)이 강제됩니까?
- [ ] 예 — 엄격한 스키마 검증이 **활성화**되어 있으며 구조적 변경을 방지함  
- [ ] 예 — 검증이 **활성화**되어 있으나 데이터 필드 내의 태그 주입을 방지하지 **못함**  
- [ ] 아니오 — 스키마 검증이 **비활성화**되어 있거나 구현되지 않음  

### 공격자가 새로운 XML 태그를 주입하여 문서 구조나 로직을 변경할 수 있습니까?
- [ ] 아니오 — 입력에 관계없이 구조가 온전하게 유지됨  
- [ ] 예 — 태그 주입은 가능하나 비즈니스 로직을 변경할 수는 **없음**  
- [ ] 예 — 태그 주입이 **가능하며** 로직 우회 또는 데이터 수정이 가능함 *(심각)*  

### CDATA 섹션 또는 XML 주석을 사용하여 XML 파서(Parser)를 조작할 수 있습니까?
- [ ] 아니오 — 파서가 CDATA/주석 마커를 올바르게 처리하거나 거부함  
- [ ] 예 — 필터를 숨기거나 우회하기 위해 `<![CDATA[...]]>` 또는 `<!-- ... -->` 주입이 **가능함**

---

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

## WSTG-INPV-09 — XPath 인젝션(XPath Injection) 테스트

XPath 인젝션(XPath Injection)은 애플리케이션이 XML 데이터에 대한 XPath 쿼리를 구성할 때 사용자 제공 정보를 사용하여 공격자가 쿼리 로직을 방해할 수 있을 때 발생합니다. 작은따옴표('), 대괄호([]), 논리 연산자와 같은 특정 구문을 주입함으로써, 공격자는 XML 문서 구조를 탐색하거나 인증을 우회하고 민감한 데이터 노드를 탈취할 수 있습니다. 이 취약점은 기존의 SQL 데이터베이스보다 XML 기반 데이터 저장소에 의존하는 웹 서비스, 구성 관리 인터페이스 및 레거시 시스템에서 흔히 발견됩니다. 공격자의 관점에서 이는 종종 불리언 기반(Boolean-based) 또는 에러 기반(Error-based) 기술을 사용하여 XML 트리를 체계적으로 매핑하고 백엔드에서 숨겨진 값을 검색하는 방식으로 악용됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-09 |
| **CWE** | CWE-643 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 치명적 |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/09-Testing_for_XPath_Injection  
* https://hacktricks.wiki/en/pentesting-web/xpath-injection.html  

**도구:** `Burp Suite (Intruder)`, `XCat`, `XPath-Injection-Tool`, `FFUF`

### XPath 쿼리를 구성하는 데 사용되는 사용자 입력이 적절하게 살균되거나 파라미터화되어 있습니까?
- [ ] 예 — 입력값이 XPath 쿼리에 사용되지 않거나 엄격하게 파라미터화(Parameterized)되어 있습니다.  
- [ ] 예 — 강력한 입력값 검증/인코딩이 적용되어 있으며 우회가 불가능합니다.  
- [ ] 예 — 일부 검증이 적용되어 있으나 인코딩된 문자를 사용하여 우회가 가능합니다.  
- [ ] 아니요 — 사용자 입력이 XPath 쿼리에 직접 결합됩니다 *(치명적)*.  

### 애플리케이션이 에러 메시지를 통해 XML/XPath 구조 세부 정보를 노출합니까?
- [ ] 아니요 — 일반적인 에러 페이지가 표시되며 XML 세부 정보가 유출되지 않습니다.  
- [ ] 예 — 에러 메시지가 XML 처리의 존재를 드러내지만 경로 세부 정보는 포함하지 않습니다.  
- [ ] 예 — 상세한 에러 메시지가 XPath 구문, 노드 이름 또는 파일 경로를 드러냅니다.  

### XPath 로직을 사용하여 인증 또는 권한 부여 로직을 우회할 수 있습니까?
- [ ] 아니요 — 인증 로직이 XML/XPath에 의존하지 않거나 보안상 안전하게 구현되었습니다.  
- [ ] 예 — `' or 1=1 or '`와 같은 로직 기반 페이로드(Payload)를 사용하여 로그인 또는 권한 확인을 우회할 수 있습니다.  

### 블라인드 인젝션(Blind Injection)을 통해 XML 문서 구조 또는 데이터를 열거할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 불리언 기반 또는 시간 기반 추론에 취약하지 않습니다.  
- [ ] 예 — 불리언 기반 추론을 사용하여 노드 이름 및 데이터를 탈취할 수 있습니다.  
- [ ] 예 — 전체 XML 트리 탐색 및 데이터 추출이 가능합니다.

---

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

## WSTG-INPV-13 — 포맷 스트링 인젝션 (Format String Injection)

포맷 스트링 인젝션(Format String Injection)은 웹 애플리케이션이 사용자 제어 입력을 C 언어의 `printf` 계열이나 저수준 로깅 래퍼와 같은 가변 인자 함수(variadic function)의 포맷 스트링 인자로 직접 전달할 때 발생합니다. 최신 매니지드 코드(managed-code) 웹 프레임워크에서는 흔하지 않지만, 이 취약점은 레거시 CGI 애플리케이션, 네이티브 확장 기능 또는 저수준 시스템 라이브러리와 상호 작용하는 특수한 로깅 시스템에서 여전히 치명적입니다. 공격자는 `%x` 또는 `%p`와 같은 포맷 지정자(format specifiers)를 제공하여 스택(Stack)에서 민감한 메모리 주소와 데이터를 유출하거나, `%n` 지정자를 사용하여 임의의 메모리 쓰기를 수행함으로써 이를 악용합니다. 공격에 성공할 경우 일반적으로 원격 코드 실행(Remote Code Execution, RCE)을 통한 완전한 시스템 장악이 가능하며, 최소한 프로세스 충돌을 통한 심각한 서비스 거부(Denial-of-Service, DoS) 상태가 발생할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-13 |
| **CWE** | CWE-134 |
| **테스트 상태** | Not Performed |
| **위험도** | 높음(High) / 치명적(Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/13-Testing_for_Format_String_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `GDB`, `strings`, `radare2`, `Checksec`, `AFL++`

### 애플리케이션이 네이티브 코드나 레거시 CGI 인터페이스를 통해 사용자 입력을 처리합니까?
- [ ] 아니요 — 애플리케이션이 완전히 매니지드 코드 프레임워크(예: JVM, CLR) 기반으로 구축되었습니다.  
- [ ] 예 — 애플리케이션이 JNI, 네이티브 C/C++ 확장 또는 레거시 바이너리 CGI를 사용하며, 포맷 스트링 취약점이 존재할 **수 있습니다**.  

### 사용자 공급 입력이 포맷 인식 함수(format-aware functions)의 기본 인자로 전달됩니까?
- [ ] 아니요 — 입력이 데이터 인자로 전달되며, 포맷 스트링은 **정적(static)**이고 **하드코딩**되어 있습니다.  
- [ ] 예 — 사용자 입력이 포맷 스트링 인자에 직접 연결되지만, 새니타이제이션(Sanitization)이 **적용되어 있습니다**.  
- [ ] 예 — 사용자 입력이 포맷 스트링으로 직접 사용되며, 새니타이제이션이 **적용되지 않았습니다**.  

### 포맷 지정자를 사용하여 메모리 내용을 유출할 수 있습니까?
- [ ] 아니요 — 입력이 검증되었으며 `%p`, `%x`, `%s`와 같은 지정자가 **처리되지 않습니다**.  
- [ ] 예 — `%p` 또는 `%x` 지정자를 공급하면 스택 또는 힙(Heap) 메모리 주소가 반사(Reflection)됩니다.  
- [ ] 예 — 포맷 지정자를 통해 민감한 데이터(예: 포인터, 스택 쿠키)가 유출될 **수 있습니다**.  

### `%n` 지정자를 사용하여 애플리케이션을 충돌시키거나 조작할 수 있습니까?
- [ ] 아니요 — 메모리 쓰기를 허용하는 지정자가 컴파일러/환경에 의해 **비활성화**되었거나 차단되었습니다.  
- [ ] 예 — `%s%s%s%s` 또는 이와 유사한 문자열이 제공될 때 애플리케이션이 충돌(DoS)합니다.  
- [ ] 예 — `%n`을 통한 임의 메모리 쓰기가 **가능**하며, 잠재적으로 원격 코드 실행(RCE)으로 이어질 수 있습니다.  

### 이 인젝션을 통해 현대적 바이너리 보호 기법(ASLR, DEP, Stack Canaries)을 우회할 수 있습니까?
- [ ] 아니요 — 환경이 요새화(Hardening)되어 있으며 메모리 유출을 보호 기법 우회에 **사용할 수 없습니다**.  
- [ ] 예 — 포맷 스트링 인젝션을 사용하여 베이스 주소를 유출할 수 있으며, 이를 통해 ASLR/DEP 우회가 **가능해집니다**.

---

## WSTG-INPV-14 — 배양된 취약점 테스트 (Testing for Incubated Vulnerabilities)

배양된 취약점(Incubated Vulnerabilities)은 지속형 또는 2차 취약점(Persistent or Second-order Vulnerabilities)으로도 불리며, 악성 페이로드(Payload)가 애플리케이션의 "휴면" 상태로 저장되었다가 나중에 다른 문맥에서 실행되거나 처리될 때 발생합니다. 이 공격 패턴은 일반적으로 적절한 재검증이나 출력 인코딩(Output Encoding) 없이 공유 데이터베이스나 파일 시스템에서 데이터를 가져오는 백엔드 시스템, 관리자 콘솔 또는 자동화된 보고 도구를 대상으로 합니다. 모의 해킹 전문가는 입력 지점("배양기")부터 해당 데이터가 최종적으로 렌더링되거나 다른 구성 요소 또는 보조 애플리케이션에서 활용되는 모든 위치까지 사용자 입력의 생명주기를 추적해야 합니다. 성공적인 공격은 저장형 XSS(Stored XSS)를 통한 관리자 세션 탈취나 내부 보고 모듈의 2차 SQL 인젝션(Second-order SQL Injection)을 통한 데이터 유출과 같이 파급력이 큰 결과를 초래하는 경우가 많습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-14 |
| **CWE** | CWE-20 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/14-Testing_for_Incubated_Vulnerability  
* https://hacktricks.wiki/en/pentesting-web/sql-injection/index.html#second-order-injection  
* https://portswigger.net/web-security/web-cache-deception  
* https://portswigger.net/web-security/web-cache-poisoning  

**도구:** `Burp Suite (Collaborator)`, `sqlmap`, `OWASP ZAP`, `KXSs`, `Dalfox`

### 사용자 제어 입력값이 나중에 다른 사용자나 프로세스에 의해 조회되기 위해 저장되는 엔드포인트(API)가 있습니까?
- [ ] 아니요 — 애플리케이션이 나중에 사용하기 위해 사용자 입력을 저장하지 않음  
- [ ] 예 — 입력값이 데이터베이스 필드, 로그 파일 또는 설정 파일에 저장됨  

### 데이터가 영구 저장 계층에 기록되기 전에 입력 검증 또는 새니타이제이션(Sanitization)이 적용됩니까?
- [ ] 예 — 입력 시점에 엄격한 허용 목록 검증(Allow-list validation)이 적용됨  
- [ ] 예 — 일반적인 새니타이제이션이 적용되지만 인코딩이나 비표준 문자를 통한 우회(Bypass)가 가능함  
- [ ] 아니요 — 입력값이 검증되지 않은 원본(Raw) 형태 그대로 저장됨  

### 저장된 데이터가 관리자 또는 보조 인터페이스에서 렌더링될 때 적절하게 인코딩되거나 새니타이제이션 처리됩니까?
- [ ] 예 — 데이터가 렌더링되는 모든 위치에서 문맥 인식 출력 인코딩(Context-aware output encoding)이 적용됨  
- [ ] 예 — 일부 위치에서는 인코딩이 적용되지만 다른 위치(예: 내부 관리자 대시보드)에서는 누락됨  
- [ ] 아니요 — 데이터가 인코딩이나 새니타이제이션 없이 렌더링되어 페이로드 실행이 가능함  

### 데이터베이스에 저장된 페이로드가 백엔드 배치 프로세스 또는 아웃 오브 밴드(OOB) 모니터링 시스템에 영향을 미칠 수 있습니까?
- [ ] 아니요 — 백엔드 프로세스가 안전한 파싱 방법이나 매개변수화된 쿼리(Parameterized queries)를 사용함  
- [ ] 예 — 저장된 페이로드가 백엔드 로직 내에서 상호작용을 유발할 수 있음 (예: CSV Injection, 로그 내 Command Injection)  
- [ ] 예 — 저장된 페이로드가 공격자가 제어하는 서버로 아웃 오브 밴드(OOB) 요청을 트리거할 수 있음

---

## WSTG-INPV-15 — HTTP 분할 및 요청 변조 (HTTP Splitting Smuggling)

HTTP 분할 및 요청 변조(HTTP Splitting and Smuggling) 취약점은 프런트엔드 프록시(Frontend Proxies)와 백엔드 서버(Backend Servers)가 `Content-Length` 및 `Transfer-Encoding` 헤더와 관련된 HTTP 요청 경계를 해석하고 처리하는 방식의 차이로 인해 발생합니다. 공격자는 정교하게 조작된 요청을 통해 백엔드에 숨겨진 요청을 "변조 삽입(Smuggle)"하거나 CRLF 시퀀스(CRLF sequences)를 주입하여 응답을 분할할 수 있으며, 이는 캐시 포이즈닝(Cache Poisoning), 요청 하이재킹(Request Hijacking) 또는 보안 제어 우회로 이어질 수 있습니다. 이러한 결함은 일반적으로 일관되지 않은 파싱 로직을 가진 리버스 프록시(Reverse Proxies), 로드 밸런서(Load Balancers) 또는 콘텐츠 전송 네트워크(CDNs)를 사용하는 복잡한 환경에서 나타납니다. 공격자 관점에서 이는 사용자 트래픽의 리디렉션, 민감한 세션 토큰(Session Tokens) 탈취 및 다른 사용자 세션의 컨텍스트 내에서 권한이 없는 작업 수행을 가능하게 합니다.


| Field | Value |
|---|---|
| **WSTG ID** | WSTG-INPV-15 |
| **CWE** | CWE-444 |
| **Test Status** | 수행되지 않음 (Not Performed) |
| **Severity** | 높음(High) / 심각(Critical) |

**References:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/15-Testing_for_HTTP_Response_Splitting  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  

**Tools:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `smuggler.py`, `curl`

### 환경이 CL.TE 또는 TE.CL 불일치를 통한 요청 변조(Request Smuggling)에 취약합니까?
- [ ] 아니요 — 프런트엔드 및 백엔드 서버가 요청 경계를 일관되게 처리함  
- [ ] 예 — 불일치가 존재하지만 인프라 완화 조치로 인해 익스플로잇(Exploit)이 불가능함  
- [ ] 예 — CL.TE 또는 TE.CL 스머글링이 가능하며, 숨겨진 요청이 백엔드에 도달할 수 있음 *(High)*  
- [ ] 예 — 프런트엔드 필터를 우회하기 위해 TE.TE (이중 인코딩/난독화)가 가능함 *(Critical)*  

### 헤더의 CRLF 인젝션(CRLF Injection)을 통해 HTTP 응답 분할(HTTP Response Splitting)을 달성할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 모든 헤더 입력에서 CRLF 시퀀스를 적절히 정리(Sanitize)함  
- [ ] 예 — CRLF 시퀀스가 헤더에 반영되지만 응답 분할은 불가능함  
- [ ] 예 — CRLF 인젝션이 가능하며, 헤더 주입 또는 캐시 포이즈닝이 허용됨  

### 변조된 요청을 사용하여 보안 제어(WAF/ACL)를 우회합니까?
- [ ] 아니요 — 보안 제어가 외부 요청과 변조된 요청 모두에 적용됨  
- [ ] 예 — 변조된 요청이 프런트엔드 WAF 규칙 또는 IP 기반 ACL을 우회할 수 있음  

### 다른 사용자의 세션을 하이재킹하거나 트래픽을 리디렉션할 수 있습니까?
- [ ] 아니요 — 요청/응답 스트림이 격리되어 있으며 교차될 수 없음  
- [ ] 예 — 요청 변조가 가능하며 다른 사용자의 요청을 캡처할 수 있음 *(Critical)*  
- [ ] 예 — 응답 분할이 가능하며 브라우저 측 캐시 포이즈닝 또는 XSS를 허용함

---

## WSTG-INPV-16 — HTTP 요청 스머글링 (HTTP Request Smuggling)

HTTP 요청 스머글링 (HTTP Request Smuggling, HRS)은 로드 밸런서 (Load Balancer)와 백엔드 웹 서버 (Back-end Web Server)와 같은 HTTP 서버 체인이 주로 `Content-Length` 및 `Transfer-Encoding` 헤더의 충돌로 인해 요청의 경계를 서로 다르게 해석할 때 발생합니다. 공격자는 이러한 비동기화 (Desynchronization) 상태를 악용하여 백엔드 서버의 요청 버퍼에 항목을 "밀어 넣어(smuggle)", 결과적으로 다음 정당한 사용자의 요청 앞에 공격자가 제어하는 세그먼트가 접두사로 붙게 만듭니다. 이 기법을 통해 세션 하이재킹 (Session Hijacking), 보안 통제 우회 (Bypassing Security Controls), 캐시 포이즈닝 (Cache Poisoning) 등 심각한 공격이 가능해집니다. 취약점 공격은 주로 타이밍 기반 분석 (Timing-based Analysis)이나 후속 요청을 서버에 보낼 때 백엔드로부터 예기치 않은 응답이 오는지 관찰함으로써 수행됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각 (Critical) / 높음 (High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**도구:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### 프런트엔드와 백엔드 서버가 `Transfer-Encoding: chunked` 헤더를 일관되게 처리합니까?
- [ ] 예 — 두 서버 모두 청크 인코딩을 동일하게 처리하며 비동기화가 **가능하지 않음**  
- [ ] 아니오 — 서버가 헤더를 다르게 해석하지만 새니타이징 (Sanitization) 또는 정규화 (Normalization)가 **적용됨**  
- [ ] 아니오 — CL.TE (Content-Length/Transfer-Encoding) 비동기화가 **가능함** *(심각)*  
- [ ] 아니오 — TE.CL (Transfer-Encoding/Content-Length) 비동기화가 **가능함** *(심각)*  

### 공격자가 스머글링된 요청을 사용하여 서버 또는 클라이언트 측 캐시를 오염시킬 수 있습니까?
- [ ] 아니오 — 캐싱이 **비활성화**되어 있거나 포이즈닝에 취약하지 않음  
- [ ] 예 — 스머글링된 요청이 사용자를 악성 도메인으로 리다이렉트하거나 캐시 포이즈닝을 통해 스크립트를 삽입할 수 **있음**  

### 요청 연결 (Request Concatenation)을 통해 다른 사용자의 요청이나 세션 토큰을 탈취할 수 있습니까?
- [ ] 아니오 — 스머글링이 사용자 간 데이터 노출을 허용하지 **않음**  
- [ ] 예 — 스머글링을 통해 다음 사용자의 요청을 공격자가 제어하는 `POST` 본문에 추가하여 데이터 유출 (Exfiltration)이 **가능함**  

### 인프라에서 HTTP/2를 사용하며, H2.CL 또는 H2.TE 비동기화에 취약합니까?
- [ ] 아니오 — 애플리케이션이 HTTP/1.1만 사용하거나 HTTP/2가 다운그레이드 (Downgrade) 없이 안전하게 구현됨  
- [ ] 예 — HTTP/2에서 HTTP/1.1로의 일반 텍스트 다운그레이드가 발생하며 비동기화가 **가능함**  

### 잘못된 형식의 헤더(예: 공백이 포함된 `Transfer-Encoding:  chunked`)가 안전하게 처리됩니까?
- [ ] 예 — 잘못된 형식의 헤더가 모든 계층에서 거부되거나 정규화됨  
- [ ] 아니오 — 잘못된 형식이나 모호한 헤더의 일관되지 않은 처리로 인해 TE.TE 비동기화가 **가능함**

---

## WSTG-INPV-17 — 호스트 헤더 인젝션 (Host Header Injection) 테스트

호스트 헤더 인젝션(Host Header Injection)은 애플리케이션이 사용자가 제공한 `Host` 헤더를 신뢰하여 충분한 검증 없이 링크 생성이나 리다이렉트와 같은 서버 측 로직에 포함할 때 발생합니다. 공격자는 이 헤더를 조작하여 웹 캐시 포이즈닝 (Web Cache Poisoning)을 유발하거나, 민감한 토큰(Token)을 외부 도메인으로 리다이렉트하여 비밀번호 초기화 포이즈닝 (Password Reset Poisoning)을 수행하거나, 헤더 기반 라우팅 (Header-based Routing)에 의존하는 보안 통제를 우회할 수 있습니다. 성공적인 익스플로잇 (Exploit)을 통해 공격자는 세션 데이터를 유출하고, 계정 탈취 (Account Takeover)를 수행하거나, 의심하지 않는 사용자를 악성 인프라로 리다이렉트할 수 있습니다. 이 취약점은 부하 분산(Load-balanced) 환경이나 수신된 요청 컨텍스트를 기반으로 절대 URL(Absolute URLs)을 동적으로 생성하는 애플리케이션에서 흔히 발견됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-17 |
| **CWE** | CWE-20 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간(Medium) / 상(High)* |

> *비밀번호 초기화와 같이 이메일 내 민감한 링크를 생성하는 데 `Host` 헤더가 사용되는 경우 위험도는 '상(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/host-header  

**도구:** `Burp Suite (Repeater/Collaborator)`, `curl`, `Param Miner`, `nmap (http-vhosts)`

### 애플리케이션이 링크, 리다이렉트 또는 스크립트 내에 `Host` 헤더 값을 반영합니까?
- [ ] 아니오 — 애플리케이션이 하드코딩된 기본 URL 또는 엄격하게 검증된 설정을 사용함  
- [ ] 예 — `Host` 헤더가 반영되지만 화이트리스트 (Whitelist)에 대해 엄격하게 검증됨  
- [ ] 예 — `Host` 헤더가 반영되며 변조된 값(예: `expected.com:attacker.com`)을 통해 우회 (Bypass)가 가능함  
- [ ] 예 — 어떠한 검증 없이 `Host` 헤더가 반영됨  

### 애플리케이션 또는 인프라에서 호스트 관련 대체 헤더를 처리합니까?
- [ ] 아니오 — `X-Forwarded-Host`, `X-Host`, 또는 `Forwarded`와 같은 헤더가 무시됨  
- [ ] 예 — 대체 헤더가 처리되지만 내부 로직을 덮어쓰지는 않음  
- [ ] 예 — 대체 헤더를 사용하여 기본 `Host` 헤더 값을 덮어쓸 수 있음  

### 시스템에서 생성된 이메일(예: 비밀번호 초기화)을 포이즈닝하기 위해 `Host` 헤더를 조작할 수 있습니까?
- [ ] 아니오 — 이메일 링크가 서버 설정의 신뢰할 수 있는 정적 도메인을 사용함  
- [ ] 예 — `Host` 헤더가 이메일 링크에 영향을 주지만 검증을 우회할 수 없음  
- [ ] 예 — 공격자 제어 도메인으로 비밀번호 초기화 토큰을 리다이렉트하도록 `Host` 헤더를 조작할 수 있음 *(치명적)*  

### 애플리케이션이 `Host` 헤더를 통한 웹 캐시 포이즈닝 (Web Cache Poisoning)에 취약합니까?
- [ ] 아니오 — 캐싱 메커니즘이 없거나 `Host` 헤더가 캐시 키(Cache Key)의 일부임  
- [ ] 예 — 캐시가 존재하지만 언키드 입력값 (Unkeyed Inputs)에 대해 `Host` 헤더를 엄격히 검증하거나 무시함  
- [ ] 예 — 캐시가 존재하며 삽입된 `Host` 헤더에 의해 포이즈닝되어 다른 사용자에게 악성 콘텐츠를 제공할 수 있음  

### 서버가 가상 호스트 라우팅을 위해 임의의 `Host` 헤더를 허용합니까?
- [ ] 아니오 — 서버가 인식되지 않는 `Host` 헤더에 대해 오류 또는 기본 페이지를 반환함  
- [ ] 예 — 서버가 모든 `Host` 헤더를 허용하며, 이는 정찰 (Reconnaissance) 또는 내부 서비스 열거에 유용함

---

## WSTG-INPV-18 — 서버 측 템플릿 주입(Server-Side Template Injection, SSTI) 점검

서버 측 템플릿 주입(Server-Side Template Injection, SSTI)은 애플리케이션이 적절한 검증 및 정제(Sanitization) 또는 이스케이핑(Escaping) 없이 사용자 제어 입력을 서버 측 템플릿 엔진에 포함할 때 발생합니다. 이를 통해 공격자는 악성 템플릿 지시어를 주입할 수 있으며, 이는 서버에서 실행되어 원격 코드 실행(Remote Code Execution, RCE)을 통한 완전한 시스템 침해로 이어질 수 있습니다. SSTI는 일반적으로 Jinja2, Twig 또는 FreeMarker와 같은 엔진을 사용하는 동적 이메일 생성, 프로필 페이지 및 알림 시스템과 같은 기능에서 나타납니다. 공격자 관점에서 이 취약점은 템플릿 엔진의 고유 객체 및 메서드를 활용하여 민감한 환경 변수를 유출하거나, 내부 파일을 읽거나, 시스템 명령을 실행하는 데 자주 악용됩니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-18 |
| **CWE** | CWE-1336 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 심각 (Critical) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/18-Testing_for_Server-side_Template_Injection  
* https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/index.html  
* https://portswigger.net/web-security/server-side-template-injection  

**도구:** `tplmap`, `Burp Suite (Intruder/Repeater)`, `ffuf`, `SSTI-Payload-Generator`

### 애플리케이션이 사용자 입력을 처리하기 위해 서버 측 템플릿 엔진을 사용합니까?
- [ ] 아니요 — 애플리케이션이 동적 콘텐츠를 위해 서버 측 템플릿을 사용하지 **않음**  
- [ ] 예 — 템플릿이 사용되지만 사용자 입력이 템플릿 지시어에 포함되지 **않음**  
- [ ] 예 — 사용자 입력이 적절한 검증 및 정제 없이 템플릿에 **포함됨**  

### 입력 필드에 주입된 기본 산술 식이 평가(Evaluate)됩니까?
- [ ] 아니요 — `{{7*7}}` 또는 `${7*7}`과 같은 페이로드(Payload)가 리터럴 문자열로 그대로 출력됨  
- [ ] 예 — 식이 평가되어 결과(예: `49`)가 반환되며, 이는 템플릿 엔진이 **취약함**을 확인하는 지표임  

### 템플릿 엔진의 내장 객체 또는 메서드에 접근할 수 있습니까?
- [ ] 아니요 — 위험한 객체, 메서드 또는 설정에 대한 접근이 **제한**되거나 샌드박스(Sandbox) 처리되어 있음  
- [ ] 예 — 내장 객체(예: `self`, `config`, `__class__`)에 접근하여 민감한 데이터를 유출할 수 **있음**  
- [ ] 예 — 시스템 호출 또는 OS 라이브러리를 통한 원격 코드 실행(RCE)이 **가능함**  

### 위험한 지시어의 실행을 방지하는 샌드박스 또는 화이트리스트(Allowlist) 메커니즘이 있습니까?
- [ ] 예 — 엄격한 화이트리스트 또는 보안 샌드박스가 적용되어 있으며 우회(Bypass)가 **불가능함**  
- [ ] 예 — 샌드박스가 존재하지만 특정 엔진 종속적인 페이로드를 통해 우회가 **가능함**  
- [ ] 아니요 — 템플릿 엔진에 샌드박스 또는 화이트리스트가 **적용되지 않음**

---

## WSTG-INPV-19 — 서버 측 요청 위조 (Server-Side Request Forgery, SSRF) 테스트

서버 측 요청 위조(Server-Side Request Forgery, SSRF)는 공격자가 웹 애플리케이션을 조작하여 서버에서 내부 또는 외부 리소스로 무단 요청을 보내도록 할 때 발생합니다. URL, 호스트 이름 또는 IP 주소를 기대하는 파라미터를 조작함으로써, 공격자는 방화벽이나 ACL(Access Control List)과 같은 네트워크 수준의 통제를 우회하여 내부 네트워크 세그먼트를 조사하거나, 클라우드 메타데이터 서비스(IMDS)에 액세스하거나, 취약한 내부 API 및 데이터베이스와 상호 작용할 수 있습니다. 이 취약점은 일반적으로 이미지 가져오기, PDF 생성, 웹훅(Webhooks) 또는 URL을 통한 파일 업로드 기능에서 나타납니다. 공격자 관점에서 SSRF는 격리된 환경으로 침투(Pivot)하고, 민감한 설정 데이터를 탈취(Exfiltrate)하며, Redis나 Memcached와 같은 내부 서비스와의 상호 작용을 통해 잠재적으로 원격 코드 실행(Remote Code Execution, RCE)을 달성할 수 있는 강력한 수단(Primitive)입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-19 |
| **CWE** | CWE-918 |
| **테스트 상태** | 미실행 |
| **위험도** | 높음 / 치명적 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/19-Testing_for_Server-Side_Request_Forgery  
* https://hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html  
* https://portswigger.net/web-security/ssrf  

**도구:** `Burp Suite (Collaborator/Interactsh)`, `Interact.sh`, `Gopherus`, `ffuf`, `cURL`, `DNSRebind Tool`

### 애플리케이션이 사용자 제공 URL 또는 호스트 이름을 처리합니까?
- [ ] 아니오 — 외부 URL 또는 호스트 이름을 허용하는 기능이 없습니다.
- [ ] 예 — 기능이 존재하지만 입력값이 허용 목록(Allow-list)에 따라 엄격하게 검증됩니다.
- [ ] 예 — 기능이 존재하며 사용자 제공 URL을 허용합니다.

### 요청된 대상에 대해 검증 통제 또는 차단 목록(Deny-list)이 적용되어 있습니까?
- [ ] 예 — 신뢰할 수 있는 도메인/IP에 대한 엄격한 허용 목록(Allow-list)이 강제됩니다.
- [ ] 예 — 차단 목록(Deny-list)이 사용됩니다 (예: `127.0.0.1`, `169.254.169.254` 차단).
- [ ] 아니오 — 입력값에 대해 검증이나 필터링이 적용되지 않습니다.

### 인코딩, 리다이렉트 또는 DNS 리바인딩(DNS Rebinding)을 통해 필터를 우회할 수 있습니까?
- [ ] 아니오 — 필터가 견고하며 리다이렉트/DNS 해석을 안전하게 처리합니다.
- [ ] 예 — URL 인코딩 또는 대체 IP 형식(16진수, 8진수)을 사용하여 우회가 가능합니다.
- [ ] 예 — 내부 대상을 향하는 3xx HTTP 리다이렉트를 통해 우회가 가능합니다.
- [ ] 예 — 내부 IP로 해석되도록 하는 DNS 리바인딩 공격을 통해 우회가 가능합니다.

### 애플리케이션이 내부 메타데이터 서비스 또는 로컬 인터페이스에 도달할 수 있습니까?
- [ ] 아니오 — 로컬 네트워크 및 클라우드 메타데이터 엔드포인트에 연결할 수 없습니다.
- [ ] 예 — 로컬 호스트(`127.0.0.1`) 또는 루프백(Loopback) 서비스에 액세스가 가능합니다.
- [ ] 예 — 클라우드 메타데이터 서비스(예: AWS/GCP/Azure IMDS)에 액세스가 가능합니다. *(치명적)*

### SSRF 응답의 성격은 무엇입니까?
- [ ] Blind — 사용자에게 응답이나 메타데이터가 반환되지 않습니다.
- [ ] Partial — 응답 메타데이터(헤더, 크기, 시간 등)가 사용자에게 반환됩니다.
- [ ] Full — 내부 리소스로부터의 전체 응답 본문(Response Body)이 사용자에게 반사(Reflected)되어 나타납니다.

---

## WSTG-INPV-20 — 대량 할당 (Mass Assignment)

대량 할당 (Mass Assignment)은 오버포스팅 (Overposting) 또는 매개변수의 안전하지 않은 역직렬화 (Insecure Deserialization)라고도 하며, 웹 애플리케이션이 적절한 속성 필터링 없이 유입되는 HTTP 요청 매개변수(HTTP request parameters)를 내부 객체 속성에 자동으로 바인딩할 때 발생합니다. 이 취약점은 JSON, XML 또는 폼 인코딩된 데이터가 애플리케이션 측 데이터 모델로 직접 역직렬화되는 현대적인 MVC 프레임워크와 RESTful API에서 흔히 발견됩니다. 공격자는 요청에 `isAdmin`, `role` 또는 `account_balance`와 같이 정의되지 않은 추가 매개변수를 주입하여 개발자가 사용자 제어에 노출하려 의도하지 않았던 민감한 필드를 수정함으로써 이 동작을 악용합니다. 성공적인 공격은 일반적으로 권한 상승 (Privilege Escalation), 무단 데이터 수정 또는 중요한 비즈니스 로직 워크플로우 우회를 초래합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-20 |
| **CWE** | CWE-915 |
| **테스트 상태** | Not Performed |
| **위험도** | High / Medium* |

> *관리자 속성, 권한 수준 또는 결제 관련 필드를 수정할 수 있는 경우 위험도는 High(높음)가 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/20-Testing_for_Mass_Assignment  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Arjun`, `Param Miner`, `Burp Suite (Repeater)`, `Postman`, `ffuf`

### 애플리케이션이 유입되는 요청 매개변수에 대해 자동 바인딩(Automatic Binding)을 사용합니까?
- [ ] 아니요 — 매개변수가 특정 변수나 안전한 객체에 수동으로 매핑됩니다.  
- [ ] 예 — 자동 바인딩이 사용되지만 엄격한 화이트리스트 (White-lists) 또는 DTO (Data Transfer Objects)가 강제 적용됩니다.  
- [ ] 예 — 자동 바인딩이 사용되며 민감한 내부 속성을 수정할 수 있습니다.  

### 관리자 또는 권한 관련 속성을 성공적으로 주입할 수 있습니까?
- [ ] 아니요 — 주입된 매개변수는 무시되거나 제거되거나 유효성 검사 오류를 발생시킵니다.  
- [ ] 예 — 추가 매개변수가 수용되지만 백엔드 객체 상태에는 영향을 미치지 않습니다.  
- [ ] 예 — `is_admin`, `role` 또는 `permissions`와 같은 매개변수 주입을 통해 권한 상승 (Privilege Escalation)에 성공합니다. *(Critical)*  

### 오버포스팅을 통해 민감한 비즈니스 로직이나 금융 필드를 수정하는 것이 가능합니까?
- [ ] 아니요 — `price`, `balance` 또는 `status`와 같은 필드는 대량 할당으로부터 보호됩니다.  
- [ ] 예 — JSON 또는 폼 요청 본문에 속성을 추가함으로써 민감한 비즈니스 속성을 수정할 수 있습니다.  

### 애플리케이션이 매개변수 추측을 용이하게 하는 내부 객체 구조를 노출합니까?
- [ ] 아니요 — 응답에는 의도된 공개 필드만 포함되며 문서는 제한되어 있습니다.  
- [ ] 예 — GET 응답에서 내부 객체 필드가 노출되어 매개변수 발견이 가능합니다.  
- [ ] 예 — API 문서 (Swagger/OpenAPI)에 할당 가능한 내부 속성이 명시적으로 나열되어 있습니다.

---

## WSTG-ERRH-01 — 부적절한 오류 처리(Improper Error Handling) 테스트

부적절한 오류 처리는 웹 애플리케이션이 오류 응답을 통해 스택 트레이스(stack traces), 데이터베이스 스키마(database schema) 정보 또는 내부 파일 경로(internal file paths)와 같은 민감한 기술적 세부 정보를 노출할 때 발생합니다. 공격자는 잘못된 형식의 입력값(malformed input)을 제출하거나, 존재하지 않는 리소스에 접근하거나, 서버 측 예외(server-side exceptions)를 유발하여 애플리케이션의 기본 아키텍처를 파악하고 추가 공격을 위한 잠재적 벡터를 식별합니다. 이러한 정보 유출은 공격자에게 정확한 환경 사양을 제공함으로써 SQL 인젝션(SQL Injection) 또는 경로 탐색(Path Traversal)을 포함한 더 심각한 공격의 전조 역할을 하는 경우가 많습니다. 안전한 구현을 위해서는 상세한 진단 정보는 보안이 유지되는 서버 측 로그에만 기록하고, 사용자에게는 일반적인 사용자 친화적 메시지를 제공해야 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ERRH-01 |
| **CWE** | CWE-209 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Repeater/Intruder)`, `ffuf`, `wfuzz`, `Arjun`, `curl`

### 애플리케이션이 처리되지 않은 예외에 대해 일반적인 글로벌 오류 핸들러를 사용합니까?
- [ ] 예 — 일반 오류 페이지가 **활성화**되어 있으며 기술적인 세부 정보를 **노출하지 않음**  
- [ ] 예 — 사용자 정의 오류 페이지를 사용하지만 특정 헤더를 통해 정보 유출이 **가능함**  
- [ ] 아니요 — 기본 서버 오류 페이지(예: Tomcat, IIS, Nginx)가 **노출됨**  

### 잘못된 형식의 입력값이나 경계값 케이스를 통해 기술적인 정보를 나열(Enumeration)할 수 있습니까?
- [ ] 아니요 — 로깅을 위한 고유 참조 ID와 함께 오류가 적절히 처리됨  
- [ ] 예 — 응답에 스택 트레이스 또는 데이터베이스 쿼리가 **노출됨**  
- [ ] 예 — 내부 파일 경로, 환경 변수 또는 서버 버전 문자열이 **노출됨**  

### API 응답에서 상세한 오류 객체나 디버그 정보가 유출되고 있습니까?
- [ ] 아니요 — API가 표준화된 오류 코드와 정제된 JSON 메시지를 반환함  
- [ ] 예 — API 응답 본문에 상세한 디버그 객체나 전체 예외 세부 정보가 포함됨  
- [ ] 예 — JSON 응답의 `details` 또는 `exception` 필드에 스택 트레이스가 포함됨  

### 오류 발생 시 애플리케이션이 다르게 동작(시간 기반 또는 콘텐츠 기반)합니까?
- [ ] 아니요 — 오류 유형에 관계없이 응답 시그니처와 타이밍이 일관됨  
- [ ] 예 — 차분 응답(differential responses)을 사용하여 유효한 상태와 유효하지 않은 상태를 구분 및 나열할 수 있음 (예: 사용자 나열(User Enumeration))

---

## WSTG-ERRH-02 — 스택 트레이스 테스트 (Testing for Stack Traces)

애플리케이션이 예외(Exception)를 적절하게 처리하지 못할 때 스택 트레이스(Stack Traces)가 생성되며, 그 결과 내부 실행 상태와 호출 계층 구조가 최종 사용자에게 노출됩니다. 모의 침투 테스터에게 이러한 트레이스는 애플리케이션 프레임워크, 라이브러리 버전, 내부 파일 시스템 경로 및 로직 흐름을 드러내어 정찰(Reconnaissance)에 필요한 노력을 크게 줄여주기 때문에 매우 유용합니다. 공격(Exploitation)은 잘못된 형식의 입력, 예상치 못한 데이터 유형 또는 경계 조건 위반을 통해 의도적으로 오류를 유발하여 애플리케이션을 불안정한 상태로 만들고 기본 기술 스택(Technology Stack)에 대한 정보를 탈취하는 방식으로 이루어집니다. 이러한 유출은 취약한 서드파티 컴포넌트를 식별하거나 노출된 코드 경로 내에서 발견된 로직 결함에 대해 구체적인 익스플로잇(Exploit)을 제작하는 데 필요한 문맥을 제공하는 경우가 많습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-ERRH-02 |
| **CWE** | CWE-209 |
| **테스트 상태** | 미수행 |
| **심각도** | 낮음 / 중간* |

> *스택 트레이스가 민감한 아키텍처 세부 정보, 데이터베이스 쿼리 또는 내부 설정 파라미터를 노출할 경우 심각도는 '중간(Medium)'으로 높아집니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/02-Testing_for_Stack_Traces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `ffuf`, `Arjun`, `Wfuzz`, `Wappalyzer`

### 애플리케이션 오류 발생 시 전체 스택 트레이스가 클라이언트에 반환됩니까?
- [ ] 아니요 — 애플리케이션이 일반적인 오류 페이지 또는 사용자 정의 오류 메시지를 반환합니다.  
- [ ] 예 — 스택 트레이스가 **활성화**되어 있으나 민감하지 않은 특정 모듈에 대해서만 표시됩니다.  
- [ ] 예 — 전체 스택 트레이스가 **활성화**되어 있으며 HTTP 응답 본문에 표시됩니다.  

### 오류 메시지가 민감한 환경 정보를 노출합니까?
- [ ] 아니요 — 오류 메시지에 기술적 또는 환경적 세부 정보가 포함되어 있지 않습니다.  
- [ ] 예 — 메시지에 **내부 파일 경로** 또는 서버 측 **디렉토리 구조**가 노출됩니다.  
- [ ] 예 — 메시지에 **데이터베이스 스키마**, **SQL 쿼리** 또는 **서드파티 라이브러리 버전**이 노출됩니다.  

### 입력 파라미터를 조작하여 스택 트레이스를 유발할 수 있습니까?
- [ ] 아니요 — 잘못된 형식의 입력이 적절하게 처리되거나 유효성 검사에 의해 거부됩니다.  
- [ ] 예 — **예상치 못한 데이터 유형**(예: 문자열이 필요한 곳에 배열을 제출함)으로 인해 트레이스가 유발될 수 있습니다.  
- [ ] 예 — **널 바이트(Null bytes)**, **특수 문자** 또는 **경계값**에 의해 트레이스가 유발됩니다.  

### 애플리케이션 전반에 걸쳐 글로벌 오류 핸들러(Global Error Handler)가 일관되게 적용되어 있습니까?
- [ ] 예 — 모든 테스트된 엔드포인트에 글로벌 예외 핸들러가 **일관되게 적용**되어 있습니다.  
- [ ] 예 — 핸들러가 존재하지만 레거시, API 또는 새로 추가된 엔드포인트에는 **적용되지 않았습니다**.  
- [ ] 아니요 — 글로벌 오류 처리 메커니즘이 **탐지되지 않으며**, 결과적으로 기본 서버 오류가 발생합니다.

---

## WSTG-CRYP-01 — 취약한 전송 계층 보안 테스트 (Testing for Weak Transport Layer Security)

취약한 전송 계층 보안 테스트는 데이터 전송 중의 기밀성과 무결성을 보장하기 위해 SSL/TLS의 구현 및 설정을 분석하는 과정을 포함합니다. 공격자는 중간자 공격 (Man-in-the-Middle, MitM)을 수행하기 위해 오래된 프로토콜(SSLv2, TLS 1.0), 취약한 암호 스위트 (Cipher Suite)(NULL, RC4 또는 익명), 유효하지 않거나 취약하게 서명된 인증서와 같은 설정 오류를 표적으로 삼습니다. 이 테스트는 일반적으로 암호화된 통신이 수립되는 애플리케이션의 웹 서버 또는 로드 밸런서 (Load Balancer) 엔드포인트를 대상으로 합니다. 성공적인 익스플로잇 (Exploit)을 통해 공격자는 민감한 데이터를 가로채거나, 사용자 세션을 탈취하거나, 프로토콜 다운그레이드 또는 트래픽 복호화를 통해 연결을 안전하지 않은 상태로 강제 전환할 수 있습니다.


| 항목 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-01 |
| **CWE** | CWE-326 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음(High) / 중간(Medium)* |

> *민감한 데이터(개인 식별 정보 (Personally Identifiable Information, PII), 자격 증명 (Credentials), 세션 토큰 (Session Token))가 전송되는 경우 위험도는 '높음'이며, 일반적인 정보 트래픽의 경우 '중간'입니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/01-Testing_for_Weak_Transport_Layer_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `testssl.sh`, `sslyze`, `nmap`, `OpenSSL`, `Qualys SSL Labs`

### 오래된 또는 안전하지 않은 TLS/SSL 프로토콜이 지원됩니까?
- [ ] 아니요 — TLS 1.2 및 TLS 1.3만 **활성화됨**  
- [ ] 예 — TLS 1.0 또는 TLS 1.1이 **활성화됨**  
- [ ] 예 — SSLv2 또는 SSLv3가 **활성화됨** *(심각)*  

### 서버가 취약하거나 안전하지 않은 암호 스위트를 허용합니까?
- [ ] 아니요 — 강력하고 현대적인 암호 방식(예: AES-GCM, ChaCha20)만 **지원됨**  
- [ ] 예 — 취약한 암호화 방식(예: 3DES, RC4) 또는 낮은 비트 길이의 암호가 **지원됨**  
- [ ] 예 — NULL, 익명(ADH) 또는 수출용(Export) 암호가 **지원됨** *(심각)*  

### 서버 인증서가 유효하며 신뢰할 수 있습니까?
- [ ] 예 — 인증서가 유효하고, 도메인과 일치하며, **신뢰할 수 있는 인증 기관 (Certificate Authority, CA)**에 의해 서명됨  
- [ ] 아니요 — 인증서가 **만료됨** 또는 **취약한 서명 알고리즘**(예: SHA-1)을 사용함  
- [ ] 아니요 — 인증서가 **자기 서명됨 (Self-signed)** 또는 도메인 이름 **불일치 (Mismatch)**가 발생함  

### 서버가 안전한 재협상(Secure Renegotiation)을 지원하고 다운그레이드 공격을 방지합니까?
- [ ] 예 — 안전한 재협상이 **지원됨** 및 TLS Fallback SCSV가 **활성화됨**  
- [ ] 아니요 — 안전한 재협상이 **지원되지 않음**, 잠재적인 MitM 인젝션 허용  
- [ ] 아니요 — 서버가 **POODLE** 또는 **ROBOT** 공격에 취약함  

### HTTP 엄격한 전송 보안 (HTTP Strict Transport Security, HSTS)이 적절히 구현되었습니까?

| 헤더 | 존재함 | 누락됨 |
|--------|:-------:|:-------:|
| `Strict-Transport-Security` |  |  |

- [ ] 예 — HSTS 헤더가 충분히 긴 `max-age`와 함께 존재하며 `includeSubDomains`가 **활성화됨**  
- [ ] 예 — HSTS 헤더가 존재하지만 `includeSubDomains`가 누락되었거나 **짧은 max-age**를 가짐  
- [ ] 아니요 — HSTS 헤더가 **존재하지 않음**

---

## WSTG-CRYP-02 — 패딩 오라클(Padding Oracle) 테스트

패딩 오라클(Padding Oracle) 취약점은 애플리케이션의 복호화 과정에서 복호화된 암호문(Ciphertext)의 패딩(Padding) 유효성 여부에 대한 정보가 유출될 때 발생합니다. 공격자는 고유한 HTTP 상태 코드, 특정 오류 메시지 또는 응답 시간 차이와 같은 이러한 부가 채널(Side-channel) 응답을 관찰함으로써, 암호화 키를 모르더라도 데이터를 복호화하고 잠재적으로 임의의 평문(Plaintext)을 재암호화할 수 있습니다. 이 취약점은 일반적으로 PKCS#7 패딩이 적용된 CBC(Cipher Block Chaining) 모드의 블록 암호(Block Cipher)를 사용하는 암호화된 세션 쿠키, 인증 토큰 또는 URL 파라미터에서 나타납니다. 취약점 악용 시 기밀성이 완전히 상실될 수 있으며, 위조된 암호화 블록 생성을 통해 무결성 침해로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-02 |
| **CWE** | CWE-209 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical)* |

> *세션 관리, 신원 토큰 또는 개인정보(PII) 암호화와 관련된 취약점인 경우 위험도는 '치명적'으로 간주됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/02-Testing_for_Padding_Oracle  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `PadBuster`, `Burp Suite (Intruder/Padding Oracle Solver)`, `pkcs7-padbuster`, `CyberChef`

### 민감한 데이터에 CBC 모드의 블록 암호가 사용됩니까?
- [ ] 아니요 — 애플리케이션이 인증된 암호화(Authenticated Encryption, AES-GCM/ChaCha20-Poly1305)를 사용함  
- [ ] 예 — 애플리케이션이 블록 암호를 사용하지만 패딩이 필요하지 않음 (예: 스트림 암호 또는 CTR 모드)  
- [ ] 예 — 애플리케이션이 CBC 모드를 사용하며 패딩이 적용됨  

### 서버가 부가 채널을 통해 패딩 유효성을 노출합니까?
- [ ] 아니요 — 서버가 일관된 오류 메시지와 응답 시간을 반환함  
- [ ] 예 — 서버가 패딩 오류에 대해 서로 다른 HTTP 상태 코드(예: 200 vs 500)를 반환함  
- [ ] 예 — 서버가 특정 오류 메시지(예: "Invalid Padding", "Decryption failed")를 반환함  
- [ ] 예 — 복호화 실패 시 서버에서 측정 가능한 시간 차이가 발생함  

### 암호문의 자동 복호화가 가능합니까?
- [ ] 아니요 — 부가 채널이 악용하기에 충분히 안정적이지 않음  
- [ ] 예 — 암호문의 일부(마지막 몇 블록)를 복호화할 수 있음  
- [ ] 예 — `PadBuster`와 같은 도구를 사용하여 전체 평문 복구가 가능함  

### 공격자가 비트 플리핑(Bit-flipping)을 수행하거나 유효한 암호문을 위조할 수 있습니까?
- [ ] 아니요 — 복호화 전 무결성 검사(HMAC)가 수행됨  
- [ ] 예 — 무결성 검사가 없으나, 페이로드(Payload) 구성이 불가능함  
- [ ] 예 — 임의의 암호문 구성이 가능하여 권한 상승(Privilege Escalation) 또는 데이터 주입(Data Injection)이 가능함

---

## WSTG-CRYP-03 — 암호화되지 않은 채널을 통해 전송되는 민감 정보 테스트 (Testing for Sensitive Information Sent Via Unencrypted Channels)

암호화되지 않은 일반 HTTP와 같은 채널을 통해 민감한 데이터를 전송하면, 중간자 공격 (Man-in-the-Middle, MITM)을 통해 정보가 가로채기되거나 수정될 위험이 있습니다. 이 취약점은 일반적으로 웹 애플리케이션이 모든 엔드포인트 (Endpoint)에서 전송 계층 보안 (Transport Layer Security, TLS)을 강제하지 않을 때 발생하며, 특히 레거시 컴포넌트, 로그인 폼, 또는 HTTP에서 HTTPS로 전환되는 초기 단계에서 자주 나타납니다. 공격자 관점에서 이는 공용 Wi-Fi와 같이 침해되었거나 신뢰할 수 없는 네트워크 세그먼트에서 인증 자격 증명, 세션 토큰 (Session Tokens), 개인 식별 정보 (Personally Identifiable Information, PII)를 수동적으로 스니핑 (Passive Sniffing) 할 수 있게 합니다. 모든 민감한 데이터가 견고한 TLS 터널 내에 캡슐화되도록 보장하는 것은 사용자 상호작용의 기밀성 및 무결성을 유지하고 세션 하이재킹 (Session Hijacking)을 방지하는 데 필수적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-03 |
| **CWE** | CWE-319 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) / 중간 (Medium) |

> *인증 자격 증명, 세션 식별자 또는 PII가 평문 (Cleartext)으로 전송되는 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/03-Testing_for_Sensitive_Information_Sent_via_Unencrypted_Channels  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Wireshark`, `tcpdump`, `Burp Suite (Proxy/Alerts)`, `nmap`, `sslyze`, `testssl.sh`

### 애플리케이션이 민감한 엔드포인트에 대해 암호화되지 않은 HTTP 통신을 허용합니까?
- [ ] 아니요 — 모든 애플리케이션 트래픽이 HTTPS로 엄격하게 강제됩니다 *(가장 안전)*  
- [ ] 예 — 민감하지 않은 페이지는 HTTP로 접근 가능하지만, 민감한 엔드포인트는 **접근할 수 없습니다**  
- [ ] 예 — 민감한 엔드포인트(로그인, 결제, 프로필 등)가 암호화되지 않은 HTTP로 **접근 가능합니다** *(높은 위험)*  

### HTTP에서 HTTPS로의 전역 리다이렉션 (Redirection)이 존재합니까?
- [ ] 예 — 애플리케이션이 모든 HTTP 요청에 대해 HTTPS로 301/302 리다이렉션을 수행합니다  
- [ ] 예 — 특정 민감한 엔드포인트에 대해서만 리다이렉션이 수행됩니다  
- [ ] 아니요 — HTTP에서 HTTPS로의 리다이렉션이 **적용되지 않았습니다**  

### HTTP 엄격한 전송 보안 (HTTP Strict Transport Security, HSTS)이 구현되어 있습니까?
- [ ] 예 — HSTS가 긴 `max-age`와 함께 **활성화**되어 있으며, `includeSubDomains` 및 `preload` 지시자를 포함합니다  
- [ ] 예 — HSTS가 **활성화**되어 있으나 `includeSubDomains` 또는 `preload` 지시자가 누락되었습니다  
- [ ] 아니요 — 애플리케이션 서버에 HSTS가 **활성화되지 않았습니다**  

### 민감한 쿠키 (Cookies)가 평문 전송으로부터 보호되고 있습니까?

| 쿠키 이름 | Secure 플래그 존재 | Secure 플래그 누락 |
|-------------|:-------------------:|:------------------:|
| `SessionID` |                     |                    |
| `AuthToken` |                     |                    |
| `CSRF-Token`|                     |                    |

### 애플리케이션이 암호화되지 않은 채널을 통해 서드파티 API로 민감한 데이터를 전송합니까?
- [ ] 아니요 — 모든 외부 API 호출이 암호화된(HTTPS) 터널을 통해 수행됩니다  
- [ ] 예 — 일부 민감하지 않은 텔레메트리 데이터가 HTTP를 통해 전송됩니다  
- [ ] 예 — API 키, PII 또는 인증 자격 증명이 암호화되지 않은 HTTP를 통해 서드파티로 **전송됩니다**

---

## WSTG-CRYP-04 — 취약한 암호화 프리미티브(Weak Cryptographic Primitives) 점검

취약한 암호화 프리미티브는 MD5, SHA-1, DES 또는 RC4와 같이 충돌 공격(Collision attacks), 빈도 분석(Frequency analysis) 또는 무차별 대입 암호 해독(Brute-force decryption)에 취약한 오래되거나 안전하지 않은 알고리즘을 사용하는 것을 의미합니다. 이러한 약점은 일반적으로 애플리케이션 백엔드 내의 비밀번호 해싱(Password hashing), 저장 데이터 암호화(Data encryption at rest), 세션 토큰 생성(Session token generation) 또는 디지털 서명 검증(Digital signature verification)에서 발생합니다. 공격자의 관점에서 이러한 프리미티브를 식별하면 가로챈 해시를 크래킹(Cracking)하여 평문 비밀번호를 획득하거나 인증 토큰을 위조하는 등 데이터 무결성 및 기밀성(Data integrity and confidentiality)을 침해할 수 있습니다. 현대적인 애플리케이션은 암호 해독(Cryptanalysis)에 대한 강력한 보호를 보장하기 위해 Argon2, Bcrypt 또는 AES-GCM과 같이 업계 표준이며 충돌 저항성이 있고 엔트로피가 높은 프리미티브를 사용해야 합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CRYP-04 |
| **CWE** | CWE-327 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음(High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/04-Testing_for_Weak_Cryptographic_Primitives  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `hashcat`, `John the Ripper`, `Burp Suite`, `CyberChef`, `OpenSSL`, `nmap (ssl-enum-ciphers)`

### MD5 또는 SHA-1과 같이 권장되지 않는 해싱 알고리즘(Hashing algorithms)이 민감 데이터 저장에 사용됩니까?
- [ ] 아니요 — 애플리케이션은 Argon2 또는 Bcrypt와 같은 현대적인 충돌 방지 알고리즘을 사용함  
- [ ] 예 — 권장되지 않는 알고리즘이 사용되지만, 보안에 민감하지 않은 무결성 검사용으로**만** 사용됨  
- [ ] 예 — 비밀번호 또는 민감한 토큰에 권장되지 않는 알고리즘이 **사용됨** *(High)*  

### DES, 3DES 또는 RC4와 같이 취약한 대칭 암호화 암호(Symmetric encryption ciphers)가 현재 사용되고 있습니까?
- [ ] 아니요 — GCM 또는 CBC와 같은 안전한 모드의 AES(128비트 이상)가 사용됨  
- [ ] 예 — 하위 호환성(Backward compatibility)을 위해 레거시 암호가 **활성화**되어 있지만 기본값은 아님  
- [ ] 예 — 레거시 암호가 저장 데이터 또는 전송 중인 데이터를 보호하는 주요 방법임  

### 애플리케이션이 자체 제작("Roll-your-own") 또는 비표준 암호화 로직을 구현합니까?
- [ ] 아니요 — 애플리케이션은 표준적이고 피어 리뷰를 거친 암호화 라이브러리를 엄격히 준수함  
- [ ] 예 — 사용자 정의 래퍼가 존재하지만 기본 프리미티브는 업계 표준을 **따름**  
- [ ] 예 — 독점적인 암호화 알고리즘 또는 사용자 정의 로직이 민감한 데이터에 **적용됨** *(Critical)*  

### 암호화 솔트(Salts) 및 초기화 벡터(Initialization Vectors, IVs)가 모범 사례에 따라 관리됩니까?
- [ ] 예 — 솔트는 사용자당 고유하며 IV는 모든 작업에 대해 예측 불가능함  
- [ ] 예 — 솔트가 사용되지만 모든 레코드에서 고유하지는 **않음**  
- [ ] 아니요 — 솔트가 정적이거나 하드코딩되어 있거나 없으며, 여러 세션에서 IV가 **재사용됨**  

### 비대칭 키(Asymmetric keys, RSA, Diffie-Hellman)의 비트 길이가 현대 표준에 충분합니까?
- [ ] 예 — RSA/DH 키가 2048비트 이상이거나 타원 곡선 암호(ECC)가 사용됨  
- [ ] 아니요 — 키가 2048비트 미만이지만 우회(Bypass)가 단순하지는 않음  
- [ ] 아니요 — 키가 1024비트 이하로, 인수분해 또는 연산 공격(Factorization or computational attacks)에 **취약함**

---

## WSTG-BUSL-01 — 비즈니스 로직 데이터 검증 테스트(Test Business Logic Data Validation)

비즈니스 로직 데이터 검증 테스트는 구문적으로는 올바르지만 비즈니스 도메인 문맥 내에서 논리적으로 유효하지 않은 데이터를 식별하고 거부하는 애플리케이션의 능력에 초점을 맞춥니다. 공격자는 장바구니의 음수 수량, 미래 날짜 이벤트에 과거 날짜 입력, 또는 매우 큰 정수(Integer) 값과 같은 예상치 못한 값을 제출하여 제약 사항을 우회(Bypass)하고 애플리케이션의 상태를 조작함으로써 이러한 결함을 악용합니다. 이러한 취약점은 주로 서버 측 로직이 사용자 입력 데이터의 문맥적 무결성을 확인하지 못하는 다단계 워크플로우(Multi-step workflows), 결제 프로세스 및 계정 관리 모듈에서 자주 발생합니다. 성공적인 취약점 공격(Exploitation)은 재정적 손실, 무결성 훼손, 그리고 제한된 기능이나 데이터에 대한 비인가 접근으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-01 |
| **CWE** | CWE-20 |
| **테스트 상태** | 수행되지 않음(Not Performed) |
| **심각도** | 높음(High) / 중간(Medium) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/01-Test_Business_Logic_Data_Validation  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**도구:** `Burp Suite (Repeater/Intruder)`, `Postman`, `Python (Requests)`, `OWASP ZAP`

### 애플리케이션이 숫자 입력값에 대해 논리적인 범위 제한을 강제합니까?
- [ ] 예 — 애플리케이션이 부적절한 음수, 0 또는 과도하게 큰 값을 올바르게 거부함 *(가장 안전)*  
- [ ] 예 — 애플리케이션이 일부 잘못된 범위를 거부하지만, 정수 오버플로(Integer Overflow) 또는 경계 우회에 취약함  
- [ ] 아니요 — 애플리케이션이 비즈니스 문맥과 관계없이 모든 숫자 값을 수용함 *(치명적)*  

### 서버 측 검증 체크가 애플리케이션의 모든 계층에서 일관되게 적용됩니까?
- [ ] 예 — API/서비스 및 데이터베이스 계층 모두에서 검증이 일관되게 적용됨  
- [ ] 예 — UI/프런트엔드에서 검증이 적용되지만, 직접적인 API 요청을 통한 우회가 가능함  
- [ ] 아니요 — 검증이 적용되지 않거나 일관성이 없어 논리적인 데이터 손상(Data Corruption)이 발생할 수 있음  

### 다단계 워크플로우에서 데이터를 조작하여 비즈니스 로직을 무력화할 수 있습니까?
- [ ] 아니요 — 상태가 서버에서 안전하게 유지되며 이전 단계의 데이터를 변조할 수 없음  
- [ ] 예 — 초기 단계에서 검증이 수행되지만, 최종 제출 시 데이터의 무결성을 재검증하지 않음  
- [ ] 예 — 단계를 건너뛰거나 순서가 맞지 않는 데이터를 제출하여 워크플로우 우회가 가능함  

### 애플리케이션이 비즈니스 제약 조건을 우회하는 "에지 케이스(Edge case)" 데이터를 적절히 처리합니까?
- [ ] 예 — 제약 조건이 견고하며 구문적으로는 유효하지만 특이한 입력(예: 윤년, 시간대 변경 등)을 적절히 처리함  
- [ ] 예 — 일부 에지 케이스는 처리되지만, 특정 조합이 예상치 못한 동작으로 이어질 수 있음  
- [ ] 아니요 — 에지 케이스가 무료 구매 또는 권한 없는 상태 변경과 같은 로직 실패로 직접 연결됨

---

## WSTG-BUSL-02 — 요청 위조 능력 테스트 (Test Ability to Forge Requests)

비즈니스 로직(Business Logic) 컨텍스트 내에서 요청을 위조하는 것은 의도된 워크플로 또는 권한 수준 외부의 작업을 수행하기 위해 애플리케이션에 정의된 파라미터(Parameter)를 조작하는 것을 포함합니다. 공격자는 서버가 신뢰할 수 있는 백엔드 소스와 비교하여 재검증하는 데 실패하는 클라이언트 측 파라미터를 통해 상태가 유지되는 결제 시스템이나 계정 등록과 같은 다단계 프로세스를 공략합니다. 숨겨진 필드, JSON 값 또는 가격, 수량, 내부 상태 플래그와 같은 REST 파라미터를 변경함으로써 공격자는 결제 요구 사항을 우회하거나 권한 상승(Privilege Escalation)을 시도하고, 의도하지 않은 상태 변경을 유도할 수 있습니다. 이러한 취약점은 서버가 트랜잭션 도중 비즈니스 상태에 대한 클라이언트의 표현을 암시적으로 신뢰하는 상태 유지형 웹 폼 또는 API에서 주로 발생합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-02 |
| **CWE** | CWE-602 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical)* |

> *위조된 요청으로 인해 재정적 손실, 승인되지 않은 관리자 작업 또는 전체 계정 탈취가 가능해질 경우 위험도는 '치명적(Critical)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/02-Test_Ability_to_Forge_Requests  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Proxy, Repeater, Intruder)`, `Postman`, `OWASP ZAP`, `CyberChef`

### 애플리케이션이 서버 측 "신뢰할 수 있는 정보원(Source of Truth)"에 대해 트랜잭션 파라미터를 검증합니까?
- [ ] 예 — 모든 민감한 파라미터(가격, 역할, ID)가 데이터베이스에 대해 검증되며 우회가 **불가능함**  
- [ ] 예 — 검증이 적용되었으나 파라미터 오염(Parameter Pollution) 또는 대체 인코딩을 통해 우회 가능함  
- [ ] 아니요 — 애플리케이션이 트랜잭션의 비용이나 결과를 결정하기 위해 클라이언트 측 값을 신뢰함 *(치명적)*  

### 비즈니스 크리티컬한 값(예: 수량, 금액)을 승인되지 않은 값으로 수정할 수 있습니까?
- [ ] 아니요 — 음수, 0 또는 지나치게 높은 값은 서버에서 거부됨  
- [ ] 예 — 서버가 수정된 값을 수용하지만 제한된 범위 내에서만 허용됨  
- [ ] 예 — 음수 또는 0의 값을 제출할 수 있으며, 이로 인해 재정적 또는 로직 우회가 발생함  

### 다단계 프로세스 전체에서 상태를 유지하기 위해 숨겨진 필드나 API 파라미터가 사용됩니까?
- [ ] 아니요 — 상태가 서버 측 세션 또는 서명된 토큰을 통해 안전하게 유지됨  
- [ ] 예 — 상태가 클라이언트를 통해 전달되지만 무결성 검사(HMAC/서명)가 **활성화**되어 있고 견고함  
- [ ] 예 — 상태가 클라이언트를 통해 전달되며 무결성 검사가 **비활성화**되어 있거나 우회 가능함  

### 워크플로에서 필수적인 중간 단계를 건너뛰는 요청을 위조하는 것이 가능합니까?
- [ ] 아니요 — 서버가 모든 트랜잭션에 대해 엄격한 작업 순서를 강제함  
- [ ] 예 — 단계를 건너뛸 수 **있으나** 최종 작업에는 여전히 이전 단계의 유효한 데이터가 필요함  
- [ ] 예 — 최종 "제출" 또는 "완료" 요청을 직접 위조하여 권한 부여 또는 결제 단계를 우회할 수 **있음**

---

## WSTG-BUSL-03 — 무결성 검사 테스트 (Test Integrity Checks)

무결성 검사 (Integrity Check) 테스트는 애플리케이션이 다양한 비즈니스 프로세스를 거치거나 클라이언트와 서버 사이를 이동하는 데이터의 변조 여부를 확인하고, 데이터가 신뢰할 수 있는 상태로 유지되는지 검증하는 데 중점을 둡니다. 공격자는 HTTP 요청, 숨겨진 필드 (Hidden fields) 또는 쿠키 (Cookies) 내의 제품 가격, 수량, 사용자 권한 또는 트랜잭션 상태와 같은 중요한 파라미터 (Parameters)를 조작하여 이러한 로직을 공격합니다. 애플리케이션에 서버 측 (Server-side) 검증이나 해싱 (Hashing) 또는 HMAC과 같은 강력한 무결성 제어 수단이 부족한 경우, 공격자는 이러한 값을 조작하여 사기를 치거나 자신의 권한을 상승 (Privilege Escalation)시키거나 의도된 비즈니스 제약을 우회할 수 있습니다. 이 테스트는 금융 거래, 민감한 상태 전환 또는 한 단계의 출력이 다음 단계의 신뢰할 수 있는 입력으로 사용되는 다단계 워크플로우 (Workflow)를 처리하는 모든 애플리케이션에서 매우 중요합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-03 |
| **CWE** | CWE-353 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *무결성 실패로 인해 금융 자산 탈취, 무단 권한 상승 또는 영구 기록의 수정이 가능한 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/03-Test_Integrity_Checks  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Proxy/Repeater)`, `CyberChef`, `Postman`, `Python (Requests)`

### 민감한 비즈니스 파라미터가 클라이언트 측 변조에 노출되어 있습니까?
- [ ] 아니오 — 민감한 값은 서버 측에서 전적으로 관리됩니다.
- [ ] 예 — 가격, 수량 또는 상태 플래그와 같은 값이 노출되어 있으나 암호화되어 있습니다.
- [ ] 예 — 값이 숨겨진 필드, URL 파라미터 또는 쿠키 내에 평문(Plain text)으로 노출되어 있습니다.

### 클라이언트가 제어하는 파라미터에 무결성 메커니즘(예: HMAC, 체크섬)이 적용되어 있습니까?
- [ ] 예 — 강력한 무결성 검사가 적용되어 있으며 모든 요청마다 검증됩니다.
- [ ] 예 — 무결성 검사가 적용되어 있으나 취약하거나 예측 가능한 알고리즘 또는 하드코딩된 비밀 키를 사용합니다.
- [ ] 아니오 — 민감한 파라미터에 무결성 검사가 적용되어 있지 않습니다.

### 공격자가 무결성 검사를 우회하거나 재계산할 수 있습니까?
- [ ] 아니오 — 서명 검증이 엄격하게 적용되어 우회가 불가능합니다.
- [ ] 예 — 서명 파라미터를 누락시킴으로써 서명 검증을 우회할 수 있습니다.
- [ ] 예 — 비밀 키나 로직이 노출되었거나 취약하여 서명을 재계산할 수 있습니다.

### 애플리케이션이 신뢰할 수 있는 소스를 바탕으로 백엔드에서 데이터를 재검증합니까?
- [ ] 예 — 서버가 데이터베이스와 대조하여 모든 값을 재검증하며 우회가 불가능합니다.
- [ ] 예 — 서버가 부분적인 검증을 수행하지만 특정 예외 케이스를 통해 우회가 가능합니다.
- [ ] 아니오 — 서버가 백엔드 (Backend) 검증 없이 클라이언트가 제공한 데이터를 신뢰합니다 *(Critical)*.

### 다단계 프로세스의 상태를 변조하는 것이 가능합니까?
- [ ] 아니오 — 세션 상태가 서버 측에서 관리되며 조작할 수 없습니다.
- [ ] 예 — 상태 정보가 클라이언트를 통해 전달되며, 단계를 건너뛰거나 작업을 반복하기 위한 변조가 가능합니다.

---

## WSTG-BUSL-04 — 프로세스 타이밍 테스트 (Process Timing)

프로세스 타이밍 공격 (Process Timing Attack)은 특정 요청을 처리하는 데 걸리는 시간을 측정하여 내부 상태나 데이터 존재 여부에 대한 민감한 정보를 유추하는 기법입니다. 조기 종료 조건 로직 (Early-exit conditional logic), 데이터베이스 조회 (Database lookup), 또는 비고정 시간 암호화 비교 (Non-constant-time cryptographic comparison)로 인해 발생하는 밀리초 단위의 응답 시간 차이를 분석함으로써, 공격자는 부채널 분석 (Side-channel analysis)을 수행하여 유효한 사용자 이름을 열거하거나 비밀번호 조각을 검증하고 특정 레코드의 존재를 확인할 수 있습니다. 이러한 취약점은 주로 인증 엔드포인트 (Authentication endpoint), 비밀번호 초기화 폼, 그리고 입력값의 유효성에 따라 백엔드 로직이 분기되는 리소스 집약적인 API 필터 (Resource-heavy API filter)에서 발생합니다. 공격자의 관점에서 이러한 타이밍 차이는 애플리케이션이 사용자에게 동일하고 일반적인 오류 메시지를 반환하더라도 백엔드 데이터에 대한 진위를 드러내는 ‘불리언 오라클 (Boolean oracle)’ 역할을 합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 (Medium) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**도구:** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### 애플리케이션이 유효한 리소스 요청과 유효하지 않은 리소스 요청에 대해 상이한 응답 시간을 보입니까?
- [ ] 아니오 — 입력값의 유효성과 관계없이 응답 시간이 동일함  
- [ ] 예 — 미세한 타이밍 차이가 존재하지만 통계적으로 유의미하지 않음  
- [ ] 예 — 일관된 타이밍 차이가 관찰되며 신뢰할 수 있는 오라클을 제공함  

### 응답 시간을 정규화하기 위해 고정 시간 비교 함수 (Constant-time comparison function) 또는 인위적인 지연이 구현되어 있습니까?
- [ ] 예 — 모든 민감한 비교 작업에 고정 시간 알고리즘이 적용됨 *(가장 안전)*  
- [ ] 예 — 인위적인 지연 또는 지터 (Jitter)가 활성화되어 있으나, 기본 로직은 여전히 비고정 시간 방식임  
- [ ] 아니오 — 타이밍 완화 조치가 적용되지 않아 로직 분기를 직접 측정할 수 있음  

### 공격자가 통계적 분석을 사용하여 네트워크 노이즈로부터 타이밍 신호를 분리할 수 있습니까?
- [ ] 아니오 — 네트워크 지터 및 애플리케이션 노이즈로 인해 타이밍 분석이 불가능함  
- [ ] 예 — 충분한 샘플 크기가 확보되면 노이즈로부터 타이밍 신호를 분리할 수 있음  
- [ ] 예 — 타이밍 델타 (Delta)가 충분히 커서 통계적 정규화가 필요하지 않음  

### 로그인 또는 비밀번호 초기화 기능을 통해 계정 열거 (Account enumeration)가 가능합니까?
- [ ] 아니오 — 타이밍을 통해 계정 존재 여부를 확인할 수 없음  
- [ ] 예 — 타이밍 차이를 통해 원격 공격자가 유효한 사용자 이름 또는 이메일을 열거할 수 있음  

### 리소스 집약적인 작업(예: 파일 처리, 복잡한 검색)이 타이밍을 통해 데이터 존재 여부를 유출합니까?
- [ ] 아니오 — 레코드 발견 여부와 관계없이 리소스 소비가 일정함  
- [ ] 예 — 백엔드 처리 시간을 측정하여 민감한 레코드의 존재 여부를 유추할 수 있음

---

## WSTG-BUSL-05 — 기능 사용 횟수 제한 테스트 (Test Number of Times a Function Can Be Used Limits)

이 테스트는 애플리케이션이 단일 사용자, 세션 또는 IP 주소에 의해 특정 작업이나 기능이 실행될 수 있는 빈도 및 총 횟수에 대해 비즈니스 로직(Business Logic) 제약 조건을 적용하는지 평가합니다. 이러한 제한을 구현하지 못하면 공격자가 SMS OTP(One-Time Password) 발송, 비밀번호 재설정 이메일 트리거, 할인 코드 적용 또는 대용량 데이터 내보내기와 같은 고가치 기능을 남용할 수 있게 되어 재정적 손실, 리소스 고갈 또는 평판 저하로 이어질 수 있습니다. 이러한 취약점은 일반적으로 트랜잭션 엔드포인트(Transactional Endpoints)나 통신 기능에서 발견되며, 의도된 비즈니스 임계값을 훨씬 초과하여 작업을 반복하는 자동화된 스크립트를 통해 악용됩니다. 공격자 관점에서 이는 명시적인 속도 제한(Rate Limiting) 또는 횟수 기반 스로틀(Throttles)이 부족한 SMS 펌핑(SMS Pumping), 메일 폭격(Mail Bombing) 또는 브루트 포스(Brute Force) 워크플로우의 주요 표적이 됩니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-05 |
| **CWE** | CWE-799 |
| **테스트 상태** | 수행되지 않음 |
| **위험도 (Severity)** | 중간 (Medium) / 높음 (High)* |

> *기능이 금융 거래, SMS 비용과 관련되거나 시스템 전체의 가용성에 영향을 미치는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/05-Test_Number_of_Times_a_Function_Can_Be_Used_Limits  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Repeater)`, `Turbo Intruder`, `Python (requests)`, `OWASP ZAP`

### 애플리케이션이 민감한 비즈니스 기능에 대해 제한을 정의하고 강제합니까?
- [ ] 예 — 제한이 엄격하게 강제되며 우회(Bypass)가 **불가능함**  
- [ ] 예 — 제한이 강제되지만 남용을 방지하기에는 너무 높음  
- [ ] 아니요 — 기능 실행에 아무런 제한이 적용되지 않음  

### 속도 제한 및 사용량 카운터가 서버 측(Server-side)에서 강제됩니까?
- [ ] 예 — 강제가 엄격히 서버 측에서 이루어지며 조작할 수 **없음**  
- [ ] 아니요 — 강제가 클라이언트 측(Client-side) 로직(예: JavaScript 또는 숨겨진 필드)에 의존함  
- [ ] 아니요 — 어느 수준에서도 강제가 존재하지 않음  

### 헤더 또는 세션 조작을 통해 사용 제한을 우회할 수 있습니까?
- [ ] 아니요 — `X-Forwarded-For`, `Origin` 또는 세션 로테이션(Session Rotation)을 통한 우회가 **불가능함**  
- [ ] 예 — IP 헤더 스푸핑(Spoofing) 또는 쿠키 삭제를 통해 제한을 우회할 수 **있음**  
- [ ] 예 — 여러 계정 또는 세션을 생성하여 제한을 우회할 수 **있음**  

### 제한을 초과했을 때 적절한 방어 응답이 트리거됩니까?
- [ ] 예 — 애플리케이션이 `429 Too Many Requests`를 반환하고 이벤트를 로그에 기록함 *(가장 안전)*  
- [ ] 예 — 애플리케이션이 오류를 반환하지만 잠재적인 남용을 로그에 기록하지 **않음**  
- [ ] 아니요 — 애플리케이션이 요청을 계속 처리하지만 출력은 무시함  
- [ ] 아니요 — 애플리케이션이 응답을 제공하지 않거나 부하 상황에서 중단됨  

### 기능 남용의 영향이 비즈니스에 중대합니까?
- [ ] 아니요 — 기능 남용이 비용이나 리소스에 미치는 영향이 미미함  
- [ ] 예 — 남용이 경미한 리소스 소비 또는 사용자 불편을 초래할 수 **있음**  
- [ ] 예 — 남용이 상당한 재정적 비용(예: SMS 요금) 또는 서비스 거부(Denial of Service)로 이어질 수 **있음** *(치명적)*

---

## WSTG-BUSL-06 — 워크플로 우회 테스트 (Testing for the Circumvention of Work Flows)

워크플로 우회(Circumvention of Work Flows)는 다단계 프로세스 내에서 의도된 순서나 전제 조건을 건너뛰기 위해 애플리케이션 로직을 조작하는 행위를 포함합니다. 공격자는 결제 확인, 관리자 승인 또는 다요소 인증(MFA) 완료 화면과 같은 민감한 엔드포인트(Endpoint)를 식별하고, 필수 중간 단계를 건너뛰어 해당 단계에 직접 접근을 시도합니다. 이 취약점은 서버 측 로직이 견고한 상태 머신(State Machine)을 유지하지 못하고, 사용자가 UI 중심의 경로를 따를 것이라는 가정에만 의존할 때 발생합니다. 성공적인 공격은 미승인 트랜잭션, 권한 상승(Privilege Escalation), 신원 확인 메커니즘 우회 등 치명적인 비즈니스 로직 결함으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-06 |
| **CWE** | CWE-841 |
| **테스트 상태** | 미수행 |
| **심각도** | 중간 / 높음* |

> *우회를 통해 미승인 금융 트랜잭션, 권한 상승 또는 관리자 접근이 가능한 경우 심각도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/06-Testing_for_the_Circumvention_of_Work_Flows  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/logic-flaws  

**도구:** `Burp Suite (Repeater/Intruder)`, `OWASP ZAP`, `Postman`, `Caido`

### 애플리케이션에 다단계 비즈니스 프로세스가 포함되어 있습니까?
- [ ] 아니오 — 애플리케이션이 단일 요청 작업으로만 구성되어 있음  
- [ ] 예 — 다단계 프로세스(예: 장바구니, 위저드 폼, 회원가입 등)가 존재함  

### 단계별 순서가 서버에 의해 엄격하게 강제됩니까?
- [ ] 예 — 서버 측 상태 추적을 통해 단계를 건너뛸 수 없도록 보장함  
- [ ] 예 — 클라이언트 측 통제는 순서를 제시하지만, 서버는 진행 상황을 검증하지 않음  
- [ ] 아니오 — 애플리케이션이 특정 작업 순서를 강제하지 않음  

### 공격자가 최종 단계의 URL이나 엔드포인트를 직접 요청하여 프로세스의 마지막 단계에 도달할 수 있습니까?
- [ ] 아니오 — 이전 단계의 완료 없이는 최종 단계에 대한 직접 접근이 불가능함  
- [ ] 예 — 최종 단계(예: `/api/v1/checkout/complete`)에 직접 요청을 통해 도달할 수 있음  

### 애플리케이션이 이전 단계의 필수 데이터가 존재하고 유효한지 확인합니까?
- [ ] 예 — 서버는 진행하기 전에 모든 이전 단계의 데이터를 검증함  
- [ ] 아니오 — 데이터가 예상되지만 검증되지 않는 단계를 "건너뛰는" 공격에 취약함  

### 워크플로 조작을 통해 MFA나 신원 확인과 같은 보안 통제를 우회할 수 있습니까?
- [ ] 아니오 — 흐름 조작을 통해 중요 통제를 우회할 수 없음  
- [ ] 예 — 검증 후 단계로 바로 이동함으로써 MFA나 신원 확인 우회가 가능함

---

## WSTG-BUSL-07 — 애플리케이션 오용 방어 테스트 (Test Defenses Against Application Misuse)

애플리케이션 오용(Misuse)에 대한 방어는 의도된 비즈니스 로직(Business Logic)에서 벗어나는 비정상적인 사용자 행위를 식별하고, 로깅(Logging)하며, 이에 대응하는 애플리케이션의 능력을 평가합니다. 전통적인 시그니처 기반 보안(Signature-based Security)과 달리, 이러한 방어 체계는 자동화된 남용을 의미하는 급격한 요청, 크리덴셜 스터핑(Credential Stuffing) 또는 순차적 리소스 열거와 같은 패턴을 탐지하는 데 집중합니다. 공격자의 관점에서 이 테스트는 표준 보안 알림을 트리거하지 않고 데이터를 유출하거나 리소스를 고갈시키도록 설계된 자동화(Automation) 및 "Low and Slow" 공격에 대한 애플리케이션의 복원력을 결정합니다. 이러한 방어 체계를 구현하지 못하면 대규모 데이터 스크레이핑(Data Scraping), 계정 탈취(Account Takeover) 또는 합법적이지만 리소스 집약적인 애플리케이션 기능을 통한 서비스 거부(Denial of Service - DoS)가 발생할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-07 |
| **CWE** | CWE-799 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/07-Test_Defenses_Against_Application_Misuse  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite (Intruder/Turbo Intruder)`, `OWASP ZAP`, `Python (Requests/Selenium)`, `Caido`, `K6`

### 민감한 엔드포인트(Endpoint)에 전송 속도 제한(Rate Limiting) 또는 스로틀링(Throttling) 메커니즘이 적용되어 있습니까?
- [ ] 예 — 엄격한 Rate Limiting이 **적용되어 있으며** 모든 민감한 엔드포인트에 대해 강제됩니다. *(가장 안전)*  
- [ ] 예 — Rate Limiting이 **적용되어 있으나** IP 교체(Rotation) 또는 헤더 조작을 통해 우회가 가능합니다.  
- [ ] 예 — 인증 엔드포인트에만 Rate Limiting이 **적용되어** 있으며, 다른 엔드포인트는 노출되어 있습니다.  
- [ ] 아니요 — 어떤 애플리케이션 기능에도 Rate Limiting 또는 Throttling이 **적용되지 않았습니다.**  

### 애플리케이션이 자동화된 상호작용 패턴을 탐지하고 차단합니까?
- [ ] 예 — 행위 분석 또는 CAPTCHA가 **활성화되어 있으며** 봇(Bot)을 효과적으로 차단합니다.  
- [ ] 예 — 자동화 방지 기능이 **활성화되어 있으나** 헤드리스 브라우저(Headless Browser) 또는 세션 사이클링을 통한 우회가 **가능합니다.**  
- [ ] 아니요 — 애플리케이션이 실제 사용자와 자동화된 스크립트를 **구분할 수 없습니다.**  

### 비정상 행위가 로깅되고 방어적 대응이 뒤따릅니까?
- [ ] 예 — 오용 탐지 시 자동 차단이 트리거되며 보안 팀에 알림이 전송됩니다.  
- [ ] 예 — 감사를 위해 오용이 로깅되지만 자동화된 방어 조치는 **취해지지 않습니다.**  
- [ ] 아니요 — 애플리케이션이 특이한 활동 패턴을 로깅하거나 이에 **대응하지 않습니다.**  

### 클라이언트 측 식별자 또는 헤더를 조작하여 방어 체계를 우회할 수 있습니까?
- [ ] 아니요 — 방어 체계가 서버 측 상태 및 검증된 원격 측정(Telemetry) 데이터에 의존합니다.  
- [ ] 예 — 쿠키를 삭제하거나 `User-Agent` 문자열을 교체하여 통제 항목을 **우회할 수 있습니다.**  
- [ ] 예 — `X-Forwarded-For` 또는 `X-Real-IP`와 같은 헤더를 삽입하여 통제 항목을 **우회할 수 있습니다.**

---

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

## WSTG-BUSL-10 — 결제 기능 테스트 (Test Payment Functionality)

결제 기능 테스트는 공격자가 결제 게이트웨이(Payment Gateways)를 우회하거나, 주문 합계를 조작하거나, 성공적인 트랜잭션 신호를 스푸핑(Spoofing)할 수 있게 하는 비즈니스 로직 결함(Business Logic Flaws)을 식별하는 작업을 포함합니다. 이러한 취약점은 일반적으로 애플리케이션, 클라이언트 브라우저 및 Stripe, PayPal과 같은 서드파티 결제 프로세서 또는 내부 원장 시스템 간의 통신에서 발생합니다. 공격자는 전송 중인 가격 파라미터(Parameter)를 수정하거나, 총 비용을 줄이기 위해 음수 수량을 제출하거나, 실제 결제 없이 주문을 이행하기 위해 성공적인 트랜잭션 콜백(Callbacks)을 재전송(Replay)하려고 시도할 수 있습니다. 성공적인 공격은 직접적인 재정적 손실, 재고 불일치 및 애플리케이션의 전자상거래 무결성 훼손으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-BUSL-10 |
| **CWE** | CWE-840 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음(High) / 치명적(Critical) |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/10-Test-Payment-Functionality  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `Turbo Intruder`, `Postman`, `Hackvertor`

### 처리 전 서버 측에서 거래 금액이 검증됩니까?
- [ ] 예 — 금액이 서버 측 상품 데이터베이스를 기준으로 엄격하게 검증됨 *(가장 안전)*  
- [ ] 예 — 금액은 검증되나 파라미터 오염(Parameter Pollution) 또는 통화 전환을 통한 로직 우회가 가능함  
- [ ] 아니요 — 클라이언트 측 요청에서 거래 금액을 수정할 수 있으며 백엔드에서 이를 수락함  

### 합계 금액이 0 또는 음수가 되도록 품목 수량을 조작할 수 있습니까?
- [ ] 아니요 — 애플리케이션 검증 로직에 의해 음수 또는 0의 수량이 거부됨  
- [ ] 예 — 장바구니에서 음수 수량이 허용되지만 체크아웃 시 합계가 수정됨  
- [ ] 예 — 음수 수량을 사용하여 전체 주문 합계를 줄이거나 크레딧을 얻을 수 있음 *(치명적)*  

### 애플리케이션이 결제 게이트웨이 콜백 또는 웹훅(Webhooks)을 안전하게 처리합니까?
- [ ] 예 — 콜백에 스푸핑이 불가능한 유효한 암호화 서명이 필요함  
- [ ] 예 — 서명을 확인하지만 비밀 키가 취약하거나 검증을 우회할 수 있음  
- [ ] 아니요 — 애플리케이션이 결제 상태를 확인하기 위해 인증되지 않거나 검증되지 않은 콜백에 의존함  

### 체크아웃 또는 결제 단계에서 경쟁 상태(Race Conditions) 취약점이 존재합니까?
- [ ] 아니요 — 트랜잭션 무결성 및 데이터베이스 잠금(Locking) 메커니즘이 활성화되어 있음  
- [ ] 예 — 경쟁 상태가 가능하지만 최종 가격이나 주문 이행에 영향을 미치지 않음  
- [ ] 예 — 동시 요청을 통해 단일 결제에 대해 여러 번 주문이 이행되거나 기프트 카드의 "이중 지불"이 가능함  

### 할인 코드나 로열티 포인트가 조작 또는 재사용에 노출되어 있습니까?
- [ ] 아니요 — 코드는 1회용으로 검증되며 로직 제약 조건이 적용됨  
- [ ] 예 — 코드를 재사용하거나 부적격 품목에 적용할 수 있으나 영향이 제한적임  
- [ ] 예 — 공격자가 상충하는 여러 코드를 적용하거나 만료 및 사용 제한을 우회할 수 있음

---

## WSTG-CLNT-01 — DOM 기반 크로스 사이트 스크립팅 테스트

DOM 기반 크로스 사이트 스크립팅 (DOM XSS)은 애플리케이션에 포함된 클라이언트 측 자바스크립트(JavaScript)가 신뢰할 수 없는 소스(Source)로부터 전달된 데이터를 안전하지 않은 방식으로 처리하여, 위험한 싱크(Sink)를 통해 문서 객체 모델 (DOM)에 데이터를 다시 기록할 때 발생합니다. 반사형(Reflected) 또는 저장형(Stored) XSS와 달리, 이 취약점은 전적으로 클라이언트 측 코드 내에 존재합니다. 페이로드(Payload)는 `innerHTML`, `eval()`, 또는 `document.write()`와 같은 싱크에 의해 처리되므로 서버로 전송되지 않는 경우가 많습니다. 공격자는 악의적인 프래그먼트나 파라미터가 포함된 URL을 제작하여 이를 악용하며, 피해자의 브라우저에서 해당 URL이 로드되면 사용자 세션 컨텍스트에서 임의의 자바스크립트가 실행됩니다. 이는 세션 토큰(Session Tokens) 유출, 민감한 데이터 탈취, 그리고 인증된 사용자를 대신하여 수행되는 무단 행위로 이어질 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**도구:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### 자바스크립트에서 사용자 제어 데이터를 캡처하기 위해 신뢰할 수 없는 소스를 사용합니까?
- [ ] 아니오 — 자바스크립트가 `location.hash`, `location.search`, 또는 `document.referrer`를 사용하지 않습니다.  
- [ ] 예 — 신뢰할 수 없는 소스가 사용되지만 데이터가 실행 싱크로 전달되지 않습니다.  
- [ ] 예 — 신뢰할 수 없는 소스가 사용되며 데이터가 클라이언트 측 로직으로 직접 전달됩니다.  

### 사용자 제어 데이터가 위험한 DOM 싱크로 전달됩니까?
- [ ] 아니오 — `innerHTML`, `outerHTML`, `document.write()`, 또는 `eval()`과 같은 싱크가 사용되지 않습니다.  
- [ ] 예 — 위험한 싱크가 존재하지만, `DOMPurify`와 같은 보안 라이브러리를 사용하여 데이터를 엄격하게 새니타이제이션(Sanitization)합니다.  
- [ ] 예 — 위험한 싱크가 존재하며 데이터가 불충분하거나 사용자 정의 정규식 기반 필터링으로 처리됩니다.  
- [ ] 예 — 위험한 싱크가 존재하며 데이터가 새니타이제이션 또는 인코딩(Encoding) 없이 전달됩니다. *(치명적)*  

### URL 기반 페이로드를 통해 실행 싱크에 도달할 수 있습니까?
- [ ] 아니오 — 외부 입력을 통해 소스에서 싱크로의 흐름(Source-to-sink flow)을 유도할 수 없습니다.  
- [ ] 예 — 흐름은 가능하지만 복잡한 사용자 상호작용이 필요합니다.  
- [ ] 예 — 흐름이 가능하며 단순한 URL 프래그먼트나 쿼리 파라미터를 통해 유도될 수 있습니다.  

### 현대적인 보안 헤더 또는 브라우저 기반 완화 조치가 위험을 중화하고 있습니까?
- [ ] 예 — `script-src` 및 `trusted-types`가 포함된 엄격한 콘텐츠 보안 정책 (CSP)이 활성화되어 실행을 방지합니다.  
- [ ] 예 — CSP가 존재하지만 `unsafe-inline` 또는 취약한 해시(Hash)로 인해 우회(Bypass)가 가능합니다.  
- [ ] 아니오 — DOM 기반 실행을 완화할 CSP가 존재하지 않습니다.  

### 애플리케이션이 보호 기능이 내장된 클라이언트 측 프레임워크를 사용합니까?
- [ ] 예 — 프레임워크(예: Angular, React)가 데이터를 자동으로 인코딩하며 우회가 불가능합니다.  
- [ ] 예 — 프레임워크를 사용하지만 `dangerouslySetInnerHTML`과 같은 "이스케이프 해치(Escape hatches)"가 활성화되어 있습니다.  
- [ ] 아니오 — 애플리케이션이 자동 이스케이프 기능이 없는 바닐라 자바스크립트(Vanilla JavaScript) 또는 레거시 라이브러리(예: 구버전 jQuery)를 사용합니다.

---

## WSTG-CLNT-02 — 자바스크립트 실행 테스트 (Testing for JavaScript Execution)

자바스크립트 실행(JavaScript Execution) 테스트는 애플리케이션이 사용자 제공 데이터를 부적절하게 처리하여 피해자의 브라우저 컨텍스트 내에서 승인되지 않은 스크립트가 실행되는 사례를 식별하는 데 중점을 둡니다. 이 취약점은 일반적으로 교차 사이트 스크립팅(Cross-Site Scripting, XSS)으로 이어지며, 공격자가 세션 쿠키(Session Cookies)를 탈취하거나, 문서 객체 모델(Document Object Model, DOM)을 조작하거나, 사용자를 대신하여 특정 동작을 수행할 수 있게 합니다. 모의 침투 테스터는 URL 프래그먼트(URL Fragments), `localStorage`, 또는 `window.name`과 같은 소스(Sources)로부터 전달된 검증되지 않은 데이터가 `innerHTML`, `document.write()`, `eval()`과 같은 위험한 싱크(Sinks)에서 처리되는지 조사합니다. 성공적인 취약점 공격(Exploit)은 사용자 세션의 무결성과 기밀성을 훼손하며, 더 복잡한 클라이언트 측 공격(Client-side attacks)을 위한 교두보 역할을 할 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **테스트 상태** | Not Performed |
| **위험도** | High |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**도구:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### 애플리케이션이 위험한 자바스크립트 싱크에서 사용자 제공 데이터를 처리합니까?
- [ ] 아니오 — 모든 데이터가 정제되거나 `textContent`와 같은 안전한 싱크에서 사용됨  
- [ ] 예 — 데이터가 위험한 싱크에서 처리되지만, 새니타이징(Sanitization) 또는 인코딩이 **적용되어 있음**  
- [ ] 예 — 데이터가 위험한 싱크에서 처리되며, 새니타이징이 **적용되어 있지 않음**  

### 스크립트 실행을 완화하기 위한 콘텐츠 보안 정책(Content Security Policy, CSP)이 존재합니까?
- [ ] 예 — 엄격한 CSP가 **활성화되어 있으며** 우회가 **불가능함**  
- [ ] 예 — CSP가 **활성화되어 있으나** `unsafe-inline` 또는 안전하지 않은 CDN을 통해 우회가 **가능함**  
- [ ] 아니오 — CSP가 **활성화되어 있지 않음**  

### URL 파라미터나 프래그먼트를 통해 자바스크립트를 실행할 수 있습니까 (DOM 기반 XSS)?
- [ ] 아니오 — 입력값이 적절히 인코딩되어 실행이 **불가능함**  
- [ ] 예 — 입력값이 DOM에 반영되지만, 현대적인 프레임워크의 보호 기능에 의해 실행이 **차단됨**  
- [ ] 예 — 입력값이 브라우저에서 직접 실행되며, 취약점 공격이 **가능함**  

### 이벤트 핸들러(Event handlers)나 URI 스킴(URI schemes)이 스크립트 삽입 공격에 취약합니까?
- [ ] 아니오 — 이벤트 속성과 `javascript:` 스킴을 제거하도록 입력값이 정제됨  
- [ ] 예 — 이벤트 핸들러를 삽입할 수 있으나, 필터로 인해 페이로드(Payload)의 복잡성이 제한됨  
- [ ] 예 — `onerror`, `onload` 또는 `href` 속성을 통한 임의의 자바스크립트 실행이 **가능함**

---

## WSTG-CLNT-03 — HTML 인젝션(HTML Injection)

HTML 인젝션(HTML Injection)은 애플리케이션이 사용자 제공 입력을 브라우저에 렌더링하기 전에 적절히 정제(Sanitize)하지 않을 때 발생하며, 공격자가 웹 페이지의 문서 객체 모델(Document Object Model, DOM) 내에 임의의 HTML 태그를 삽입할 수 있게 합니다. 교차 사이트 스크립팅(Cross-Site Scripting, XSS)과 유사하지만, HTML 인젝션의 주요 목적은 페이지의 시각적 표현을 조작하거나 악성 폼(Form) 및 기만적인 콘텐츠를 삽입하여 피싱(Phishing) 공격을 용이하게 하는 것입니다. 공격자는 검색어, 사용자 프로필 필드 또는 에러 메시지와 같이 UI에 반영되는 파라미터를 표적으로 삼아, 피해자가 민감한 자격 증명(Credentials)을 공개하거나 승인되지 않은 제3자 링크와 상호 작용하도록 유도합니다. 이 취약점은 애플리케이션 사용자 인터페이스의 무결성에 중대한 위험을 초래하며, 더 복잡한 사회 공학(Social Engineering) 공격이나 세션 관련 공격의 전조가 될 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-03 |
| **CWE** | CWE-80 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 낮음 / 중간 |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/03-Testing_for_HTML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `OWASP ZAP`, `DOM Invader`, `Wfuzz`

### 사용자 제어 입력이 적절한 HTML 인코딩(HTML Encoding) 없이 DOM에 반영됩니까?
- [ ] 아니요 — 모든 반영된 입력은 렌더링되기 전에 엄격하게 HTML 인코딩됩니다.
- [ ] 예 — 일부 문자는 인코딩되지만 특정 태그에 대한 우회(Bypass)가 가능합니다.
- [ ] 예 — 입력이 원본 그대로 반영되며 HTML 인젝션이 가능합니다.

### 페이지 레이아웃을 변경하기 위해 구조적 HTML 요소를 삽입할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 `<div>`, `<table>`, 또는 `<iframe>`과 같은 구조적 태그를 필터링하거나 제거합니다.
- [ ] 예 — 구조적 요소가 삽입될 수 있지만 UI에 큰 영향을 미치지는 않습니다.
- [ ] 예 — 삽입된 태그를 통해 심각한 UI 변조(Defacement)가 가능합니다.

### 폼이나 악성 링크와 같은 기능적인 피싱 요소를 삽입할 수 있습니까?
- [ ] 아니요 — `<form>`, `<input>`, 및 `<a>` 태그가 효과적으로 차단되거나 정제됩니다.
- [ ] 예 — 사용자를 외부 사이트로 리다이렉트(Redirect)하는 악성 링크가 삽입될 수 있습니다.
- [ ] 예 — 자격 증명을 수집하기 위한 기능적인 `<form>` 요소가 삽입될 수 있습니다. *(중간)*

### 보안 헤더 또는 콘텐츠 보안 정책(Content Security Policy, CSP)이 인젝션의 영향을 완화합니까?
- [ ] 예 — 제한적인 CSP가 적용되어 있으며 `form-action` 또는 `frame-src`가 적용되어 있습니다.
- [ ] 아니요 — 보안 헤더는 존재하지만 CSP가 비활성화되어 있거나 unsafe-inline/와일드카드(Wildcards)를 포함하고 있습니다.
- [ ] 아니요 — CSP나 관련 보안 헤더가 활성화되어 있지 않습니다.

---

## WSTG-CLNT-04 — 클라이언트 측 URL 리다이렉트(Client-Side URL Redirect) 테스트

클라이언트 측 URL 리다이렉트(Client-Side URL Redirect)는 웹 애플리케이션이 주로 URL 파라미터나 해시 프래그먼트(Hash Fragments)를 통해 사용자 제어 입력을 수락하고, 이를 `window.location` 또는 `document.location.href`와 같은 클라이언트 측 리다이렉션 싱크(Sink) 내에서 사용할 때 발생합니다. 이 취약점은 피해자가 악성 사이트로 조용히 리다이렉트되기 전에 먼저 합법적인 도메인을 신뢰하게 되므로, 공격자가 정교한 피싱(Phishing) 캠페인을 수행하는 데 주로 활용됩니다. 피싱 외에도, URL이나 `Referer` 헤더에 해당 값이 포함된 상태에서 리다이렉션이 발생하는 경우, 클라이언트 측 리다이렉트를 연쇄적으로 이용하여 OAuth 액세스 토큰(Access Tokens)이나 세션 식별자(Session Identifiers)와 같은 민감한 데이터를 탈취(Exfiltrate)할 수 있습니다. 현대의 단일 페이지 애플리케이션(SPA)에서는 라우팅 로직, 인증 후 "return to" 파라미터, 또는 언어 전환 기능에서 이 취약점이 자주 나타납니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-04 |
| **CWE** | CWE-601 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium)* |

> *리다이렉트를 사용하여 OAuth 토큰, 세션 ID를 탈취하거나 인증 흐름의 보안 통제를 우회할 수 있는 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/04-Testing_for_Client-side_URL_Redirect  
* https://hacktricks.wiki/en/pentesting-web/open-redirect.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `LinkFinder`, `Arjun`, `B-Config`

### 애플리케이션이 사용자 입력을 기반으로 리다이렉트를 처리하기 위해 클라이언트 측 스크립트를 사용합니까?
- [ ] 아니요 — 리다이렉션은 `3xx` HTTP 상태 코드를 통해 서버 측에서만 처리됩니다.  
- [ ] 예 — 애플리케이션이 URL 파라미터를 기반으로 이동하기 위해 `window.location`과 같은 JavaScript 싱크를 사용합니다.  
- [ ] 예 — 애플리케이션이 사용자 제공 경로를 처리하는 클라이언트 측 프레임워크 라우터(예: React Router, Vue Router)를 사용합니다.  

### 대상 URL에 대해 입력 검증 또는 화이트리스트(Whitelisting) 통제가 구현되어 있습니까?
- [ ] 예 — 허용된 도메인/경로에 대한 엄격한 **화이트리스트**가 적용되어 있으며 우회가 **불가능**합니다.  
- [ ] 예 — 검증이 존재하지만 취약한 정규식(Regex) 또는 블랙리스트(Blacklist)에 의존하며 우회가 **가능**합니다.  
- [ ] 아니요 — 애플리케이션이 임의의 문자열을 리다이렉션 대상으로 수락합니다.  

### 일반적인 난독화 기법을 사용하여 리다이렉트 로직을 우회할 수 있습니까?
- [ ] 아니요 — 통제 장치가 인코딩된 문자, 프로토콜 상대 URL(`//`) 및 백슬래시를 올바르게 처리합니다.  
- [ ] 예 — URL 인코딩 또는 이중 인코딩(Double-encoding)을 사용하여 우회가 **가능**합니다.  
- [ ] 예 — 프로토콜 상대 URL 또는 `@` 문자(예: `https://expected.com@attacker.com`)를 사용하여 우회가 **가능**합니다.  
- [ ] 예 — 특정 브라우저의 특이성(Quirks) 또는 널 바이트 주입(Null Byte Injection)을 사용하여 우회가 **가능**합니다.  

### 리다이렉션 과정에서 대상 사이트에 민감한 데이터가 노출됩니까?
- [ ] 아니요 — URL에 민감한 데이터가 없으며 리다이렉트 중에 헤더를 통해 전송되지 않습니다.  
- [ ] 예 — 민감한 데이터(예: 세션 토큰, API 키)가 `Referer` 헤더를 통해 외부 사이트로 **유출**됩니다.  
- [ ] 예 — URL 프래그먼트(`#`)에 포함된 민감한 데이터에 대상 사이트의 스크립트가 **접근 가능**합니다.  

### 리다이렉트 싱크를 통해 JavaScript를 실행할 수 있습니까 (DOM 기반 XSS)?
- [ ] 아니요 — 싱크가 `http` 또는 `https` 프로토콜로의 이동만 허용합니다.  
- [ ] 예 — 리다이렉트 싱크가 `javascript:` 또는 `data:` URI를 허용하여 DOM 기반 XSS(DOM-based XSS)가 **가능**합니다.

---

## WSTG-CLNT-05 — CSS 주입(CSS Injection) 테스트

CSS 주입(CSS Injection)은 애플리케이션이 적절한 새니타이징(Sanitization) 또는 이스케이프(Escaping) 처리 없이 사용자 입력값이 웹 페이지의 CSS(Cascading Style Sheets)에 영향을 미치도록 허용할 때 발생합니다. 흔히 단순한 외관상의 문제로 치부되기도 하지만, 공격자는 속성 선택자(Attribute Selectors)와 background-image 속성을 사용하여 외부 요청을 트리거함으로써 CSRF 토큰(CSRF Tokens)이나 세션 ID(Session IDs)와 같은 민감한 데이터를 탈취하는 데 CSS 주입을 악용할 수 있습니다. 이 취약점은 일반적으로 사용자 설정(예: 사용자 정의 테마, 글꼴 색상)이 `<style>` 블록이나 인라인 `style` 속성 내에 반영되는 엔드포인트에서 발생합니다. 공격자 관점에서 성공적인 공격은 UI 리드레싱(UI Redressing), 승인되지 않은 레이아웃 수정을 통한 피싱(Phishing), 또는 엄격한 콘텐츠 보안 정책(Content Security Policy, CSP)으로 인해 JavaScript 기반 공격이 차단된 환경에서의 은밀한 데이터 탈취로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-05 |
| **CWE** | CWE-74 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음* |

> *주입 지점에서 CSRF 토큰과 같은 민감한 속성을 탈취할 수 있거나, 민감한 워크플로우에서 UI 리드레싱에 사용될 수 있는 경우 위험도는 '높음'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/05-Testing_for_CSS_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `CSS-Injection-Payload-Generator`, `CyberChef`, `Postman`

### 애플리케이션이 CSS 컨텍스트 내에서 사용자 제어 입력값을 반영합니까?
- [ ] 아니요 — 입력값이 스타일 태그, 속성 또는 CSS 파일에 반영되지 않음  
- [ ] 예 — 입력값이 반영되지만 안전한 영숫자 값으로 엄격히 제한됨  
- [ ] 예 — 입력값이 `style` 속성 또는 `<style>` 블록 내에 반영됨  

### CSS 전용 메타 문자 및 함수가 적절하게 새니타이징 되었습니까?
- [ ] 예 — `{`, `}`, `:`와 같은 문자와 `url()`과 같은 함수가 **적절히 이스케이프 처리됨**  
- [ ] 예 — 새니타이징이 적용되어 있으나 인코딩 또는 CSS 주석을 통한 **우회가 가능함**  
- [ ] 아니요 — CSS 메타 문자에 대해 새니타이징이 적용되지 않음  

### CSS 선택자를 사용한 데이터 탈취가 가능합니까?
- [ ] 아니요 — 선택자가 외부 요청을 트리거하거나 데이터를 유출할 수 없음  
- [ ] 예 — 속성 선택자와 `background-image`를 통한 **부분적인 데이터 탈취가 가능함**  
- [ ] 예 — 자동화된 브루트 포스(Brute Force) 기법을 사용하여 민감한 토큰의 **전체 탈취가 가능함**  

### 콘텐츠 보안 정책(CSP)이 CSS 주입 위험을 완화합니까?
- [ ] 예 — `style-src`가 'self'로 제한되어 있고 `img-src` 또는 `connect-src`가 **제한되어 있음**  
- [ ] 예 — CSP가 존재하지만 'unsafe-inline'을 사용하거나 와일드카드 외부 도메인을 허용함  
- [ ] 아니요 — CSS를 통한 외부 리소스 로드를 방지하기 위한 CSP가 구현되지 않음  

### 취약점을 악용하여 UI 리드레싱 또는 피싱을 수행할 수 있습니까?
- [ ] 아니요 — 주입 범위가 너무 제한적이어서 레이아웃을 크게 수정할 수 없음  
- [ ] 예 — **UI 수정이 가능**하며, 악성 요소를 겹쳐 놓을 수 있음  
- [ ] 예 — 전체 페이지 레이아웃 **제어가 가능**하여, 매우 정교한 피싱 공격이 가능함

---

## WSTG-CLNT-06 — 클라이언트 측 리소스 변조 테스트 (Testing for Client-Side Resource Manipulation)

클라이언트 측 리소스 변조(Client-side resource manipulation)는 애플리케이션이 사용자 제어 입력을 허용하여 브라우저에서 로드되는 스크립트, 스타일시트 또는 iframe과 같은 리소스의 소스나 경로에 영향을 줄 때 발생합니다. URI 프래그먼트(URI fragments), 쿼리 파라미터(Query parameters) 또는 기타 DOM 소스(DOM sources)를 조작함으로써, 공격자는 애플리케이션이 악성 외부 콘텐츠를 로드하거나 승인되지 않은 코드를 실행하도록 강제할 수 있습니다. 이 취약점은 주로 신뢰할 수 있는 허용 목록(Allow-list)에 대한 충분한 검증 없이 HTML 요소의 `src` 또는 `href` 속성을 동적으로 채우는 JavaScript 로직에서 발견됩니다. 공격자 관점에서는 리소스 로딩을 공격자가 제어하는 서버로 리다이렉트(Redirect)하는 URL을 생성하여 이를 악용하며, 이는 잠재적으로 DOM 기반 크로스 사이트 스크립팅(DOM-based Cross-Site Scripting, XSS), 민감한 데이터 탈취(Exfiltration) 또는 UI 리드레싱(UI redressing)으로 이어질 수 있습니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-06 |
| **CWE** | CWE-99 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/06-Testing_for_Client-side_Resource_Manipulation  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `grep`, `Snyk`, `LinkFinder`

### 애플리케이션이 리소스를 동적으로 로드하거나 삽입하기 위해 클라이언트 측 싱크(Sinks)를 사용합니까?
- [ ] 아니요 — 리소스는 정적 HTML 또는 서버 측 로직을 통해서만 로드됩니다.  
- [ ] 예 — 클라이언트 측 JavaScript가 `src`, `href` 또는 `data`와 같은 리소스 속성을 동적으로 채웁니다.  

### 리소스 URI 소스에 대해 검증 제어 또는 허용 목록이 구현되어 있습니까?
- [ ] 예 — 모든 동적 리소스에 대해 엄격한 허용 목록 및 오리진 검증(Origin validation)이 적용됩니다.  
- [ ] 예 — 검증이 존재하지만 취약한 차단 목록(Blacklists) 또는 쉽게 우회 가능한 정규 표현식(Regex)에 의존합니다.  
- [ ] 아니요 — 사용자가 제어하는 리소스 경로에 대해 어떠한 검증 제어도 적용되지 않습니다.  

### 대체 프로토콜이나 인코딩을 사용하여 URI 검증을 우회하는 것이 가능합니까?
- [ ] 아니요 — 리소스 필터 우회가 불가능합니다.  
- [ ] 예 — `data:`, `vbscript:` 또는 `javascript:`와 같은 다른 프로토콜을 사용하여 우회가 가능합니다.  
- [ ] 예 — 인코딩된 문자나 널 바이트(Null bytes)를 사용하여 단순 문자열 매칭을 우회하는 것이 가능합니다.  

### 공격자가 리소스 로딩을 신뢰할 수 없는 외부 도메인으로 리다이렉트할 수 있습니까?
- [ ] 아니요 — 리소스 경로는 동일 오리진(Same origin) 또는 고정된 하위 디렉토리로 제한됩니다.  
- [ ] 예 — 애플리케이션이 임의의 외부 도메인에서 스크립트나 스타일을 로드하도록 강제될 수 있습니다.  

### 리소스 변조의 최대 영향은 무엇입니까?
- [ ] 낮음 — 변조가 이미지나 민감하지 않은 텍스트와 같은 실행 불가능한 요소에만 영향을 미칩니다.  
- [ ] 중간 — 변조를 통해 CSS 인젝션(CSS Injection) 또는 상당한 수준의 UI 리드레싱이 가능합니다.  
- [ ] 높음 — 변조가 DOM 기반 XSS 또는 임의의 JavaScript 실행으로 이어집니다. *(치명적)*

---

## WSTG-CLNT-07 — 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)

교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)는 서버가 다른 출처(Origin)로부터의 리소스 접근을 명시적으로 허용하여 동일 출처 정책 (Same-Origin Policy, SOP)을 완화하는 브라우저 기반 메커니즘입니다. 애플리케이션이 임의의 출처를 과도하게 신뢰할 때 설정 오류가 발생하며, 이는 주로 응답 헤더의 `Access-Control-Allow-Origin`에 `Origin` 헤더를 그대로 반영(Reflect)하거나 잘못 구현된 정규식(Regex) 화이트리스트(Whitelist)를 사용할 때 발생합니다. 공격자의 관점에서, 허용적인 CORS 정책은 공격자가 제어하는 도메인에 악성 스크립트를 호스팅하고 취약한 애플리케이션에 인증된 요청을 보내도록 유도함으로써 악용될 수 있습니다. 이를 통해 공격자는 응답 본문에서 직접 CSRF 토큰이나 개인정보 (PII)와 같은 민감한 데이터를 탈취할 수 있습니다. 이 취약점은 인증된 세션을 처리하고 비공개 정보를 반환하는 API 엔드포인트에서 가장 치명적입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간 / 높음 |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**도구:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### 애플리케이션이 인증된 엔드포인트에 CORS 헤더를 구현하고 있습니까?
- [ ] 아니요 — CORS가 **활성화되지 않음**; 브라우저는 엄격한 동일 출처 정책 (SOP)을 기본으로 적용함  
- [ ] 예 — CORS 헤더가 존재하며 엄격한 **화이트리스트**로 제한됨  
- [ ] 예 — CORS 헤더가 존재하지만 **허용적(Permissive)**으로 설정됨  

### 서버가 `Access-Control-Allow-Origin` (ACAO) 헤더를 어떻게 처리합니까?
- [ ] ACAO가 특정 **신뢰된** 정적 도메인으로 설정됨  
- [ ] ACAO가 `*` (와일드카드)로 설정되었으며 자격 증명(Credentials)을 **보낼 수 없음**  
- [ ] ACAO가 `*` (와일드카드)로 설정되었으며 자격 증명을 **보낼 수 있음** *(참고: 대부분의 브라우저는 이를 차단함)*  
- [ ] ACAO가 요청의 `Origin` 헤더 값을 **동적으로 반영함**  

### `Access-Control-Allow-Credentials` (ACAC) 헤더가 `true`로 설정되어 있습니까?
- [ ] 아니요 — ACAC가 **없거나** `false`로 설정됨  
- [ ] 예 — ACAC가 `true`로 설정되었으나, ACAO가 신뢰된 출처로 **제한됨**  
- [ ] 예 — ACAC가 `true`로 설정되었으며 ACAO가 임의의 출처를 **반영함** *(치명적)*  

### 서브도메인 또는 null 출처 조작을 통해 출처 화이트리스트를 우회할 수 있습니까?
- [ ] 아니요 — 화이트리스트가 엄격하게 강제되며 **우회할 수 없음**  
- [ ] 예 — 화이트리스트가 타겟의 **모든** 서브도메인을 허용함 (예: `attacker.target.com`)  
- [ ] 예 — 화이트리스트가 샌드박스 처리된 iframe을 통한 `null` 출처를 허용함  
- [ ] 예 — 화이트리스트가 정규식(Regex) 우회에 취약함 (예: `target.com.attacker.com` 또는 `target.com-safe.com`)  

### CORS 설정이 민감한 데이터 탈취(Exfiltration)를 허용합니까?
- [ ] 아니요 — 노출된 엔드포인트에 민감한 정보나 세션 특정 데이터가 **포함되지 않음**  
- [ ] 예 — 인증된 엔드포인트가 권한이 없는 출처에서 읽을 수 있는 개인정보 (PII), 자격 증명 또는 CSRF 토큰을 반환함

---

## WSTG-CLNT-08 — 크로스 사이트 플래싱 테스트 (Testing for Cross Site Flashing)

크로스 사이트 플래싱 (Cross-Site Flashing, XSF)은 플래시 (Flash, SWF) 파일이 사용자가 제어하는 입력값을 부적절하게 처리할 때 발생하는 클라이언트 측 취약점으로, 공격자가 악성 액션스크립트 (ActionScript) 코드를 실행하거나 브라우저의 자바스크립트 (JavaScript) 환경으로 연결할 수 있게 합니다. `ExternalInterface.call`, `getURL`, 또는 `loadMovie`와 같은 싱크(sink)로 전달되는 `FlashVars` 또는 URL 쿼리 문자열과 같은 파라미터를 조작함으로써, 공격자는 취약한 도메인의 컨텍스트에서 세션 하이재킹 (Session Hijacking) 및 데이터 유출 (Data Exfiltration)을 포함한 동작을 수행할 수 있습니다. 이 취약점은 주로 SWF 파일을 여전히 호스팅하고 있는 레거시 기업용 애플리케이션에서 발견되며, 불충분한 입력값 검증 (Input Validation)으로 인해 플래시 무비가 크로스 사이트 스크립팅 (Cross-Site Scripting, XSS)의 벡터로 재사용되거나 허용적인 교차 도메인 설정을 통해 동일 출처 정책 (Same-Origin Policy, SOP)을 우회하는 데 악용될 수 있습니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **심각도** | 중간 (Medium) / 높음 (High) |

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### 애플리케이션에 레거시 Adobe Flash (.swf) 파일이 호스팅되어 있습니까?
- [ ] 아니요 — 애플리케이션 범위 내에 호스팅되거나 참조되는 플래시 파일이 없습니다.  
- [ ] 예 — SWF 파일이 존재하지만 정적 콘텐츠만 제공합니다.  
- [ ] 예 — SWF 파일이 존재하며 `FlashVars` 또는 URL 문자열을 통해 동적 파라미터를 허용합니다.  

### ActionScript 싱크로 전달되는 입력값이 살균(sanitize) 처리됩니까?
- [ ] 예 — 모든 입력값이 엄격하게 검증되며 ActionScript 싱크를 조작할 수 없습니다.  
- [ ] 예 — 검증이 적용되어 있으나 특정 인코딩 기술을 통해 우회가 가능합니다.  
- [ ] 아니요 — 입력값이 `ExternalInterface.call` 또는 `getURL`과 같은 민감한 싱크로 직접 전달됩니다. *(위험)*  

### 플래시 파일을 사용하여 임의의 자바스크립트(XSS)를 실행할 수 있습니까?
- [ ] 아니요 — `ExternalInterface`가 비활성화되어 있거나 `allowScriptAccess` 파라미터가 `never`로 설정되어 있습니다.  
- [ ] 예 — `allowScriptAccess`가 `sameDomain`으로 설정되어 있으나 SWF가 대상 도메인에 호스팅되어 있습니다.  
- [ ] 예 — `allowScriptAccess`가 `always`로 설정되어 있어 모든 도메인에서 XSS가 가능합니다.  

### `crossdomain.xml` 정책이 승인되지 않은 교차 출처 요청을 방지합니까?
- [ ] 예 — `crossdomain.xml`이 제한적이며 신뢰할 수 있는 특정 출처만 허용합니다.  
- [ ] 아니요 — `crossdomain.xml` 파일이 누락되었습니다.  
- [ ] 예 — 정책이 과도하게 허용적입니다 (예: `<allow-access-from domain="*" />`).  

### SWF 파일이 공격자가 제어하는 외부 무비를 강제로 로드하도록 할 수 있습니까?
- [ ] 아니요 — 애플리케이션이 외부 SWF 파일을 로드하도록 강제할 수 없습니다.  
- [ ] 예 — `loadMovie` 또는 `loadMovieNum` 함수가 검증되지 않은 외부 URL을 허용합니다.

---

## WSTG-CLNT-09 — 클릭재킹(Clickjacking) 테스트

클릭재킹(Clickjacking)은 UI 레드레싱(UI redressing)이라고도 하며, 공격자가 투명하거나 불투명한 레이어를 사용하여 사용자가 의도했던 상위 페이지의 버튼이나 링크가 아닌 다른 페이지의 요소를 클릭하도록 속이는 기법입니다. 이 취약점은 사용자 동작의 무결성을 훼손하여, 피해자를 대신해 승인되지 않은 데이터 수정, 계정 변경 또는 금융 송금 등을 실행할 가능성이 있습니다. 공격자는 일반적으로 제어 하에 있는 악성 사이트의 `<iframe>` 내에 대상 웹사이트를 삽입하고, 그 위에 기만적인 콘텐츠나 유도 장치를 겹쳐서 이를 악용합니다. 이는 "계정 삭제" 버튼, 비밀번호 재설정 양식 또는 관리 설정 패널과 같이 민감한 상태 변경 작업(state-changing actions)을 수행하는 페이지에서 가장 빈번하게 발생합니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *클릭재킹을 통해 민감한 작업(예: 자금 이체, 계정 삭제 또는 권한 상승)을 실행할 수 있는 경우 위험도는 '높음'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**도구:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### 애플리케이션이 권한이 없는 프레이밍(framing)을 방지하기 위해 서버 측 헤더를 구현하고 있습니까?
- [ ] 예 — `X-Frame-Options` 또는 `Content-Security-Policy`가 **적용되어** 프레이밍을 방지함 *(가장 안전)*  
- [ ] 예 — 헤더가 존재하지만 **잘못 설정됨** (예: 브라우저 지원이 광범위하지 않은 `X-Frame-Options: ALLOW-FROM` 사용)  
- [ ] 아니요 — 프레임 보호 헤더가 **적용되지 않음**  

### `Content-Security-Policy` (CSP)의 `frame-ancestors` 지시문이 구현되어 있습니까?
- [ ] 예 — `frame-ancestors 'none'` 또는 `'self'`가 **적용됨**  
- [ ] 예 — `frame-ancestors`가 존재하지만 신뢰할 수 없거나 너무 광범위한 출처(origin)를 **허용함**  
- [ ] 아니요 — 지시문이 **누락되었거나** CSP가 구현되지 않음  

### 레거시 브라우저 지원을 위해 `X-Frame-Options` (XFO) 헤더가 올바르게 설정되어 있습니까?
- [ ] 예 — `DENY` 또는 `SAMEORIGIN`이 **적용됨**  
- [ ] 예 — XFO가 존재하지만 CSP `frame-ancestors` 지시문이 있어 최신 브라우저에서 **무시됨**  
- [ ] 아니요 — XFO 헤더가 **누락되었거나** 안전하지 않은 값으로 설정됨  

### 알려진 기법을 사용하여 프레이밍 보호를 우회할 수 있습니까?
- [ ] 아니요 — 표준 기법(Double-framing 등)을 통한 우회가 **불가능함**  
- [ ] 예 — `frame-ancestors` 화이트리스트의 취약한 정규식 또는 로직으로 인해 보호를 **우회할 수 있음**  
- [ ] 예 — 애플리케이션이 자바스크립트 기반의 프레임 버스팅(Frame-busting)에만 의존하여 보호를 **우회할 수 있음**  

### 클릭재킹 개념 증명(PoC)을 통해 민감한 작업을 악용할 수 있습니까?
- [ ] 아니요 — 프레이밍이 차단되었거나 민감한 작업에 접근할 수 없음  
- [ ] 예 — UI 레드레싱이 **가능하지만** 복잡한 사용자 상호작용이 필요함  
- [ ] 예 — UI 레드레싱이 **가능하며** 즉각적인 상태 변경 작업을 허용함 *(심각함)*

---

## WSTG-CLNT-10 — 웹소켓(WebSockets) 테스트

웹소켓(WebSocket) 테스트는 클라이언트와 서버 사이에 설정된 전 이중(Full-duplex) 통신 채널에 중점을 둡니다. 이는 전통적인 HTTP 요청-응답 패턴을 우회합니다. 침투 테스트 수행자(Pentesters)는 핸드쉐이크(Handshake) 과정의 보안성, 크로스 사이트 웹소켓 하이재킹(Cross-Site WebSocket Hijacking, CSWSH) 방어 기제의 존재 여부, 그리고 메시지 페이로드(Payload)에 적용된 입력값 검증의 엄격함을 평가합니다. 익스플로잇(Exploit)은 일반적으로 활성화된 세션을 하이재킹(Hijacking)하여 실시간으로 데이터를 유출하거나, 지속적인 소켓을 통해 커맨드 인젝션(Command Injection) 또는 SQL 인젝션(SQL Injection, SQLi)과 같은 서버 측 취약점을 유발하는 악성 페이로드를 주입하는 방식으로 이루어집니다. 많은 기존 웹 애플리케이션 방화벽(Web Application Firewalls, WAF)이 웹소켓 트래픽을 효과적으로 검사하지 못하기 때문에, 이 프로토콜은 종종 경계 방어를 우회하는 은밀한 벡터를 제공합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-10 |
| **CWE** | CWE-1385 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/10-Testing_WebSockets  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/websockets  

**도구:** `Burp Suite (WebSockets tab)`, `OWASP ZAP`, `wscat`, `Socket.io-client`, `Wireshark`

### 웹소켓 연결이 보안 채널을 통해 설정되어 있습니까?
- [ ] 예 — 모든 연결에 대해 `wss://` (WebSocket Secure)가 강제됩니다.  
- [ ] 아니오 — `ws://`가 사용되어 중간자 공격(Person-in-the-Middle, PITM) 도청이 가능합니다.  

### 애플리케이션이 크로스 사이트 웹소켓 하이재킹(CSWSH)으로부터 보호되고 있습니까?
- [ ] 아니오 — 서버가 핸드쉐이크 동안 `Origin` 헤더를 엄격하게 검증합니다.  
- [ ] 예 — `Origin` 헤더 검증이 약하거나 null 또는 스푸핑(Spoofed)된 origin을 통해 우회될 수 있습니다.  
- [ ] 예 — `Origin` 헤더 검증이 수행되지 않아 크로스 사이트 공격자가 연결을 시작할 수 있습니다 *(치명적, Critical)*.  

### 웹소켓 메시지 페이로드에 입력값 검증 및 필터링(Sanitization)이 적용되었습니까?
- [ ] 예 — 모든 수신 메시지가 필터링되고 스키마(Schema)에 대해 검증됩니다.  
- [ ] 예 — 일부 검증이 존재하지만 인코딩되거나 비표준 페이로드를 사용한 우회가 가능합니다.  
- [ ] 아니오 — 메시지가 직접 처리되어 인젝션(XSS, SQLi, RCE) 또는 로직 조작이 가능합니다.  

### 모든 웹소켓 메시지에 대해 인증(Authentication) 및 인가(Authorization) 검사가 수행됩니까?
- [ ] 예 — 모든 메시지에 대해 세션이 확인되거나 프로토콜이 토큰을 사용하는 상태 비저장(Stateless) 방식입니다.  
- [ ] 예 — 핸드쉐이크 시 인증이 수행되지만, 특정 작업에 대한 인가는 메시지별로 강제되지 않습니다.  
- [ ] 아니오 — 인증이 핸드쉐이크 시에만 확인되므로, 세션 하이재킹을 통해 전체 권한 획득이 가능합니다.  

### 서버가 웹소켓에 속도 제한(Rate Limiting) 또는 리소스 제약을 구현하고 있습니까?
- [ ] 예 — 속도 제한 및 최대 메시지 크기가 활성화되어 강제됩니다.  
- [ ] 아니오 — 제한이 존재하지 않아 메시지 플러딩(Flooding) 또는 대용량 페이로드를 통한 서비스 거부(Denial of Service, DoS) 공격이 가능합니다.

---

## WSTG-CLNT-11 — 웹 메시징(Web Messaging) 테스트

웹 메시징(Web Messaging) 또는 `postMessage`는 부모 페이지와 생성된 팝업 또는 임베디드 아이프레임(embedded iframe)과 같은 윈도우 객체 간의 교차 출처 통신(Cross-origin communication)을 가능하게 합니다. 이 메커니즘은 현대적인 웹 기능에 필수적이지만, 수신 측 애플리케이션이 발신자의 신원을 확인하지 않거나 메시지 페이로드(Payload)를 부적절하게 처리할 경우 심각한 위험을 초래합니다. 공격자는 대상 애플리케이션을 임베드한 악성 사이트를 호스팅하고, 조작된 메시지를 전송하여 의도하지 않은 작업을 유도하거나, 민감한 데이터를 탈취(Exfiltrate)하거나, DOM 기반 교차 사이트 스크립팅(DOM-based XSS)을 실행함으로써 이러한 취약점을 악용합니다. 메시지 리스너(Message listener)가 `origin` 속성을 엄격하게 검증하지 않거나 `message.data`를 위험한 실행 싱크(Execution sinks)에 직접 전달할 때 성공적인 공격이 이루어집니다.

| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-11 |
| **CWE** | CWE-345 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 중간(Medium) / 높음(High) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/11-Testing_Web_Messaging  
* https://hacktricks.wiki/en/pentesting-web/postmessage-vulnerabilities/index.html  

**도구:** `Burp Suite (DOM Invader)`, `Browser Developer Tools`, `PostMessage-Tracker`, `PMHook`

### 애플리케이션이 문서 간 통신을 위해 `postMessage` API를 사용합니까?
- [ ] 아니오 — 클라이언트 측 코드에 `postMessage` 리스너가 존재하지 않음  
- [ ] 예 — 내부 컴포넌트 통신을 위해 `postMessage`가 사용됨  
- [ ] 예 — 제3자 도메인 또는 위젯과의 통신을 위해 `postMessage`가 사용됨  

### 수신 메시지의 출처(Origin)가 엄격하게 검증됩니까?
- [ ] 예 — `===` 또는 동일 비교 연산자를 사용하여 엄격한 화이트리스트(Whitelist)에 대해 출처를 확인합니다 *(가장 안전)*  
- [ ] 예 — 정규식(Regex)을 사용하여 출처를 검증하지만, 로직이 우회(Bypass)될 가능성이 있습니다 (예: `^https://trusted.com`)  
- [ ] 아니오 — `origin` 속성을 확인하지 않아 모든 도메인에서 메시지를 보낼 수 있습니다  
- [ ] 아니오 — 송신자의 `targetOrigin`에 와일드카드 `*`가 사용되어 악의적인 리스너에게 데이터가 유출될 가능성이 있습니다  

### 메시지 페이로드가 애플리케이션에서 처리되기 전에 살균(Sanitize)됩니까?
- [ ] 예 — 데이터를 일반 텍스트(Plain text)로 취급하며 위험한 싱크(Sink)를 사용하지 않습니다  
- [ ] 예 — 데이터를 사용하기 전에 파싱(예: `JSON.parse`) 및 검증을 수행하며, 우회가 불가능합니다  
- [ ] 아니오 — 메시지 데이터가 `innerHTML`, `eval()`, 또는 `setTimeout()`과 같은 위험한 싱크에 직접 전달됩니다  
- [ ] 아니오 — 검증 없이 메시지 데이터를 사용하여 윈도우를 탐색하거나 `location.href`를 변경합니다  

### 공격자가 악성 메시지를 통해 권한 없는 작업을 유도하거나 데이터를 탈취할 수 있습니까?
- [ ] 아니오 — 위조된 메시지를 사용하더라도 민감한 작업이나 데이터에 접근할 수 없습니다  
- [ ] 예 — 조작된 메시지로 민감한 상태 변경(예: 비밀번호 변경, 프로필 업데이트)을 유도할 수 있습니다  
- [ ] 예 — 조작된 메시지로 인해 DOM 기반 XSS가 발생하거나 CSRF 토큰/세션 데이터가 탈취될 수 있습니다

---

## WSTG-CLNT-12 — 브라우저 스토리지 테스트 (Test Browser Storage)

브라우저 스토리지(Browser Storage) 테스트는 애플리케이션이 데이터를 유지하기 위해 로컬 스토리지(LocalStorage), 세션 스토리지(SessionStorage), IndexedDB 및 WebSQL과 같은 클라이언트 측 메커니즘을 어떻게 활용하는지 분석하는 과정을 포함합니다. 개인식별정보(PII), 인증 토큰(Authentication Tokens) 또는 세션 식별자(Session Identifiers)를 포함하여 안전하지 않게 저장된 민감한 정보는 공격자가 교차 사이트 스크립팅(XSS)을 수행하거나 사용자의 장치에 물리적으로 접근할 경우 노출될 수 있습니다. 모의해킹 전문가(Pentester)는 데이터가 평문(Cleartext)으로 저장되는지, 로그아웃 후에도 접근 가능한지, 그리고 데이터 유출을 방지하기 위해 스토리지 생명주기가 적절히 관리되는지 확인해야 합니다. 공격자 관점에서 이러한 스토리지 메커니즘은 세션 하이재킹(Session Hijacking)이나 장기적인 계정 지속성(Account Persistence)을 가능하게 하는 상태 유지 정보를 탈취(Exfiltrating)하기 위한 매력적인 대상입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-12 |
| **CWE** | CWE-922 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 중간 (Medium) / 높음 (High)* |

> *세션 토큰이나 민감한 PII가 LocalStorage에 저장되어 있고 XSS를 통해 접근 가능한 경우 위험도는 '높음(High)'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/12-Testing_Browser_Storage  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Browser DevTools`, `Storage Explorer`, `Burp Suite`, `OWASP ZAP`

### 애플리케이션이 브라우저 스토리지에 민감한 정보를 저장합니까?
- [ ] 아니오 — 애플리케이션이 브라우저 스토리지를 사용하지 않거나 민감하지 않은 UI 상태만 저장함  
- [ ] 예 — 민감한 데이터가 저장되지만 강력한 클라이언트 측 암호화 기술을 사용하여 암호화됨  
- [ ] 예 — 민감한 데이터(토큰, PII, 비밀 정보)가 **평문(Cleartext)**으로 저장됨 *(중간)*  

### 저장된 데이터가 권한이 없는 스크립트에서 접근 가능합니까 (XSS 위험)?
- [ ] 아니오 — 데이터가 HttpOnly 쿠키(브라우저 스토리지가 아님)에 저장되거나 스토리지가 사용되지 않음  
- [ ] 예 — LocalStorage/SessionStorage가 사용되어, 임의의 XSS 페이로드(Payload)를 통해 데이터에 **완전한 접근**이 가능함 *(높음)*  

### 사용자 로그아웃 시 브라우저 스토리지가 적절히 삭제됩니까?
- [ ] 예 — 로그아웃 과정에서 모든 애플리케이션 관련 스토리지가 **명시적으로 삭제(Cleared)**됨  
- [ ] 아니오 — 로그아웃 후에도 스토리지가 유지되지만, 민감한 세션 데이터를 포함하지 않음  
- [ ] 아니오 — 세션이 종료된 후에도 인증 토큰이나 PII가 스토리지에 **유지**됨 *(중간)*  

### 애플리케이션이 대규모/민감한 데이터 세트를 위해 IndexedDB 또는 WebSQL을 사용합니까?
- [ ] 아니오 — 이러한 스토리지 메커니즘이 사용되지 않음  
- [ ] 예 — 데이터가 저장되며 DOM에 렌더링되기 전에 **적절히 새니타이징(Sanitized)**됨  
- [ ] 예 — 민감한 데이터 세트가 IndexedDB/WebSQL 구조 내에 **평문**으로 저장됨  

### 저장된 데이터의 무결성(Integrity)을 조작하여 애플리케이션 로직에 영향을 미칠 수 있습니까?
- [ ] 아니오 — 애플리케이션이 스토리지를 처리하기 전에 데이터를 검증하거나 서명함  
- [ ] 예 — 스토리지 값(예: 사용자 역할, 플래그)을 수정하는 것이 **가능**하며, 그 결과 애플리케이션 동작이 변경됨

---

## WSTG-CLNT-13 — 사이트 간 스크립트 포함(Cross-Site Script Inclusion) 테스트

사이트 간 스크립트 포함(Cross-Site Script Inclusion, XSSI)은 웹 애플리케이션이 동적으로 생성된 JavaScript 파일이나 JSONP 응답 내에 민감한 데이터를 포함하여 내보낼 때 발생하며, 공격자가 제어하는 사이트에서 `<script>` 태그를 통해 이를 포함할 수 있습니다. 스크립트 포함은 동일 출처 정책(Same-Origin Policy, SOP)을 우회하므로, 공격자는 자신의 컨텍스트에서 스크립트를 실행하고 전역 객체 또는 변수를 오버라이드(Override)하여 민감한 정보를 탈취(Exfiltration)할 수 있습니다. 이 취약점은 주로 이메일 주소, API 키(API keys) 또는 세션 메타데이터와 같은 사용자 특정 데이터를 스크립트 형식으로 반환하는 인증된 엔드포인트에서 나타납니다. 공격자 관점에서 익스플로잇(Exploit)은 취약한 스크립트를 참조하고 그 결과로 발생하는 전역 변수 변경이나 함수 호출을 캡처하는 악성 페이지를 제작하는 과정을 포함합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 보통 / 높음 |

> *유출된 데이터에 CSRF 토큰, 세션 식별자 또는 매우 민감한 개인 식별 정보(Personally Identifiable Information, PII)가 포함된 경우 심각도는 '높음'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### 인증된 엔드포인트가 민감한 정보를 포함하는 동적 JavaScript 또는 JSONP를 반환합니까?
- [ ] 아니요 — 모든 스크립트가 정적이며 사용자 특정 데이터를 포함할 수 없습니다.
- [ ] 예 — 동적 스크립트가 존재하지만 민감한 정보는 포함되어 있지 않습니다.
- [ ] 예 — 동적 스크립트에 PII 또는 토큰이 포함되어 있으며 출처 간(Cross-Origin) 접근이 가능합니다.

### 민감한 스크립트 응답에 `X-Content-Type-Options: nosniff` 헤더가 구현되어 있습니까?
- [ ] 예 — 헤더가 적용되어 MIME 타입 스니핑(MIME-type sniffing)을 방지합니다.
- [ ] 아니요 — 헤더가 누락되어 스크립트가 아닌 콘텐츠가 스크립트로 실행될 가능성이 있습니다.

### 민감한 스크립트가 실행 불가능한 접두사(Non-executable prefixes) 또는 사용자 정의 헤더와 같은 anti-XSSI 메커니즘으로 보호됩니까?
- [ ] 예 — 스크립트 호출 시 표준 `<script>` 태그를 통해 전송할 수 없는 사용자 정의 헤더 또는 토큰이 필요합니다.
- [ ] 예 — 스크립트가 쉽게 우회할 수 없는 실행 불가능한 접두사(예: `)]}'`)를 사용합니다.
- [ ] 아니요 — 앰비언트 자격 증명(Ambient credentials)을 사용하는 GET 요청을 통해 스크립트에 직접 접근할 수 있습니다. *(중대함)*

### 전역 변수나 프로토타입(Prototypes)을 오버라이드하여 민감한 데이터를 탈취하는 것이 가능합니까?
- [ ] 아니요 — 민감한 데이터의 범위가 로컬로 제한되거나 현대적인 브라우저 기능을 통해 보호됩니다.
- [ ] 예 — 민감한 데이터가 전역 변수에 할당되어 캡처될 수 있습니다.
- [ ] 예 — 인터셉트(Intercept) 가능한 JSONP 콜백을 통해 데이터가 유출됩니다.

---

## WSTG-CLNT-14 — 역 탭내빙 테스트 (Testing for Reverse Tabnabbing)

역 탭내빙(Reverse Tabnabbing)은 `target="_blank"` 속성을 통해 열린 페이지가 `window.opener` 객체를 통해 부모 페이지에 대한 기능적 참조를 유지하는 클라이언트 측 취약점(Client-side vulnerability)입니다. 이를 통해 공격자가 제어하는 대상 사이트는 프로그래밍 방식으로 원래의 신뢰할 수 있는 탭을 악성 URL(일반적으로 자격 증명을 탈취하기 위해 설계된 피싱 페이지(Phishing page))로 리다이렉트(Redirect)할 수 있습니다. 이 공격은 사용자가 원래 탭에 대해 가지는 본질적인 신뢰를 악용하며, 사용자는 방금 떠난 페이지가 다른 도메인으로 이동했다는 사실을 인지하지 못할 가능성이 높습니다. 현대적인 보안 관행에서는 이러한 교차 창 통신을 방지하기 위해 앵커 태그(Anchor tags)에 `rel="noopener"` 또는 `rel="noreferrer"` 속성을 사용하거나, 자바스크립트(JavaScript)에서 `opener` 속성을 null로 설정할 것을 요구합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 낮음 / 중간* |

> *현대적인 브라우저(Modern browsers)는 이제 `target="_blank"` 사용 시 기본적으로 `rel="noopener"`를 적용하므로 대부분의 경우 심각도는 '낮음'입니다. 애플리케이션이 레거시 브라우저(Legacy browsers)를 대상으로 하거나, `opener` 속성을 null로 설정하지 않고 `window.open()`을 사용하는 경우 심각도는 '중간'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### 새 창이나 탭에서 열리는 링크가 존재합니까?
- [ ] 아니오 — `target="_blank"` 또는 그에 상응하는 자바스크립트 기반 리다이렉션을 사용하는 링크가 없습니다.  
- [ ] 예 — `target="_blank"`가 내부의 신뢰할 수 있는 링크에서만 사용됩니다.  
- [ ] 예 — `target="_blank"`가 외부 링크나 사용자 제공 URL에 사용됩니다.  

### 앵커 태그에서 `window.opener` 관계가 제한되어 있습니까?
- [ ] 예 — `target="_blank"`가 포함된 모든 링크에 `rel="noopener"` 또는 `rel="noreferrer"`가 **일관되게 적용**되어 있습니다 (가장 안전).  
- [ ] 예 — 속성이 적용되었지만 고위험 또는 사용자 생성 링크에서 **누락**되었습니다.  
- [ ] 아니오 — 속성이 **적용되지 않아** 하위 페이지가 `window.opener`에 액세스할 수 있습니다.  

### 애플리케이션이 자바스크립트 `window.open()`을 통한 역 탭내빙에 취약합니까?
- [ ] 아니오 — `window.open()`이 사용되지 않거나 `opener` 속성을 명시적으로 null로 설정합니다.  
- [ ] 예 — `window.open()`이 사용되며 하위 창에서 `opener` 참조가 **접근 가능한 상태로 유지**됩니다.  

### 공격자가 새 탭에서 열리는 링크의 목적지를 제어할 수 있습니까?
- [ ] 아니오 — 모든 링크 목적지가 하드코딩되어 있으며 신뢰할 수 있는 도메인을 가리킵니다.  
- [ ] 예 — 링크 목적지가 사용자로부터 제공되며(예: 소셜 미디어 프로필, 바이오 링크 등), 탭내빙 방지를 위한 **필터링(Sanitization)이 수행되지 않습니다.**

---

## WSTG-APIT-01 — API 정찰 (API Reconnaissance)

API 정찰(API Reconnaissance)은 엔드포인트(Endpoints), 지원되는 메서드 및 기본 데이터 구조를 파악하기 위해 API 공격 표면(Attack Surface)을 식별하고 매핑하는 체계적인 과정입니다. 공격자는 이 단계를 통해 문서화되지 않은 섀도우 API(Shadow API), 레거시 취약점이 있는 지원 중단된 버전, 그리고 내부 비즈니스 로직을 드러내는 Swagger 또는 OpenAPI 정의와 같은 노출된 문서 파일을 발견합니다. 예측 가능한 URL 패턴, 클라이언트 측 JavaScript 파일 및 디렉토리 브루트 포스(Directory Brute Force) 결과를 분석함으로써, 공격자는 API의 포괄적인 맵을 구축하여 객체 수준 권한 관리 취약점(Broken Object Level Authorization, BOLA) 또는 매스 어사인먼트(Mass Assignment) 공격 대상을 식별할 수 있습니다. 이러한 정찰은 일반적으로 `/api/v1/`, `/swagger.json` 또는 `/graphql`과 같은 공통 경로를 대상으로 하여 애플리케이션의 백엔드 기능에 대한 로드맵을 확보합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-APIT-01 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 (Not Performed) |
| **위험도** | 정보성 (Informational) / 낮음 (Low) |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/01-API_Reconnaissance  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/api-testing  

**도구:** `Kiterunner`, `ffuf`, `Arjun`, `Burp Suite (Logger++)`, `Postman`, `Swagger-ez`

### API 문서 파일(Swagger, OpenAPI, WSDL)이 공개적으로 접근 가능한가?
- [ ] 아니요 — API 문서에 접근할 수 없거나 인증에 의해 제한되어 있습니다.  
- [ ] 예 — 문서에 접근할 수 있지만 유효한 자격 증명이 필요합니다.  
- [ ] 예 — API 문서(예: `/swagger-ui.html`)가 인증 없이 **공개적으로 접근 가능**합니다.  

### 퍼징(Fuzzing)을 통해 문서화되지 않은 "섀도우" API 엔드포인트를 발견할 수 있는가?
- [ ] 아니요 — 디렉토리 및 엔드포인트 퍼징 결과 문서화되지 않은 경로가 발견되지 않았습니다.  
- [ ] 예 — 문서화되지 않은 엔드포인트가 발견되었으나 권한 없이는 접근할 수 없습니다.  
- [ ] 예 — 문서화되지 않은 엔드포인트가 발견되었으며 유효한 권한 없이도 **접근할 수 있습니다**.  

### 애플리케이션이 여러 API 버전(예: /v1/, /v2/, /beta/)을 노출하고 있는가?
- [ ] 아니요 — 현재의 강화된 API 버전만 접근 가능합니다.  
- [ ] 예 — 레거시 버전이 존재하지만 현재 버전과 **동일한** 보안 통제가 적용되어 있습니다.  
- [ ] 예 — 레거시 버전(예: `/v1/`)에 접근 가능하며 최신 버전의 보안 통제가 **누락**되어 있습니다.  

### 클라이언트 측 리소스(JavaScript/모바일 앱)에서 API 엔드포인트 구조나 키가 유출되는가?
- [ ] 아니요 — 클라이언트 측 코드에 하드코딩된 API 경로 또는 민감한 키가 포함되어 있지 않습니다.  
- [ ] 예 — 클라이언트 측 코드에 API 엔드포인트 맵이 포함되어 있으나 민감한 키는 없습니다.  
- [ ] 예 — API 키, 비밀번호(Secrets) 또는 내부 전용 엔드포인트 URL이 클라이언트 측 리소스에 **하드코딩되어 있습니다**.  

### 알려진 엔드포인트에서 숨겨진 파라미터나 헤더를 발견할 수 있는가?
- [ ] 아니요 — 파라미터 퍼징(예: `Arjun` 이용)을 통해 숨겨진 입력값이 발견되지 않았습니다.  
- [ ] 예 — 숨겨진 파라미터(예: `debug=true`, `admin=1`)가 발견되었으나 작동하지 않습니다.  
- [ ] 예 — 숨겨진 파라미터가 발견되었으며 애플리케이션 동작을 변경하는 데 **사용될 수 있습니다**.

---

## WSTG-APIT-02 — API 객체 수준 권한 관리 취약점 (Broken Object Level Authorization, BOLA)

객체 수준 권한 관리 취약점(Broken Object Level Authorization, BOLA)은 안전하지 않은 직접 객체 참조(Insecure Direct Object Reference, IDOR)라고도 하며, API가 사용자가 ID로 식별된 특정 리소스에 액세스하거나 조작할 수 있는 적절한 권한이 있는지 검증하지 못할 때 발생합니다. 공격자는 요청 경로, 쿼리 매개변수 또는 JSON 본문에서 숫자 ID나 UUID와 같은 식별자를 체계적으로 열거하거나 추측하여 다른 사용자의 데이터에 액세스함으로써 이를 악용합니다. 이 취약점은 현대 API 보안에서 가장 흔하고 영향력이 큰 문제로, 대규모 데이터 유출, 무단 수정 또는 완전한 계정 탈취로 이어질 수 있습니다. 공격자의 관점에서 목표는 객체 식별자를 허용하는 엔드포인트를 식별하고, 서버 측 권한 부여(Authorization) 로직이 누락되었거나 해당 ID를 조작하여 우회할 수 있는지 테스트하는 것입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-APIT-02 |
| **CWE** | CWE-285 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 / 치명적 |

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/02-API_Broken_Object_Level_Authorization  
* https://hacktricks.wiki/en/pentesting-web/idor.html  
* https://portswigger.net/web-security/api-testing  

**도구:** `Burp Suite (Intruder/Repeater)`, `Autorize`, `Postman`, `ffuf`, `Arjun`

### API 요청 내에서 객체 식별자를 예측하거나 열거할 수 있습니까?
- [ ] 아니요 — 식별자가 길고 무작위적이며 암호학적으로 안전합니다 (예: UUIDv4)  
- [ ] 예 — 식별자가 순차적인 정수입니다 (예: `101`, `102`)  
- [ ] 예 — 식별자가 예측 가능한 패턴을 따르거나 공개된 정보에서 파생됩니다  

### API가 모든 요청에 대해 객체 소유권에 대한 서버 측 검증을 수행합니까?
- [ ] 예 — 권한 부여 확인이 모든 요청에 적용되며 우회가 **불가능합니다**  
- [ ] 예 — 확인이 적용되지만 매개변수 오염(Parameter Pollution) 또는 메서드 터널링(Method Tunneling)을 통해 우회가 **가능합니다**  
- [ ] 아니요 — 애플리케이션이 특정 리소스 소유권을 확인하지 않고 사용자가 인증(Authentication)되었다는 사실에만 전적으로 의존합니다  

### 식별자를 변경하여 다른 사용자의 리소스에 액세스하거나 수정할 수 있습니까?
- [ ] 아니요 — 다른 사용자의 리소스에 대한 무단 액세스가 **불가능합니다**  
- [ ] 예 — 무단 **읽기** 액세스(수평적 BOLA)가 **가능합니다**  
- [ ] 예 — 무단 **수정** 또는 **삭제**(수평적 BOLA)가 **가능합니다**  

### API가 ID를 대체하여 관리자 또는 시스템 수준 객체에 액세스하는 것을 허용합니까?
- [ ] 아니요 — 관리 리소스는 보조 권한 부여 계층에 의해 보호됩니다  
- [ ] 예 — 시스템 수준 리소스 액세스 또는 수정(수직적 BOLA)이 **가능합니다**  

### 식별자를 요청의 다른 부분으로 이동하여 권한 부여 확인을 우회할 수 있습니까?
- [ ] 아니요 — 매개변수 위치에 관계없이 권한 부여 로직이 일관됩니다  
- [ ] 예 — ID를 URL 경로에서 JSON 본문이나 헤더로 이동할 때 우회가 **가능합니다**  
- [ ] 예 — 여러 ID가 제공되고 서버가 권한이 없는 식별자를 처리할 때 우회가 **가능합니다**

---

## WSTG-APIT-99 — GraphQL 테스트 (Testing GraphQL)

GraphQL은 클라이언트가 특정 데이터 구조를 요청할 수 있게 해주는 API용 쿼리 언어(Query Language)이지만, 설정 오류로 인해 정보 노출(Information Disclosure) 및 리소스 고갈(Resource Exhaustion)을 포함한 심각한 보안 위험이 발생하는 경우가 많습니다. 취약점은 주로 활성화된 인트로스펙션(Introspection), 쿼리 깊이/복잡도 제한(Query depth/complexity limits) 부족, 리졸버 함수(Resolver functions) 내의 객체 수준 권한 관리 미흡(BOLA)으로 인해 발생합니다. 공격자는 인트로스펙션을 사용하여 전체 스키마(Schema)를 매핑하거나, 순환 프래그먼트(Circular fragments)를 활용하여 서비스 거부(DoS) 공격을 유발하거나, 중첩 쿼리(Nested queries)를 통해 승인되지 않은 필드에 접근하여 권한을 우회함으로써 이러한 엔드포인트(Endpoints)를 공격합니다. 테스트는 구현 시 엄격한 접근 제어 및 리소스 관리가 수행되는지 확인하기 위해 `/graphql`, `/v1/graphql` 또는 `/graphiql` 엔드포인트에 집중합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-APIT-99 |
| **CWE** | CWE-200 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 / 치명적* |

> *객체 수준 권한 관리 미흡(BOLA)으로 인해 개인 식별 정보(PII) 또는 관리자 뮤테이션(Mutations)에 대한 무단 접근이 허용되는 경우 심각도는 '치명적(Critical)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/12-API_Testing/99-Testing_GraphQL  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/graphql  

**도구:** `InQL`, `Graphw00f`, `GraphQL Voyager`, `Burp Suite`, `Altair GraphQL Client`, `Clairvoyance`

### GraphQL 인트로스펙션 시스템이 활성화되어 있습니까?
- [ ] 아니오 — 인트로스펙션이 **비활성화**되어 있으며 스키마를 매핑할 수 없음  
- [ ] 예 — 인트로스펙션이 **활성화**되어 있지만 관리자 인증이 필요함  
- [ ] 예 — 인트로스펙션이 **활성화**되어 있으며 공개적으로 접근 가능함 *(높음)*  

### 쿼리에 리소스 제한(깊이 및 복잡도)이 적용되어 있습니까?
- [ ] 예 — 엄격한 쿼리 깊이 및 복잡도 제한이 **적용** 및 강제됨  
- [ ] 예 — 제한이 **적용**되어 있으나 별칭(Aliases) 또는 프래그먼트(Fragments)를 사용하여 우회할 수 있음  
- [ ] 아니오 — 제한이 **적용되지 않아** 재귀 쿼리 및 DoS가 가능함  

### 리졸버에 객체 수준 권한 관리 미흡(BOLA)이 존재합니까?
- [ ] 아니오 — 모든 쿼리에 대해 필드 및 객체 수준에서 권한 부여가 **적용**됨  
- [ ] 예 — 일부 필드에는 권한 부여가 **적용**되지만 민감한 객체가 노출됨  
- [ ] 예 — 권한 부여가 **적용되지 않아** ID 조작을 통해 모든 객체에 접근 가능함  

### GraphQL 뮤테이션이 무단 사용으로부터 적절히 보호되고 있습니까?
- [ ] 아니오 — 스키마에 뮤테이션이 **존재하지 않음**  
- [ ] 예 — 뮤테이션에 유효한 인증 및 엄격한 권한 부여 확인이 **필요함**  
- [ ] 예 — 인증되지 않은 사용자가 뮤테이션을 **수행 가능**하거나 권한 부여가 부족함  

### 오류 메시지에 민감한 구현 또는 스키마 세부 정보가 유출됩니까?
- [ ] 아니오 — 오류 메시지가 일반적이며 내부 로직을 **비공개**함  
- [ ] 예 — 오류 메시지가 "Did you mean...?"과 같은 제안을 통해 기본 데이터베이스 유형이나 스키마를 **노출함**  
- [ ] 예 — 응답에 전체 스택 트레이스(Stack traces) 및 민감한 설정 데이터가 **노출됨**