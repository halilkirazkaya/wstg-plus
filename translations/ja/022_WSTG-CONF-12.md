## WSTG-CONF-12 — コンテンツセキュリティポリシー (Content Security Policy, CSP)

コンテンツセキュリティポリシー (Content Security Policy, CSP) は、クロスサイトスクリプティング (Cross-Site Scripting, XSS)、クリックジャッキング (clickjacking)、およびデータインジェクション攻撃などのリスクを軽減するために HTTP レスポンスヘッダーを通じて実装される、重要な多層防御メカニズムです。許可される動的リソースのロードを定義することで、適切に構成された CSP は、攻撃者が不正なスクリプトを実行したり、機密データを外部ドメインに送出 (exfiltrate) したりする能力を制限します。ペネトレーションテスターは、ポリシーに過度に寛容なディレクティブが含まれていないか、`'unsafe-inline'` のような安全でないキーワードが使用されていないか、あるいはバイパス (bypass) を容易にする可能性のある安全でない CDN に依存していないかを評価します。脆弱な、あるいは欠落している CSP は、直接的な脆弱性とはなりませんが、二次的な防御レイヤーの提供に失敗することで、インジェクション攻撃が成功した際の影響を大幅に増大させます。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-12 |
| **CWE** | CWE-693 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 / 中 |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy  
* https://hacktricks.wiki/en/pentesting-web/content-security-policy-csp-bypass/index.html  

**ツール:** `Google CSP Evaluator`, `Burp Suite (CSP Pro)`, `Mozilla Observatory`, `ZAP`, `CSP Mitigator`

### アプリケーションのレスポンスにコンテンツセキュリティポリシー (CSP) ヘッダーが存在するか？
- [ ] はい — `Content-Security-Policy` ヘッダーが **存在** し、**適用 (enforced)** されている  
- [ ] はい — テスト用に `Content-Security-Policy-Report-Only` が **存在** する  
- [ ] いいえ — レスポンスに CSP ヘッダーが **存在しない**  

### `script-src` または `default-src` ディレクティブは適切に制限されているか？
- [ ] はい — ディレクティブは厳格なホワイトリスト、nonce、またはハッシュを使用しており、バイパスは **不可能** である  
- [ ] はい — ディレクティブは **適切に構成** されているが、ホワイトリストにバイパス可能な既知の CDN が含まれている  
- [ ] いいえ — ディレクティブにワイルドカード (`*`) または `data:` スキームが使用されており、バイパスが **可能** である  

### ポリシーは安全でないインラインスクリプトまたは eval の実行を許可しているか？
- [ ] いいえ — `'unsafe-inline'` および `'unsafe-eval'` は **存在しない**  
- [ ] はい — `'unsafe-inline'` は **存在** するが、`nonce` または `hash` によって保護されている  
- [ ] はい — 追加の保護なしで `'unsafe-inline'` または `'unsafe-eval'` が **有効** になっている *(重要)*  

### アプリケーションは `frame-ancestors` ディレクティブによってクリックジャッキングから保護されているか？
- [ ] はい — `frame-ancestors` が `'none'` または `'self'` に設定されている  
- [ ] はい — `frame-ancestors` が **有効** であり、特定の信頼されたサードパーティドメインを許可している  
- [ ] いいえ — `frame-ancestors` が **欠落** しており、レガシーな `X-Frame-Options` に依存しているか、保護されていない  

### CSP 違反を報告するメカニズムはあるか？
- [ ] はい — `report-uri` または `report-to` が **構成** され、アクティブである  
- [ ] いいえ — 違反レポートが **無効** であるか、**構成されていない**  

---