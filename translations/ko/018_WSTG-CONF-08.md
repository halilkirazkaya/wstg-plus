## WSTG-CONF-08 — RIA 교차 도메인 정책 테스트 (RIA Cross Domain Policy Testing)

리치 인터넷 애플리케이션(Rich Internet Application, RIA) 교차 도메인 정책은 Adobe Flash 및 Microsoft Silverlight와 같은 웹 클라이언트가 교차 출처 요청(Cross-origin requests)을 처리하는 방식을 정의하는 `crossdomain.xml` 및 `clientaccesspolicy.xml`과 같은 파일을 포함합니다. 이 파일들은 동일 출처 정책(Same-Origin Policy, SOP)을 명시적으로 재정의하여 외부 도메인이 애플리케이션과 상호 작용하고 응답을 읽을 수 있도록 허용하는 범위를 결정하므로 보안상 매우 중요합니다. `domain` 속성에 와일드카드(Wildcard)를 사용하는 것과 같이 과도하게 허용적인 설정은 공격자가 제3자 사이트에 악성 RIA 객체를 호스팅하여 민감한 데이터를 탈취(Exfiltrate)하거나 인증된 사용자를 대신해 작업을 수행할 수 있게 합니다. 모의 해킹 전문가(Pentester)는 일반적으로 웹 루트(Web root)에서 이러한 파일을 찾아 제3자 출처에 부여된 신뢰 수준을 평가하고 잠재적인 교차 출처 데이터 유출을 식별합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CONF-08 |
| **CWE** | CWE-942 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 중간(Medium) / 높음(High)* |

> *애플리케이션이 허용적인 정책을 통해 유출될 수 있는 민감한 사용자 데이터나 세션 식별자를 처리하는 경우 위험도는 '높음(High)'이 됩니다.

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `curl`, `Burp Suite`, `FFUF`, `dirsearch`, `Wget`

### 웹 루트에 교차 도메인 정책 파일(`crossdomain.xml` 또는 `clientaccesspolicy.xml`)이 존재합니까?
- [ ] 아니요 — 루트 디렉토리에서 파일을 찾을 수 없음  
- [ ] 예 — `crossdomain.xml` (Flash)이 존재함  
- [ ] 예 — `clientaccesspolicy.xml` (Silverlight)이 존재함  
- [ ] 예 — 두 RIA 정책 파일이 모두 존재함  

### `allow-access-from` 도메인 속성이 신뢰할 수 있는 출처로 제한되어 있습니까?
- [ ] 아니요 — 파일은 존재하지만 `allow-access-from` 항목이 없음  
- [ ] 예 — 특정 신뢰할 수 있는 도메인이 명시적으로 화이트리스트(Whitelist)에 등록되어 있으며 우회할 수 없음  
- [ ] 예 — 도메인이 화이트리스트에 등록되어 있으나 신뢰할 수 없는 제3자 또는 하위 도메인이 포함됨  
- [ ] 예 — 와일드카드 `*`가 사용되어 **모든** 출처에서의 접근을 허용함 *(위험)*  

### 정책이 HTTPS 리소스에 대해 HTTP를 통한 안전하지 않은 통신을 허용합니까?
- [ ] 아니요 — `secure="true"`로 설정되어(또는 기본값) HTTP 출처가 HTTPS 데이터에 접근하는 것을 방지함  
- [ ] 예 — `secure="false"`가 명시적으로 설정되어 중간자 공격(MITM)자가 보안 데이터를 탈취할 수 있음  

### 교차 도메인 설정에서 HTTP 헤더가 과도하게 허용되어 있습니까?
- [ ] 아니요 — `allow-http-request-headers-from`이 제한되어 있거나 활성화되지 않음  
- [ ] 예 — 신뢰할 수 있는 도메인에 대해 특정 헤더가 허용됨  
- [ ] 예 — 헤더에 와일드카드 `*`가 사용되어 공격자가 모든 도메인에서 커스텀 헤더(Custom headers)를 보낼 수 있음  

### `site-control` (마스터 정책) 설정이 제한적입니까?
- [ ] 아니요 — `permitted-cross-domain-policies`가 `none` 또는 `master-only`로 설정됨  
- [ ] 예 — 정책이 서버의 다른 파일이 자체 규칙을 정의하도록 허용함 (`all`)  

---