## WSTG-CLNT-14 — リバースタブナビング（Reverse Tabnabbing）のテスト

リバースタブナビング（Reverse Tabnabbing）は、`target="_blank"` を使用して開かれたページが、`window.opener` オブジェクトを介して親ページへの機能的な参照を保持するクライアントサイドの脆弱性です。これにより、攻撃者が制御する遷移先サイトが、プログラムによって元の信頼されたタブを悪意のある URL（通常は資格情報（Credentials）を収集するために設計されたフィッシングページ）にリダイレクトさせることが可能になります。この攻撃は、ユーザーが元のタブに対して抱いている固有の信頼を悪用します。ユーザーは、直前まで閲覧していたページが別のドメインに遷移したことに気づきにくいためです。最新のセキュリティプラクティスでは、アンカータグに `rel="noopener"` または `rel="noreferrer"` 属性を使用するか、JavaScript で `opener` プロパティを null に設定することで、このウィンドウ間通信を防止することが求められます。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-14 |
| **CWE** | CWE-1022 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 (Low) / 中 (Medium)* |

> *最新のブラウザの多くでは、現在 `target="_blank"` のデフォルト動作が `rel="noopener"` に設定されているため、深刻度は「低」となります。アプリケーションがレガシーブラウザを対象としている場合、または `opener` プロパティを null に設定せずに `window.open()` を使用している場合、深刻度は「中」となります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/14-Testing_for_Reverse_Tabnabbing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Browser DevTools (Inspector)`, `Burp Suite (Repeater)`, `ZAP`

### 新しいウィンドウまたはタブで開くリンクが存在するか？
- [ ] いいえ — `target="_blank"` または同等の JavaScript ベースのリダイレクトを使用しているリンクはありません。  
- [ ] はい — 内部の信頼されたリンクのみで `target="_blank"` が使用されています。  
- [ ] はい — 外部リンクまたはユーザーが提供した URL で `target="_blank"` が使用されています。  

### アンカータグにおいて `window.opener` の関係が制限されているか？
- [ ] はい — `target="_blank"` を持つすべてのリンクに `rel="noopener"` または `rel="noreferrer"` が**一貫して適用されています**（最も安全）。  
- [ ] はい — 属性は適用されていますが、高リスクなリンクやユーザー生成リンクで**欠落しています**。  
- [ ] いいえ — 属性が**適用されておらず**、子ページが `window.opener` にアクセス可能です。  

### JavaScript の `window.open()` を介して、アプリケーションにリバースタブナビングの脆弱性があるか？
- [ ] いいえ — `window.open()` は使用されていないか、あるいは明示的に `opener` プロパティを null に設定しています。  
- [ ] はい — `window.open()` が使用されており、子ウィンドウから `opener` 参照が**引き続きアクセス可能です**。  

### 攻撃者が新しいタブで開くリンクの遷移先を制御できるか？
- [ ] いいえ — すべてのリンク先はハードコードされており、信頼されたドメインを指しています。  
- [ ] はい — リンク先がユーザー提供（例：ソーシャルメディアのプロフィール、プロフィール欄のリンクなど）であり、タブナビング対策のための**サニタイズ（Sanitization）が行われていません**。  

---