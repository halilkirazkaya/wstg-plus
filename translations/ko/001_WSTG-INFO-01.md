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