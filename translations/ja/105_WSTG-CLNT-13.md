## WSTG-CLNT-13 — クロスサイト・スクリプト・インクルージョン (Cross-Site Script Inclusion; XSSI) のテスト

クロスサイト・スクリプト・インクルージョン (Cross-Site Script Inclusion; XSSI) は、ウェブアプリケーションが動的に生成された JavaScript ファイルや JSONP (JSON with Padding) レスポンスの中に機密データを出力し、攻撃者が制御するサイトが `<script>` タグを介してそれを取り込める場合に発生します。スクリプトの取り込みは同一生成元ポリシー (Same-Origin Policy; SOP) をバイパスするため、攻撃者は自身のコンテキストでスクリプトを実行し、グローバルオブジェクトや変数を上書きして機密情報を奪取 (Exfiltrate) することが可能です。この脆弱性は通常、メールアドレス、API キー、セッションメタデータなどのユーザー固有のデータをスクリプト形式で返す、認証が必要なエンドポイントで顕在化します。攻撃者の視点では、脆弱なスクリプトを参照し、結果として生じるグローバル変数の変更や関数呼び出しをキャプチャする悪意のあるページを作成することで、エクスプロイト (Exploit) が行われます。

| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-13 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 中 (Medium) / 高 (High) |

> *深刻度は、漏洩したデータに CSRF トークン、セッション識別子、または非常に機密性の高い個人を特定できる情報 (Personally Identifiable Information; PII) が含まれる場合に「高 (High)」となります。

**リファレンス:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/13-Testing_for_Cross_Site_Script_Inclusion  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Burp Suite`, `JSParser`, `LinkFinder`, `Browser Developer Tools`

### 認証が必要なエンドポイントが、機密情報を含む動的な JavaScript または JSONP を返していますか？
- [ ] いいえ — すべてのスクリプトは静的であり、ユーザー固有の情報を含めることは**できません**  
- [ ] はい — 動的なスクリプトは存在しますが、機密情報は含まれて**いません**  
- [ ] はい — 動的なスクリプトに PII またはトークンが含まれており、オリジンを跨いでアクセス**可能です**  

### 機密性の高いスクリプトのレスポンスに `X-Content-Type-Options: nosniff` ヘッダーが実装されていますか？
- [ ] はい — ヘッダーが**適用**されており、MIME タイプ・スニッフィング (MIME-type sniffing) を防止しています  
- [ ] いいえ — ヘッダーが**欠落**しており、スクリプト以外のコンテンツがスクリプトとして実行される可能性があります  

### 機密性の高いスクリプトは、実行不能なプレフィックスやカスタムヘッダーなどの対 XSSI メカニズムによって保護されていますか？
- [ ] はい — スクリプトには、標準の `<script>` タグでは送信**できない**カスタムヘッダーまたはトークンが必要です  
- [ ] はい — スクリプトは、容易にバイパス**できない**実行不能なプレフィックス (non-executable prefixes) (例: `)]}'`) を使用しています  
- [ ] いいえ — スクリプトは、アンビエント・クレデンシャル (ambient credentials) を使用した GET リクエストを介して直接アクセス可能です *(重要/Critical)*  

### グローバル変数やプロトタイプを上書きすることで、機密データの奪取が可能ですか？
- [ ] いいえ — 機密データはローカルスコープに限定されているか、現代的なブラウザ機能によって保護されています  
- [ ] はい — 機密データがグローバル変数に割り当てられており、キャプチャ**可能です**  
- [ ] はい — インターセプト**可能な** JSONP コールバックを通じてデータが漏洩しています  

---