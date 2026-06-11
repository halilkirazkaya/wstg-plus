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