## WSTG-CLNT-14 — 역 탭내빙 테스트 (Testing for Reverse Tabnabbing)

역 탭내빙(Reverse Tabnabbing)은 `target="_blank"` 속성을 통해 열린 페이지가 `window.opener` 객체를 통해 부모 페이지에 대한 기능적 참조를 유지하는 클라이언트 측 취약점(Client-side vulnerability)입니다. 이를 통해 공격자가 제어하는 대상 사이트는 프로그래밍 방식으로 원래의 신뢰할 수 있는 탭을 악성 URL(일반적으로 자격 증명을 탈취하기 위해 설계된 피싱 페이지(Phishing page))로 리다이렉트(Redirect)할 수 있습니다. 이 공격은 사용자가 원래 탭에 대해 가지는 본질적인 신뢰를 악용하며, 사용자는 방금 떠난 페이지가 다른 도메인으로 이동했다는 사실을 인지하지 못할 가능성이 높습니다. 현대적인 보안 관행에서는 이러한 교차 창 통신을 방지하기 위해 앵커 태그(Anchor tags)에 `rel="noopener"` 또는 `rel="noreferrer"` 속성을 사용하거나, 자바스크립트(JavaScript)에서 `opener` 속성을 null로 설정할 것을 요구합니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **테스트 상태** | 수행되지 않음 |
| **심각도** | 낮음 / 중간* |

> *현대적인 브라우저(Modern browsers)는 이제 `target="_blank"` 사용 시 기본적으로 `rel="noopener"`를 적용하므로 대부분의 경우 심각도는 '낮음'입니다. 애플리케이션이 레거시 브라우저(Legacy browsers)를 대상으로 하거나, `opener` 속성을 null로 설정하지 않고 `window.open()`을 사용하는 경우 심각도는 '중간'이 됩니다.

**참조:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**도구:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### 새 창이나 탭에서 열리는 링크가 존재합니까?
- [ ] 아니오 — `target="_blank"` 또는 그에 상응하는 자바스크립트 기반 리다이렉션을 사용하는 링크가 없습니다.  
- [ ] 예 — `target="_blank"`가 내부의 신뢰할 수 있는 링크에서만 사용됩니다.  
- [ ] 예 — `target="_blank"`가 외부 링크나 사용자 제공 URL에 사용됩니다.  

### 앵커 태그에서 `window.opener` 관계가 제한되어 있습니까?
- [ ] 예 — `target="_blank"`가 포함된 모든 링크에 `rel="noopener"` 또는 `rel="noreferrer"`가 **일관되게 적용**되어 있습니다 (가장 안전).  
- [ ] 예 — 속성이 적용되었지만 고위험 또는 사용자 생성 링크에서 **누락**되었습니다.  
- [ ] 아니오 — 속성이 **적용되지 않아** 하위 페이지가 `window.opener`에 액세스할 수 있습니다.  

### 애플리케이션이 자바스크립트 `window.open()`을 통한 역 탭내빙에 취약합니까?
- [ ] 아니오 — `window.open()`이 사용되지 않거나 `opener` 속성을 명시적으로 null로 설정합니다.  
- [ ] 예 — `window.open()`이 사용되며 하위 창에서 `opener` 참조가 **접근 가능한 상태로 유지**됩니다.  

### 공격자가 새 탭에서 열리는 링크의 목적지를 제어할 수 있습니까?
- [ ] 아니오 — 모든 링크 목적지가 하드코딩되어 있으며 신뢰할 수 있는 도메인을 가리킵니다.  
- [ ] 예 — 링크 목적지가 사용자로부터 제공되며(예: 소셜 미디어 프로필, 바이오 링크 등), 탭내빙 방지를 위한 **필터링(Sanitization)이 수행되지 않습니다.**  

---