## WSTG-INPV-16 — HTTPリクエストスマグリング

HTTPリクエストスマグリング (HTTP Request Smuggling; HRS) は、ロードバランサとバックエンドWebサーバなどの一連のHTTPサーバが、主に `Content-Length` ヘッダと `Transfer-Encoding` ヘッダの競合により、リクエストの境界を異なって解釈した場合に発生します。攻撃者はこの解釈の不一致（デシンクロナイゼーション）を悪用して、バックエンドサーバのリクエストバッファにエントリを「スマグリング（密かに挿入）」し、実質的に次の正当なユーザのリクエストの先頭に攻撃者が制御するセグメントを付加します。この手法により、セッションハイジャック (Session Hijacking)、セキュリティ制御のバイパス、キャッシュポイズニング (Cache Poisoning) などの深刻な攻撃が可能になります。悪用の判定は、通常、タイミングベースの分析、または後続のリクエストを送信した際のバックエンドからの予期しないレスポンスを観察することによって行われます。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INPV-16 |
| **CWE** | CWE-444 |
| **テストステータス** | 未実施 |
| **深刻度** | 緊急 (Critical) / 高 (High) |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/16-Testing_for_HTTP_Request_Smuggling  
* https://hacktricks.wiki/en/pentesting-web/http-request-smuggling/index.html  
* https://portswigger.net/web-security/request-smuggling  

**ツール:** `Burp Suite (HTTP Request Smuggler extension)`, `Turbo Intruder`, `curl`, `SmuggleHunter`

### フロントエンドとバックエンドのサーバは、`Transfer-Encoding: chunked` ヘッダを一貫して処理していますか？
- [ ] はい — 両方のサーバがチャンク形式のエンコーディング (Chunked Encoding) を同一に処理しており、デシンクロナイゼーションは**不可能**です。
- [ ] いいえ — サーバ間でヘッダの解釈が異なりますが、サニタイズまたは正規化が**適用されています**。
- [ ] いいえ — CL.TE (Content-Length/Transfer-Encoding) のデシンクロナイゼーションが**可能**です *(緊急)*  
- [ ] いいえ — TE.CL (Transfer-Encoding/Content-Length) のデシンクロナイゼーションが**可能**です *(緊急)*  

### 攻撃者はスマグリングされたリクエストを使用して、サーバ側またはクライアント側のキャッシュをポイズニングできますか？
- [ ] いいえ — キャッシュが**無効**であるか、ポイズニングの影響を受けません。
- [ ] はい — スマグリングされたリクエストにより、ユーザを悪意のあるドメインにリダイレクトしたり、キャッシュポイズニングを介してスクリプトを注入したりすることが**可能**です。

### リクエストの連結により、他のユーザのリクエストやセッショントークンを奪取することは可能ですか？
- [ ] いいえ — スマグリングによってユーザ間のデータが露出することはありません。
- [ ] はい — スマグリングにより、次のユーザのリクエストを攻撃者が制御する `POST` ボディに付加させることができ、データの抽出 (Exfiltration) が**可能**になります。

### インフラストラクチャは HTTP/2 を使用していますか？また、H2.CL または H2.TE のデシンクロナイゼーションの影響を受けますか？
- [ ] いいえ — アプリケーションは HTTP/1.1 のみを使用しているか、HTTP/2 がダウングレードなしで安全に実装されています。
- [ ] はい — HTTP/2 から HTTP/1.1 へのクリアテキストへのダウングレードが発生しており、デシンクロナイゼーションが**可能**です。

### 不正な形式のヘッダ（例：スペースを含む `Transfer-Encoding:  chunked`）は安全に処理されていますか？
- [ ] はい — 不正な形式のヘッダは、すべての階層で拒絶されるか正規化されます。
- [ ] いいえ — 不正な形式または難読化されたヘッダの処理が不一致であるため、TE.TE のデシンクロナイゼーションが**可能**です。

---