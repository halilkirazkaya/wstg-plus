## WSTG-CLNT-09 — クリックジャッキング (Clickjacking) のテスト

クリックジャッキング (Clickjacking) は、UI再構成 (UI Redressing) とも呼ばれ、攻撃者が透明または不透明なレイヤーを使用して、ユーザーが最前面のページをクリックしようとした際に、別のページ上のボタンやリンクを騙してクリックさせる攻撃です。この脆弱性はユーザー操作の整合性を損ない、被害者に代わって不正なデータ改ざん、アカウント情報の変更、または資金移動が実行される可能性があります。攻撃者は通常、制御下にある悪意のあるサイト内の `<iframe>` に標的となるウェブサイトを埋め込み、その上に欺瞞的なコンテンツや魅力的な誘い文句を重ねることで、この脆弱性を悪用します。これは、「アカウント削除」ボタン、パスワードリセットフォーム、または管理設定パネルなど、機密性の高い状態変更を伴う操作を行うページで最も頻繁に発生します。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-09 |
| **CWE** | CWE-1021 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 / 高* |

> *クリックジャッキングを通じて機密性の高い操作（例：資金移動、アカウント削除、または権限昇格 (Privilege Escalation)）を誘発できる場合、深刻度は「高」となります。

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/09-Testing_for_Clickjacking  
* https://hacktricks.wiki/en/pentesting-web/clickjacking.html  
* https://portswigger.net/web-security/clickjacking  

**ツール:** `Burp Suite Clickbandit`, `Browser Developer Tools`, `OWASP ZAP`, `Online Clickjack Test`

### アプリケーションは、許可されていないフレーミングを防止するためのサーバー側ヘッダーを実装していますか？
- [ ] はい — `X-Frame-Options` または `Content-Security-Policy` **が適用されており**、フレーミングを防止している（最も安全）  
- [ ] はい — ヘッダーは存在するが、**設定不備がある**（例：広範なブラウザサポートを欠く `X-Frame-Options: ALLOW-FROM` など）  
- [ ] いいえ — フレーミング保護ヘッダーが**適用されていない**  

### `Content-Security-Policy` (CSP) の `frame-ancestors` ディレクティブは実装されていますか？
- [ ] はい — `frame-ancestors 'none'` または `'self'` **が適用されている**  
- [ ] はい — `frame-ancestors` は存在するが、信頼できない、あるいは広範すぎるオリジンを**許可している**  
- [ ] いいえ — ディレクティブが**欠落している**、あるいは CSP が実装されていない  

### `X-Frame-Options` (XFO) ヘッダーは、レガシーブラウザのサポートのために正しく設定されていますか？
- [ ] はい — `DENY` または `SAMEORIGIN` **が適用されている**  
- [ ] はい — XFO は存在するが、CSP の `frame-ancestors` ディレクティブが存在するため、モダンブラウザでは**無視される**  
- [ ] いいえ — XFO ヘッダーが**欠落している**、または安全でない値が設定されている  

### 既知の手法を用いてフレーミング保護をバイパス (Bypass) できますか？
- [ ] いいえ — 標準的な手法（二重フレーミング (Double-framing) など）によるバイパスは**不可能**  
- [ ] はい — `frame-ancestors` のホワイトリストにおける脆弱な正規表現やロジックにより、保護を**バイパスできる**  
- [ ] はい — アプリケーションが JavaScript ベースのフレームバスター (Frame-busting) のみに依存しているため、保護を**バイパスできる**  

### 機密性の高い操作が、クリックジャッキングの実証コード (Proof-of-Concept) を通じて悪用可能ですか？
- [ ] いいえ — フレーミングがブロックされている、または機密性の高い操作に到達できない  
- [ ] はい — UI再構成は**可能である**が、複雑なユーザーインタラクションを必要とする  
- [ ] はい — UI再構成が**可能であり**、即座に状態変更を伴う操作が実行できる（重要 / Critical）  

---