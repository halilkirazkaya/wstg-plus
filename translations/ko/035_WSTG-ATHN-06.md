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