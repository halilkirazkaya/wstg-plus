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