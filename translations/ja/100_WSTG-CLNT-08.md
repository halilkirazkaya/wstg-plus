## WSTG-CLNT-08 — クロスサイトフラッシング (Cross-Site Flashing / XSF) のテスト

クロスサイトフラッシング (Cross-Site Flashing / XSF) は、Flash (SWF) ファイルがユーザー制御の入力を不適切に処理した際に発生するクライアントサイドの脆弱性です。これにより、攻撃者は悪意のある ActionScript コードを実行したり、ブラウザの JavaScript 環境へブリッジしたりすることが可能になります。`FlashVars` や URL クエリ文字列などのパラメータが `ExternalInterface.call`、`getURL`、または `loadMovie` といったシンク (Sinks) に渡される際、それらを操作することで、攻撃者はセッションハイジャック (Session Hijacking) やデータの外部流出 (Data Exfiltration) を含むアクションを、脆弱なドメインのコンテキストで実行できます。この脆弱性は、主に現在も SWF ファイルをホストしているレガシーなエンタープライズアプリケーションで発見されます。不十分な入力値の検証により、Flash ムービーが クロスサイトスクリプティング (XSS) のベクトルとして悪用されたり、許容的なクロスドメイン設定を介して 同一生成元ポリシー (Same-Origin Policy / SOP) をバイパスしたりするために転用される可能性があります。

| フィールド | 値 |
|---|---|
| **WSTG ID** | WSTG-CLNT-08 |
| **CWE** | CWE-79 |
| **テストステータス** | 未実施 |
| **深刻度** | 中 / 高 |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client-side_Testing/08-Testing_for_Cross_Site_Flashing  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `JPEXS Free Flash Decompiler`, `FFDec`, `Burp Suite`, `Google Dorks`, `strings`

### レガシーな Adobe Flash (.swf) ファイルがアプリケーション上でホストされていますか？
- [ ] いいえ — アプリケーションのスコープ内で Flash ファイルはホストも参照もされていません  
- [ ] はい — SWF ファイルは存在しますが、静的コンテンツのみを提供しています  
- [ ] はい — SWF ファイルが存在し、`FlashVars` または URL 文字列を介して動的パラメータを受け入れます  

### ActionScript のシンクに渡される入力はサニタイズされていますか？
- [ ] はい — すべての入力は厳格に検証されており、ActionScript のシンクを操作することはできません  
- [ ] はい — 検証は適用されていますが、特定のエンコーディング手法によりバイパスが可能です  
- [ ] いいえ — 入力は `ExternalInterface.call` や `getURL` といった重要なシンクに直接渡されています *(緊急)*  

### Flash ファイルを使用して任意の JavaScript (XSS) を実行できますか？
- [ ] いいえ — `ExternalInterface` が無効化されているか、`allowScriptAccess` パラメータが `never` に設定されています  
- [ ] はい — `allowScriptAccess` が `sameDomain` に設定されていますが、SWF がターゲットドメイン上でホストされています  
- [ ] はい — `allowScriptAccess` が `always` に設定されており、任意のドメインからの XSS が可能です  

### `crossdomain.xml` ポリシーは、不正なクロスオリジンリクエストを防止していますか？
- [ ] はい — `crossdomain.xml` は制限的であり、信頼された特定のオリジンのみを許可しています  
- [ ] いいえ — `crossdomain.xml` ファイルが存在しません  
- [ ] はい — ポリシーが過度に許容的です (例: `<allow-access-from domain="*" />`)  

### SWF ファイルに対して、外部の攻撃者が制御するムービーを強制的にロードさせることができますか？
- [ ] いいえ — アプリケーションに外部 SWF ファイルを強制的にロードさせることはできません  
- [ ] はい — `loadMovie` または `loadMovieNum` 関数が検証されていない外部 URL を受け入れます  

---