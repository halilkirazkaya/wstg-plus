## WSTG-CLNT-01 — DOMベースのクロスサイトスクリプティング（DOM Based Cross Site Scripting）のテスト

DOMベースのクロスサイトスクリプティング（DOM XSS）は、アプリケーションに、信頼できないソースからのデータを安全でない方法で処理するクライアントサイドのJavaScriptが含まれている場合に発生します。通常、危険なシンク（Sink）を介してデータをドキュメントオブジェクトモデル（DOM）に書き戻すことによって発生します。反射型（Reflected）や蓄積型（Stored）のクロスサイトスクリプティング（XSS）とは異なり、この脆弱性は完全にクライアントサイドのコード内に存在します。ペイロード（Payload）は、`innerHTML`、`eval()`、`document.write()` などのシンクによって処理されるため、サーバーには一切送信されないことがよくあります。攻撃者は、悪意のあるフラグメントやパラメータを含むURLを作成することでこれを悪用し、被害者のブラウザで読み込まれた際に、ユーザーセッションのコンテキストで任意のJavaScriptを実行させます。これにより、セッショントークンの漏洩、機密データの窃取、認証済みユーザーに代わって行われる不正な操作につながる可能性があります。

| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-01 |
| **CWE** | CWE-79 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 (High) |

**参照先:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/01-Testing_for_DOM-based_Cross_Site_Scripting  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/dom-xss.html  
* https://portswigger.net/web-security/cross-site-scripting  
* https://portswigger.net/web-security/dom-based  

**ツール:** `Burp Suite (DOM Invader)`, `Browser DevTools`, `KNOXSS`, `XSStrike`, `Snyk (SAST)`

### JavaScriptでユーザー制御のデータを取得するために、信頼できないソースが使用されていますか？
- [ ] いいえ — JavaScriptは `location.hash`、`location.search`、または `document.referrer` を使用して**いません**。
- [ ] はい — 信頼できないソースが使用されていますが、データは実行シンクに渡されて**いません**。
- [ ] はい — 信頼できないソースが使用されており、データがクライアントサイドのロジックに直接渡されています。

### ユーザー制御のデータが危険なDOMシンクに渡されていますか？
- [ ] いいえ — `innerHTML`、`outerHTML`、`document.write()`、または `eval()` などのシンクは使用されて**いません**。
- [ ] はい — 危険なシンクが存在しますが、データは `DOMPurify` のような安全なライブラリを使用して厳格にサニタイズ（Sanitize）されています。
- [ ] はい — 危険なシンクが存在し、データが不十分な、または独自の正規表現ベースのフィルタリングで処理されています。
- [ ] はい — 危険なシンクが存在し、データがサニタイズやエンコード（Encoding）**なし**で渡されています *(致命的/Critical)*。

### URLベースのペイロードを介して実行シンクに到達可能ですか？
- [ ] いいえ — ソースからシンク（Source-to-sink）へのフローを外部入力からトリガーすることは**できません**。
- [ ] はい — フローは**可能**ですが、複雑なユーザー操作が必要です。
- [ ] はい — フローは**可能**であり、単純なURLフラグメントやクエリパラメータを介してトリガーできます。

### 最新のセキュリティヘッダーやブラウザベースの緩和策によってリスクが軽減されていますか？
- [ ] はい — `script-src` および `trusted-types` を含む厳格なコンテンツセキュリティポリシー（CSP: Content-Security-Policy）が**有効**であり、実行を防止しています。
- [ ] はい — CSPは存在しますが、`unsafe-inline` や脆弱なハッシュによりバイパス（Bypass）が**可能**です。
- [ ] いいえ — DOMベースの実行を緩和するためのCSPが存在しません。

### アプリケーションは組み込みの保護機能を備えたクライアントサイドフレームワークを使用していますか？
- [ ] はい — フレームワーク（例：Angular, React）がデータを自動的にエンコードしており、バイパスは**不可能**です。
- [ ] はい — フレームワークは使用されていますが、`dangerouslySetInnerHTML` のような「エスケープハッチ（Escape hatches）」が**有効**になっています。
- [ ] いいえ — アプリケーションはバニラJavaScript（Vanilla JavaScript）または自動エスケープ機能のないレガシーライブラリ（古いバージョンの `jQuery` など）を使用しています。

---