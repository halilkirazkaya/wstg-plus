## WSTG-SESS-02 — クッキー属性のテスト (Testing for Cookies Attributes)

セッション管理は多くの場合、状態（ステート）を維持するために HTTP クッキー (HTTP cookies) に依存しており、これらのクッキーのセキュリティ属性は攻撃者の主な標的となります。`HttpOnly`、`Secure`、`SameSite` などの属性を省略したり設定ミスをしたりすることで、アプリケーションはクロスサイトスクリプティング (Cross-Site Scripting / XSS) によるセッショントークンの窃取、暗号化されていないチャネルを介した傍受、またはクロスサイトリクエストフォージェリ (Cross-Site Request Forgery / CSRF) 攻撃にさらされることになります。ペネトレーションテスター (Pentesters) は、攻撃者がブラウザのドキュメントオブジェクトモデル (Document Object Model / DOM) からセッション識別子を抽出できるか、あるいはクロスオリジンリクエスト (cross-origin requests) でブラウザにそれらを送信させることができるかどうかを判断するために、これらのフラグを調査します。適切な属性設定は、機密性の高いトークンを許可された安全なコンテキスト内のみに限定するための、多層防御 (defense-in-depth) の基本的な手段です。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-SESS-02 |
| **CWE** | CWE-614 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 (Medium) |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/06-Session_Management_Testing/02-Testing_for_Cookies_Attributes  
* https://hacktricks.wiki/en/pentesting-web/hacking-with-cookies/index.html  

**ツール:** `Burp Suite (Proxy/Scanner)`, `OWASP ZAP`, `Browser Developer Tools (Storage Tab)`, `EditThisCookie`, `curl`

### クッキーフラグ実装の分析
| 属性 | 存在 | 欠如 |
|-----------|:-------:|:-------:|
| `HttpOnly` |  |  |
| `Secure` |  |  |
| `SameSite` |  |  |

### 機密性の高いセッションクッキーに `HttpOnly` フラグが適用されていますか？
- [ ] はい — **すべて**の機密性の高いセッションクッキーに適用されている（最も安全）  
- [ ] はい — 一部のセッションクッキーにのみ適用されている  
- [ ] いいえ — フラグが**適用されていない**ため、クライアントサイドスクリプトによるアクセスが可能である（重大）  

### HTTPS 経由で送信されるクッキーに `Secure` フラグが強制されていますか？
- [ ] はい — TLS 経由で送信される**すべて**のクッキーに適用されている  
- [ ] いいえ — フラグが**適用されていない**ため、平文の HTTP 経由でクッキーが送信される可能性がある  

### CSRF リスクを軽減するために `SameSite` 属性が使用されていますか？
- [ ] はい — セッションクッキーに対して `Strict` または `Lax` に設定されている  
- [ ] はい — `None` に設定されているが、`Secure` フラグが付与されている  
- [ ] いいえ — 属性が**欠如**している、あるいは `Secure` なしで `None` に設定されている  

### `Domain` および `Path` 属性は、必要最小限のスコープに制限されていますか？
- [ ] はい — **特定**のサブドメインおよびアプリケーションパスにスコープが限定されている  
- [ ] いいえ — スコープが**広すぎる**（例：親ドメインやルート `/` に設定されている）ため、攻撃対象領域 (Attack Surface) が増大している  

### 機密性の高いセッションクッキーは、`Expires` または `Max-Age` 属性を介して適切に期限切れになりますか？
- [ ] はい — 適切な有効期限が設定されている  
- [ ] いいえ — クッキーが永続的であり、有効期間が過度に**長い**  
- [ ] いいえ — クッキーはセッション限定だが、サーバー側でのタイムアウト強制が**欠如**している  

---