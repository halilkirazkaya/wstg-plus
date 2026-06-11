## WSTG-INFO-04 — ウェブサーバー上のアプリケーションの列挙 (Enumerate Applications on Webserver)

アプリケーションの列挙 (Application enumeration) とは、単一のウェブサーバーまたは IP アドレス上でホストされているすべてのウェブアプリケーションおよびサービスを特定するプロセスです。現代のウェブサーバーは、複数のバーチャルホスト (Virtual Hosts)、レガシーなステージング環境、または標準外のポートやパスにある管理インターフェースをホストしていることが多いため、これらの隠れた資産を特定できない場合、攻撃対象領域 (Attack Surface) の大部分がテストされないまま残ることになります。攻撃者は、DNS ゾーン転送 (DNS Zone Transfers)、バーチャルホストに対するブルートフォース (Brute Force)、逆引き IP 検索 (Reverse IP Lookups) などの手法を使用して、これらの忘れ去られた、あるいは隠蔽されたエントリポイントを発見します。これらは多くの場合、主要な本番環境のアプリケーションよりもセキュリティが脆弱です。


| 項目 | 値 |
|---|---|
| **WSTG ID** | WSTG-INFO-04 |
| **CWE** | CWE-200 |
| **テストステータス** | 未実施 |
| **深刻度** | 情報 (Informational) / 低 (Low) |

**参考文献:**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/01-Information_Gathering/04-Attack_Surface_Identification  
* https://hacktricks.wiki/en/generic-methodologies-and-resources/external-recon-methodology/index.html  

**ツール:** `nmap`, `ffuf`, `gobuster`, `theHarvester`, `Dig`, `DNSrecon`, `Reverse IP Lookups`, `Shodan`

### 対象のインフラストラクチャに複数の IP アドレスまたは DNS エイリアス (DNS Aliases) が関連付けられていますか？
- [ ] いいえ — 単一の IP と A レコードのみが存在する  
- [ ] はい — 複数の IP アドレスまたはエイリアスが特定され、スコープが拡大した  

### ウェブサーバーは同じ IP 上で複数のバーチャルホスト (Virtual Hosts) をホストしていますか？
- [ ] いいえ — プライマリドメインのみがサーバーによって提供されている  
- [ ] はい — 追加のバーチャルホストが特定されたが、アクセスはできない（例：403 Forbidden）  
- [ ] はい — 隠された、開発用、またはステージング用のバーチャルホストが特定され、アクセス可能である  

### アプリケーションは標準外のポートでホストされていますか？
- [ ] いいえ — 標準ポート (80/443) のみが開放されている  
- [ ] はい — 標準外のポートは開放されているが、ウェブアプリケーションはホストされていない  
- [ ] はい — 標準外のポート（例：8080, 8443, 9000）で追加のウェブアプリケーションが特定された  

### 特定の URI パスに個別のアプリケーションがマッピングされていますか？
- [ ] いいえ — 単一のアプリケーションがルートディレクトリ全体を提供している  
- [ ] はい — ディレクトリ列挙 (Directory Enumeration) を通じて複数のアプリケーションが発見された（例：`/api`, `/portal`, `/backup`）  

### サードパーティの偵察サービスによって、過去の、あるいは二次的なアプリケーションが明らかになりましたか？
- [ ] いいえ — `Shodan`、`Censys`、または `Wayback Machine` からの重要な発見はない  
- [ ] はい — 過去のスナップショットやサービススキャンにより、現在アクティブだがリンクされていないアプリケーションが明らかになった  

---