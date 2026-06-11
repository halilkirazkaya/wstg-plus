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