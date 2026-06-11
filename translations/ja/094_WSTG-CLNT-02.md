## WSTG-CLNT-02 — JavaScript実行のテスト

JavaScript実行のテストは、アプリケーションがユーザー提供データを不適切に処理し、被害者のブラウザコンテキスト内で不正なスクリプトが実行される箇所を特定することに焦点を当てています。この脆弱性は通常、クロスサイトスクリプティング (Cross-Site Scripting (XSS)) を引き起こし、攻撃者がセッションクッキーの窃取、ドキュメントオブジェクトモデル (Document Object Model (DOM)) の操作、またはユーザーに代わってアクションを実行することを可能にします。ペネトレーションテスターは、URLフラグメント、`localStorage`、または `window.name` といったソース (Sources) からのサニタイズされていないデータが処理される可能性のある、`innerHTML`、`document.write()`、`eval()` などのシンク (Sinks) を調査します。攻撃の成功は、ユーザーセッションの整合性と機密性を損ない、より複雑なクライアントサイド攻撃の起点となる可能性があります。


| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-02 |
| **CWE** | CWE-79 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 (High) |

**参考資料:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/02-Testing_for_JavaScript_Execution  
* https://hacktricks.wiki/en/pentesting-web/xss-cross-site-scripting/index.html  
* https://portswigger.net/web-security/dom-based  
* https://portswigger.net/web-security/prototype-pollution  

**ツール:** `Burp Suite`, `Browser Developer Tools`, `XSStrike`, `DOMPurify`, `KNOXSS`

### アプリケーションは、危険なJavaScriptシンクでユーザー提供データを処理していますか？
- [ ] いいえ — すべてのデータはサニタイズされているか、`textContent` のような安全なシンクで使用されている  
- [ ] はい — データは危険なシンクで処理されているが、サニタイズまたはエンコーディング (Encoding) が適用されている  
- [ ] はい — データは危険なシンクで処理されており、サニタイズが適用されていない  

### スクリプト実行を緩和するためのコンテンツセキュリティポリシー (Content Security Policy (CSP)) は存在しますか？
- [ ] はい — 制限的なCSPが有効化されており、バイパス (Bypass) は不可能である  
- [ ] はい — CSPは有効化されているが、`unsafe-inline` や安全でないCDNを介したバイパスが可能である  
- [ ] いいえ — CSPは有効化されていない  

### URLパラメータやフラグメントを介してJavaScriptを実行できますか (DOMベースのXSS (DOM-based XSS))？
- [ ] いいえ — 入力は適切にエンコードされており、実行は不可能である  
- [ ] はい — 入力はDOMに反映されるが、モダンなフレームワークの保護機能により実行が阻止されている  
- [ ] はい — 入力がブラウザで直接実行され、エクスプロイト (Exploit) が可能である  

### イベントハンドラ (Event handlers) やURIスキームはスクリプトインジェクションに対して脆弱ですか？
- [ ] いいえ — 入力はサニタイズされ、イベント属性や `javascript:` スキームは削除されている  
- [ ] はい — イベントハンドラを注入することは可能だが、フィルタによってペイロード (Payload) の複雑さが制限されている  
- [ ] はい — `onerror`、`onload`、または `href` 属性を介した任意のJavaScript実行が可能である  

---