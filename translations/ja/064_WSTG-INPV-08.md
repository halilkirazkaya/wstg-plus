## WSTG-INPV-08 — SSI注入のテスト (Testing for SSI Injection)

サーバーサイド・インクルード (SSI) 注入 (SSI Injection) は、アプリケーションがユーザー入力を適切にサニタイズせずに、サーバーのSSIエンジンによって処理されるHTMLファイルに組み込んだ場合に発生します。攻撃者は、`<!--#exec cmd="id" -->` などのSSIディレクティブを注入することで、任意のシェルコマンドの実行、ローカルファイルの読み取り、または環境変数の窃取を試みます。この脆弱性は通常、`.shtml`、`.shtm`、`.stm` といったレガシーなファイル拡張子を使用するアプリケーションや、標準的なHTMLファイル内でSSIを解析するように明示的に設定されたウェブサーバーで見つかります。攻撃に成功すると、攻撃者はウェブサーバーのプロセスと同じ権限を取得し、多くの場合、システム全体の侵害や機密データの露出を招きます。

| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INPV-08 |
| **CWE** | CWE-97 |
| **テストステータス** | 未実施 |
| **深刻度** | 高 (High) |

**参照:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/08-Testing_for_SSI_Injection  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `Burp Suite (Intruder/Repeater)`, `ffuf`, `Wfuzz`, `curl`

### ウェブサーバーはSSIディレクティブをサポートし、処理しますか？
- [ ] いいえ — サーバーはSSIをサポートしていない、または `.shtml` などの拡張子が無効化されている  
- [ ] はい — SSIは有効化されているが、ユーザー入力は処理後のページに反映されない  
- [ ] はい — SSIは有効化されており、ユーザー入力が処理後のページに反映される  

### `#exec` ディレクティブを使用して任意のシステムコマンドを実行できますか？
- [ ] いいえ — `#exec` ディレクティブが無効化されている、またはサーバー設定によって制限されている  
- [ ] はい — `#exec cmd` または `#exec cgi` ペイロード (Payload) を介したコマンド実行が可能である  

### 機密ファイルまたは環境変数の窃取は可能ですか？
- [ ] いいえ — `#include` や `#config` などのディレクティブが無効化されている  
- [ ] はい — `#include virtual` を介してローカルファイル（例: `/etc/passwd`）の読み取りが可能である  
- [ ] はい — `#printenv` または `#echo` を介してサーバーの環境変数を窃取できる  

### 入力値検証 (Input Validation) またはWAFの制御は、SSIペイロードに対して有効ですか？
- [ ] はい — 厳格な入力値検証が適用されており、バイパス (Bypass) は不可能である  
- [ ] はい — 検証またはWAFが適用されているが、文字エンコーディングや異なるSSI構文を使用することでバイパスが可能である  
- [ ] いいえ — SSIに使用される文字 (`<`, `!`, `#`, `-`, `"`) に対する検証やWAFによる保護が存在しない  

---