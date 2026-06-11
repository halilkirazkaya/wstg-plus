## WSTG-ATHN-01 — 暗号化チャネルを介して転送される資格情報のテスト (Testing for Credentials Transported over an Encrypted Channel)

暗号化チャネルを介して転送される資格情報 (Credentials) のテストでは、ユーザー名、パスワード、セッショントークン (Session tokens) などの機密性の高い認証データが、転送中に傍受されないよう保護されていることを確認します。パブリック Wi-Fi での中間者攻撃 (Man-in-the-Middle (MitM) attacks) や侵害された内部ネットワークなどに配置された攻撃者は、TLS/SSL が適切に強制されていない場合、パケットスニッフィング (Packet sniffing) ツールを使用してクリアテキスト (Cleartext) の資格情報を取得できます。この脆弱性は、通常、アプリケーションが HTTPS を強制しなかったり、HTTP からの不適切なリダイレクト (Redirect) を行ったりするログインエンドポイント (Login endpoints)、パスワードリセットフォーム、API 認証ヘッダーで発生します。悪用に成功すると、攻撃者はユーザーアカウントへのフルアクセス権を取得し、ユーザーデータの完全な侵害や、アプリケーション内での横展開 (Lateral movement) を引き起こす可能性があります。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-ATHN-01 |
| **CWE** | CWE-319 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  
* https://portswigger.net/web-security/authentication  

**ツール:** `Wireshark`, `tcpdump`, `Burp Suite`, `OWASP ZAP`, `testssl.sh`, `sslyze`, `nmap`

### 認証プロセス全体で HTTPS が強制されていますか？
- [ ] はい — HSTS (HTTP Strict Transport Security) が有効化されており、すべてのトラフィックが厳密に HTTPS 経由で強制されている  
- [ ] はい — HTTPS へのリダイレクトは行われるが、HSTS は有効化されていない  
- [ ] いいえ — 暗号化されていない HTTP 経由で資格情報を送信できる  

### ログインページやフォームは暗号化チャネルを介して提供されていますか？
- [ ] はい — ログインフォームを含むページが HTTPS 経由で配信されている  
- [ ] いいえ — ログインページが HTTP 経由で提供されており、フォームアクションのハイジャックや資格情報のスニッフィングが可能である  

### すべての機密性の高いセッションクッキーに `Secure` 属性が適用されていますか？
- [ ] はい — すべての認証クッキーおよびセッションクッキーに `Secure` 属性が適用されている  
- [ ] はい — 一部のクッキーにのみ `Secure` 属性が適用されている  
- [ ] いいえ — `Secure` 属性が適用されておらず、暗号化されていないリクエストを介してクッキーが流出する可能性がある  

### サーバーは脆弱な TLS バージョンや安全でない暗号スイートをサポートしていますか？
- [ ] いいえ — 最新の TLS (1.2 以上) および強力な暗号スイートのみが有効化されている  
- [ ] はい — レガシーバージョン (TLS 1.0/1.1) や脆弱な暗号スイート (RC4, DES, CBC) がサポートされている  

### アプリケーションは、暗号化されていないチャネルを介したサードパーティドメインへの資格情報送信を防止していますか？
- [ ] はい — 外部ドメインに機密データが送信されない、またはすべての外部呼び出しが HTTPS 経由で強制されている  
- [ ] いいえ — 認証ヘッダーや資格情報が HTTP 経由でサードパーティのエンドポイント (例: アナリティクスや SSO (Single Sign-On)) に送信されている  

---