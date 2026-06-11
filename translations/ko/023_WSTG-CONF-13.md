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