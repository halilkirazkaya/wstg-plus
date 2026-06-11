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