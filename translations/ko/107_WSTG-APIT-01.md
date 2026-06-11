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