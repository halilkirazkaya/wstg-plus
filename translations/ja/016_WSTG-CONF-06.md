## WSTG-CONF-06 — HTTPメソッドのテスト (Test HTTP Methods)

HTTPメソッドのテスト（Test HTTP Methods）は、Webサーバーやアプリケーションがどのメソッド（Verbs）をサポートしているかを特定し、必要な機能のみが公開されていることを確認するプロセスです。標準的な `GET` や `POST` 以外にも、サーバーが誤って `PUT`、`DELETE`、`TRACE` といった危険なメソッドを有効にしている場合があります。これにより、攻撃者が悪意のあるファイルをアップロードしたり、既存のコンテンツを削除したり、クロスサイトトレーシング（Cross-Site Tracing (XST)）を介してセッションクッキーを窃取したりすることが可能になります。ペネトレーションテスター（Pentesters）は、`Allow` や `Public` といったレスポンスヘッダーを調査し、メソッドオーバーライド（Method Overriding）技術やHTTP動詞改ざん（Verb Tampering）を用いて制限をバイパスし、未認可の機能へアクセスを試みます。このテストは、攻撃対象領域（Attack Surface）を堅牢化し、サーバーの侵害や不正なデータ操作につながる設定ミスを防止するために不可欠です。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-06 |
| **CWE** | CWE-650 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 低 (Low) / 中 (Medium)* |

> *`PUT` または `DELETE` によって不正なファイル操作が可能な場合、または `TRACE` がセッション・トークンの窃取を容易にする場合、深刻度は「高 (High)」になります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `curl`, `nmap`, `Burp Suite (Repeater)`, `ZAP`, `Metasploit`

### アプリケーション全体で `PUT` や `DELETE` などの危険なHTTPメソッドが、無効化されていますか？
- [ ] はい — 安全なメソッド (GET, POST, HEAD) のみが **有効になっています**  
- [ ] いいえ — `PUT` または `DELETE` が **有効になっています** が、有効な認証が必要です  
- [ ] いいえ — `PUT` または `DELETE` が **有効になっており**、認証なしでアクセス可能です *(重要/Critical)*  

### クロスサイトトレーシング (XST) を防ぐために、`TRACE` メソッドは無効化されていますか？
- [ ] はい — `TRACE` メソッドが **無効化されている** か、405 Method Not Allowed を返します  
- [ ] いいえ — `TRACE` メソッドは **有効です** が、機密性の高いヘッダーを反映しません  
- [ ] いいえ — `TRACE` メソッドが **有効になっており**、`Cookie` や `Authorization` ヘッダーを反映します *(中/Medium)*  

### サーバーは HTTPメソッドオーバーライド (HTTP Method Overriding)（例：`X-HTTP-Method-Override`）をサポートしていますか？
- [ ] いいえ — サーバーはメソッドオーバーライドヘッダーを **処理しません**  
- [ ] はい — サーバーはオーバーライドヘッダーを処理しますが、アクセス制御は **適用されています**  
- [ ] はい — サーバーはオーバーライドヘッダーを処理し、アクセス制御の **バイパスが可能です**  

### サーバーは、任意または不正な形式のHTTPメソッドに対して安全にレスポンスを返しますか？
- [ ] はい — サーバーは 405 Method Not Allowed または 501 Not Implemented を返します  
- [ ] いいえ — `JEFF` や `TEST` といった任意のメソッドに対して、サーバーが 200 OK または 500 Error を返します  
- [ ] いいえ — 任意のメソッドを使用することで、認証または認可のフィルターをバイパスできます  

### `OPTIONS` メソッドは制限されているか、情報漏洩を最小限に抑えるように設定されていますか？
- [ ] はい — `OPTIONS` は無効化されているか、最小限の `Allow` ヘッダーを返します  
- [ ] いいえ — `OPTIONS` により、DAVやデバッグ用の動詞を含む、広範囲の有効なメソッドが明らかになります  

---