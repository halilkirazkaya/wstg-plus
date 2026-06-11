## WSTG-CONF-07 — HTTP Strict Transport Security (HSTS) のテスト

HTTP Strict Transport Security (HSTS) は、Web サーバーがブラウザに対して HTTPS 経由でのみアクセスすべきであることを通知するためのセキュリティメカニズムであり、プロトコルのダウングレード攻撃やクッキーのハイジャックを防止します。暗号化接続を強制することで、HSTS は SSLStrip などの中間者攻撃 (Man-in-the-Middle (MitM) 攻撃) を緩和します。SSLStrip では、攻撃者が最初の安全でない HTTP リクエストを傍受し、TLS への移行を阻止します。攻撃者の視点では、このヘッダーの欠如や短い `max-age` 期間は、初期のクリアテキストによるハンドシェイク (Handshake) 中に機密性の高いセッショントークンや認証情報を傍受する機会を与えます。このテストでは、すべての機密性の高いアプリケーションエンドポイントにおいて、`Strict-Transport-Security` ヘッダーの存在、期間、およびスコープを検証することに焦点を当てます。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-07 |
| **CWE** | CWE-319 |
| **テストステータス** | 未実施 |
| **深刻度** | 低 / 中* |

> *アプリケーションが個人を特定できる情報 (PII)、セッショントークン、または認証情報を扱う場合、HSTS の欠如によって MitM 攻撃の成功率が高まるため、深刻度は「中」になります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `curl`, `Burp Suite`, `nmap`, `SSLLabs`, `hsts-preload-checker`

### HTTPS レスポンスに `Strict-Transport-Security` ヘッダーが含まれているか？
- [ ] はい — すべての機密性の高いエンドポイントにヘッダーが **存在する**  
- [ ] はい — ヘッダーは **存在する** が、ランディングページのみである  
- [ ] いいえ — アプリケーション全体でヘッダーが **欠落している**  

### `max-age` ディレクティブは十分な期間に設定されているか？
- [ ] はい — `max-age` が1年以上（例：`31536000`）に設定されている  
- [ ] はい — `max-age` は設定されているが、**短すぎる**（例：6ヶ月未満）  
- [ ] いいえ — `max-age` ディレクティブが **欠落している** か、`0` に設定されている *(重要)*  

### HSTS ポリシーはすべてのサブドメインをカバーしているか？
- [ ] はい — `includeSubDomains` ディレクティブが **適用されている**  
- [ ] いいえ — `includeSubDomains` ディレクティブが **欠落しており**、サブドメインがダウングレード攻撃に対して脆弱な状態である  
- [ ] いいえ — サブドメインが存在しないため、この機能は適用されない  

### アプリケーションは HSTS プリロード (HSTS Preloading) に登録されているか？
- [ ] はい — `preload` ディレクティブが **存在し**、ドメインが HSTS プリロードリストに含まれている  
- [ ] はい — `preload` ディレクティブは **存在する** が、ドメインはまだプリロードリストに **含まれていない**  
- [ ] いいえ — `preload` ディレクティブが **欠落しており**、初回訪問時に MitM 攻撃を受ける隙がある  

### HSTS ヘッダーがプレーンテキストの HTTP 経由で誤って送信されていないか？
- [ ] いいえ — ヘッダーはセキュアな HTTPS 接続経由でのみ **送信されている**  
- [ ] はい — ヘッダーが HTTP 経由で **送信されている**。これはブラウザによって無視されるが、構成の詳細が漏洩する可能性がある  

---