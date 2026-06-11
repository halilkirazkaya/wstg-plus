## WSTG-CLNT-07 — オリジン間リソース共有 (Cross-Origin Resource Sharing (CORS))

オリジン間リソース共有 (Cross-Origin Resource Sharing (CORS)) は、サーバーが異なるオリジンからのリソースへのアクセスを明示的に許可することを可能にし、同一オリジンポリシー (Same-Origin Policy (SOP)) を効果的に緩和するブラウザベースの仕組みです。設定ミスは、アプリケーションが任意のオリジンを過度に信頼している場合に発生します。これは多くの場合、レスポンスの `Access-Control-Allow-Origin` ヘッダーに `Origin` ヘッダーをそのまま反映させたり、不適切に実装された正規表現のホワイトリストを使用したりすることで起こります。攻撃者の視点からは、許可設定が緩い CORS ポリシーを悪用し、管理下のドメインに悪意のあるスクリプトを配置することで、脆弱なアプリケーションに対して認証済みのリクエストを送信させることが可能です。これにより、攻撃者はレスポンスボディからクロスサイトリクエストフォージェリ (CSRF) トークンや個人情報 (Personally Identifiable Information (PII)) などの機密データを直接抽出 (Exfiltrate) することができます。この脆弱性は、認証済みセッションを処理し、非公開情報を返す API エンドポイントにおいて最も深刻です。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-07 |
| **CWE** | CWE-942 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 / 高 |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/07-Testing_Cross_Origin_Resource_Sharing  
* https://hacktricks.wiki/en/pentesting-web/cors-bypass.html  
* https://portswigger.net/web-security/cors  

**ツール:** `Burp Suite (CORS Raider)`, `curl`, `Postman`, `Corsy`

### アプリケーションは認証済みエンドポイントで CORS ヘッダーを実装していますか？
- [ ] いいえ — CORS は**有効化されていません**。ブラウザは厳格な同一オリジンポリシー (Same-Origin Policy) をデフォルトとして適用します。
- [ ] はい — CORS ヘッダーが存在し、厳格な**ホワイトリスト**に制限されています。
- [ ] はい — CORS ヘッダーが存在しますが、設定が**寛容 (Permissive)** です。

### サーバーは `Access-Control-Allow-Origin` (ACAO) ヘッダーをどのように処理していますか？
- [ ] ACAO は特定の**信頼された**静的ドメインに設定されている
- [ ] ACAO は `*` (ワイルドカード) に設定されており、クレデンシャル (Credentials) は**送信できない**
- [ ] ACAO は `*` (ワイルドカード) に設定されており、クレデンシャルを**送信できる**（注意：ほとんどのブラウザはこれをブロックします）
- [ ] ACAO はリクエストの `Origin` ヘッダーの値を**動的に反映**している

### `Access-Control-Allow-Credentials` (ACAC) ヘッダーは `true` に設定されていますか？
- [ ] いいえ — ACAC は**存在しない**、または `false` に設定されている
- [ ] はい — ACAC は `true` に設定されているが、ACAO は信頼されたオリジンに**制限されている**
- [ ] はい — ACAC は `true` に設定されており、ACAO は任意のオリジンを**反映**している（重要/クリティカル）

### サブドメインや null オリジンの操作によってホワイトリストをバイパス (Bypass) できますか？
- [ ] いいえ — ホワイトリストは厳格に適用されており、**バイパスできません**
- [ ] はい — ホワイトリストはターゲットの**任意の**サブドメインを許可している（例：`attacker.target.com`）
- [ ] はい — ホワイトリストはサンドボックス化された iframe を介した `null` オリジンを許可している
- [ ] はい — ホワイトリストは正規表現のバイパスに対して脆弱である（例：`target.com.attacker.com` や `target.com-safe.com`）

### CORS 設定によって機密データの抽出が可能ですか？
- [ ] いいえ — 公開されているエンドポイントに機密データやセッション固有のデータは**含まれていません**
- [ ] はい — 認証済みエンドポイントが、権限のないオリジンによって読み取り可能な PII、クレデンシャル、または CSRF トークンを返している

---