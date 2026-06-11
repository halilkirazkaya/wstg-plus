## WSTG-INPV-07 — XML 인젝션 (XML Injection)

XML 인젝션(XML Injection)은 애플리케이션이 XML 메타 문자에 대한 적절한 검증, 정제(Sanitization) 또는 이스케이프(Escaping) 처리 없이 사용자 제공 데이터를 XML 문서나 스트림에 포함할 때 발생합니다. 공격자는 특정 XML 태그나 구조적 요소를 주입함으로써 문서의 로직을 조작하거나, 인증 메커니즘을 우회하고, 백엔드에서 처리되는 데이터의 무결성을 방해할 수 있습니다. 이 취약점은 SOAP 기반 웹 서비스, XML을 수용하는 REST API, SAML 어설션(SAML assertions), 그리고 XML 설정 파일이나 보고서를 동적으로 생성하는 애플리케이션에서 흔히 발견됩니다. 공격자의 관점에서 목표는 의도된 데이터 컨텍스트를 벗어나 문서 구조를 변경하여, 잠재적인 권한 상승(Privilege Escalation)이나 의도하지 않은 비즈니스 로직을 실행하는 것입니다.


| 필드 | 값 |
|---|---|
| **WSTG ID** | WSTG-INPV-07 |
| **CWE** | CWE-91 |
| **테스트 상태** | 수행되지 않음 |
| **위험도** | 높음 / 중간 |

**참고 문헌:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/07-Testing_for_XML_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/xxe  

**도구:** `Burp Suite (Intruder/Repeater)`, `OWASP ZAP`, `XMLSpy`, `CyberChef`

### 애플리케이션이 사용자로부터 XML 형식의 입력을 수락하고 처리합니까?
- [ ] 아니오 — 애플리케이션이 XML 입력을 처리하지 않음  
- [ ] 예 — API (SOAP/REST)를 통해 XML 입력이 처리됨  
- [ ] 예 — 파일 업로드(SVG, DOCX 등)를 통해 XML이 처리됨  

### XML 메타 문자(예: `<`, `>`, `&`, `'`, `"`)가 적절히 이스케이프 또는 정제 처리됩니까?
- [ ] 예 — 모든 메타 문자가 **적절히** 이스케이프 또는 정제됨 *(가장 안전)*  
- [ ] 예 — 일부 필터링이 적용되나 인코딩을 사용하여 **우회가 가능함**  
- [ ] 아니오 — 정제 **없이** 원본 입력이 XML 구조에 직접 포함됨  

### 모든 XML 입력에 대해 서버 측 스키마 검증(XSD/DTD)이 강제됩니까?
- [ ] 예 — 엄격한 스키마 검증이 **활성화**되어 있으며 구조적 변경을 방지함  
- [ ] 예 — 검증이 **활성화**되어 있으나 데이터 필드 내의 태그 주입을 방지하지 **못함**  
- [ ] 아니오 — 스키마 검증이 **비활성화**되어 있거나 구현되지 않음  

### 공격자가 새로운 XML 태그를 주입하여 문서 구조나 로직을 변경할 수 있습니까?
- [ ] 아니오 — 입력에 관계없이 구조가 온전하게 유지됨  
- [ ] 예 — 태그 주입은 가능하나 비즈니스 로직을 변경할 수는 **없음**  
- [ ] 예 — 태그 주입이 **가능하며** 로직 우회 또는 데이터 수정이 가능함 *(심각)*  

### CDATA 섹션 또는 XML 주석을 사용하여 XML 파서(Parser)를 조작할 수 있습니까?
- [ ] 아니오 — 파서가 CDATA/주석 마커를 올바르게 처리하거나 거부함  
- [ ] 예 — 필터를 숨기거나 우회하기 위해 `<![CDATA[...]]>` 또는 `<!-- ... -->` 주입이 **가능함**  

---