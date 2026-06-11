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

---