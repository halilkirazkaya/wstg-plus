## WSTG-CONF-10 — 서브도메인 탈취 (Subdomain Takeover)

서브도메인 탈취(Subdomain Takeover)는 DNS 레코드(주로 CNAME이지만, 때로는 A 또는 MX 레코드)가 제3자 클라우드 서비스 제공업체(Third-party cloud provider)나 호스팅 서비스에서 더 이상 사용되지 않거나 존재하지 않는 리소스를 가리킬 때 발생합니다. 공격자는 AWS, GitHub, Azure와 같은 제공업체로부터 리소스가 더 이상 점유되지 않았음을 나타내는 특정 오류 메시지를 찾아 이러한 "연결되지 않은(Dangling)" DNS 레코드를 식별합니다. 공격자는 해당 제공업체의 플랫폼에서 방치된 리소스 이름을 등록함으로써 서브도메인에 대한 전체 제어권을 획득하며, 이를 통해 악성 콘텐츠를 호스팅하거나 기본 도메인(Base domain)으로 범위가 지정된 세션 쿠키(Session cookies)를 탈취하고, 콘텐츠 보안 정책(CSP) 또는 교차 출처 리소스 공유(CORS) 보호를 우회할 수 있습니다. 이 취약점은 조직의 정당한 도메인 구조와 관련된 신뢰를 활용하여 파급력이 큰 피싱(Phishing) 및 데이터 탈취를 용이하게 하기 때문에 특히 위험합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-10 |
| **CWE** | CWE-1329 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 높음 / 치명적* |

> *서브도메인이 기본 도메인에서 세션 쿠키를 탈취하거나 SSO/OAuth 리다이렉트를 우회하는 데 사용될 수 있는 경우 심각도는 '치명적(Critical)'이 됩니다.

**참고 자료:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://github.com/EdOverflow/can-i-take-over-xyz  

**도구:** `Subjack`, `SubOver`, `dnscan`, `Amass`, `Nuclei`, `dig`, `nslookup`

### 애플리케이션이 서브도메인을 통해 제3자 호스팅 또는 클라우드 서비스를 이용합니까?
- [ ] 아니요 — 모든 서브도메인이 조직에서 제어하는 IP 대역을 가리킵니다.
- [ ] 예 — 서브도메인이 제3자 제공업체(S3, GitHub Pages, Heroku 등)를 가리킵니다.

### 존재하지 않는 리소스를 가리키는 "연결되지 않은(Dangling)" DNS 레코드가 있습니까?
- [ ] 아니요 — 식별된 모든 DNS 레코드가 활성 상태이며 제어 가능한 리소스로 확인됩니다.
- [ ] 예 — 제공업체에 **더 이상 존재하지 않는** 리소스에 대한 CNAME 또는 A 레코드가 존재합니다.
- [ ] 예 — DNS 레코드가 NXDOMAIN 또는 제공업체별 오류 페이지 *(예: "NoSuchBucket")* 로 확인됩니다.

### 식별된 연결되지 않은 리소스를 성공적으로 점유할 수 있습니까?
- [ ] 아니요 — 제공업체가 방치된 이름의 점유를 방지하는 보호 조치를 갖추고 있거나 계정 인증이 필요합니다.
- [ ] 예 — 임의의 사용자가 제3자 플랫폼에서 해당 리소스 이름을 **등록할 수 있습니다**.

### 기본 도메인이 침해될 수 있는 "도메인(Domain)" 범위의 쿠키를 사용합니까?
- [ ] 아니요 — 쿠키의 범위가 특정 서브도메인으로 엄격히 제한되거나 `Host-Only` 플래그를 사용합니다.
- [ ] 예 — 민감한 쿠키(세션 ID, JWT 등)의 범위가 부모 도메인으로 설정되어 있으며 탈취된 서브도메인을 통해 **유출될 수 있습니다**.

### 서브도메인을 보안 헤더 또는 인증 흐름을 우회하는 데 사용할 수 있습니까?
- [ ] 아니요 — 식별된 서브도메인에 대한 보안 의존성이 없습니다.
- [ ] 예 — 서브도메인이 CSP `script-src` 또는 `connect-src` 지시어의 화이트리스트에 포함되어 있습니다.
- [ ] 예 — 서브도메인이 OAuth 또는 SSO 설정의 유효한 리다이렉트 URI입니다.

---