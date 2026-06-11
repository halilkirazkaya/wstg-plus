## WSTG-INPV-12 — コマンドインジェクション (Command Injection)

コマンドインジェクション (Command Injection) は、アプリケーションがフォーム入力、HTTPヘッダー (HTTP headers)、クッキー (Cookies) などの安全でないユーザー提供データをシステムシェルに渡すことで発生します。この脆弱性により、攻撃者は通常、Webアプリケーションプロセスの権限でサーバー上の任意のオペレーティングシステムコマンドを実行できます。攻撃者の視点からは、セミコロン、パイプ、バッククォートなどのシェルのメタ文字 (Shell metacharacters) を使用して、意図された実行フローに悪意のあるコマンドを連結することで悪用 (Exploit) されます。その影響はしばしば壊滅的であり、システムの完全な侵害、不正なデータの流出 (Data exfiltration)、または内部ネットワーク内でのラテラルムーブメント (Lateral movement) につながります。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INPV-12 |
| **CWE** | CWE-78 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 緊急 (Critical) |

**参照先:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection  
* https://hacktricks.wiki/en/pentesting-web/command-injection.html  
* https://portswigger.net/web-security/os-command-injection  

**ツール:** `Commix`, `Burp Suite (Intruder/Repeater)`, `dnsbin`, `Interactsh`, `Collaborator`

### アプリケーションはシステムレベルのコマンドまたはシェルの実行ファイルを呼び出していますか？
- [ ] いいえ — アプリケーションは OS シェルと相互作用しません  
- [ ] はい — アプリケーションはシステムタスクに内部 API または高レベルライブラリを使用しています  
- [ ] はい — アプリケーションは `system()`、`exec()`、または `popen()` などのシステムコールを介してシェルコマンドを直接呼び出しています  

### ユーザー入力はシステムコールに渡される前にバリデーションまたはサニタイズされていますか？
- [ ] はい — 厳格な許可リスト (Allow-listing) と入力バリデーション (Input Validation) が適用されています  
- [ ] はい — サニタイズ (Sanitization) またはエスケープが使用されていますが、バイパス (Bypass) が可能です  
- [ ] いいえ — ユーザー入力はバリデーションなしで直接シェルに渡されています  

### シェルのメタ文字を使用してコマンドの実行フローを操作できますか？
- [ ] いいえ — メタ文字は正しくフィルタリングまたはエスケープされています  
- [ ] はい — コマンド出力がレスポンスに反映されるインバンド (In-band) 実行が可能です  
- [ ] はい — 出力が反映されないブラインド (Blind) 実行が可能です  

### ホストからのアウトオブバンド (Out-of-band / OOB) によるデータ流出は可能ですか？
- [ ] いいえ — サーバーに外部への通信手段 (Egress) がないか、OOB シグナル (DNS/HTTP) が失敗します  
- [ ] はい — DNS または HTTP ベースの OOB シグナルを介したデータの流出が可能です  

### アプリケーションプロセスは昇格したシステム権限で実行されていますか？
- [ ] いいえ — アプリケーションは専用の低権限サービスアカウントで実行されています  
- [ ] はい — アプリケーションは `root`、`Administrator`、または高権限ユーザーとして実行されています *(緊急)*  

---