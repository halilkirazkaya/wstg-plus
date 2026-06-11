## WSTG-CONF-05 — インフラおよびアプリケーションの管理インターフェースの列挙 (Enumerate Infrastructure and Application Admin Interfaces)

管理インターフェース (Administrative Interfaces) は、アプリケーションおよびその基盤となるインフラの管理制御プレーンを構成しており、システム設定、ユーザーデータ、およびサーバー側の操作に対する特権的なアクセスを許可することがよくあります。攻撃者は、ディレクトリ・ブルートフォース (Directory Brute-forcing) や `robots.txt` の分析、あるいは予測可能な URLパターン (URL Patterns) の観察を通じて、これらのインターフェースの列挙を優先的に行い、強力な認証が欠如している、あるいはデフォルトの認証情報 (Default Credentials) に依存している可能性のあるエントリポイントを探し出します。公開されている管理パネル (Admin Panel) の発見は、システム全体の完全な侵害、不正なデータ改ざん、またはサービス停止のリスクを大幅に増大させます。これらのインターフェースは、しばしば非標準ポートやサブドメイン上で見つかるため、自動スキャナーや手動による偵察 (Manual Reconnaissance) の主な標的となります。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-CONF-05 |
| **CWE** | CWE-425 |
| **テストステータス** | 未実施 (Not Performed) |
| **深刻度** | 中 / 高* |

> *多要素認証 (MFA) なしでシステム全体の設定変更、ユーザー管理、またはデータの持ち出しが可能な場合、深刻度は「高」となります。

**リファレンス:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces  
* https://hacktricks.wiki/en/pentesting-web/web-vulnerabilities-methodology.html  

**ツール:** `ffuf`, `dirsearch`, `Gobuster`, `Nmap`, `Wappalyzer`, `Burp Suite (Intruder)`

### 管理インターフェースは、ディレクトリ・ファジング (Directory Fuzzing) や偵察によって発見可能ですか？
- [ ] いいえ — 管理インターフェースは公開されておらず、発見もできません  
- [ ] はい — インターフェースは発見可能ですが、アクセスは内部IP範囲に制限されています  
- [ ] はい — インターフェースは発見可能で、パブリックにアクセス可能です  

### 発見された管理ポータルで認証は強制されていますか？
- [ ] はい — 多要素認証 (MFA) が強制されています  
- [ ] はい — 単要素認証 (Single-factor Authentication) が強制されています  
- [ ] いいえ — インターフェースへのアクセスに認証は不要です *(致命的)*  

### 発見を防ぐために、管理パスは隠蔽されているか、あるいは非標準的なものになっていますか？
- [ ] はい — パスには非標準的、ランダム、または予測不可能な命名が使用されています  
- [ ] いいえ — パスには一般的な命名規則（例: `/admin`, `/manager`, `/console`）が使用されており、容易に推測可能です  

### インターフェースは、機密性の高いシステム機能やアプリケーション機能へのアクセスを提供していますか？
- [ ] いいえ — インターフェースは影響が最小限の「読み取り専用」ステータスダッシュボードです  
- [ ] はい — インターフェースはアプリケーション設定やユーザーデータを変更可能です  
- [ ] はい — インターフェースはシステムレベルのコマンド実行やデータベース管理を実行可能です  

---