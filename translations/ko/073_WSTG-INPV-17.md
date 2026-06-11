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