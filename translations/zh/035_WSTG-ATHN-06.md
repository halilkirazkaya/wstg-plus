## WSTG-ATHN-06 — 浏览器缓存弱点测试 (Testing for Browser Cache Weakness)

浏览器缓存弱点（Browser cache weakness）发生在敏感信息被存储在本地浏览器缓存中，且可被能够访问同一台物理机器的未经授权用户检索时。由于缺乏适当的缓存控制响应头 (Cache control headers)，导致潜在的敏感数据（如个人信息、账户详情或会话标识符）在用户登出或关闭会话后仍然存在。攻击者或共享/公共终端的后续用户可以通过浏览浏览器历史记录、使用“后退”按钮或检查本地缓存文件来窃取 (Exfiltrate) 缓存的响应。确保实施严格的指令，如 `Cache-Control: no-store` 和 `Pragma: no-cache`，对于任何处理经过身份验证的内容的应用程序至关重要，以确保敏感数据绝不会被写入磁盘。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-06 |
| **CWE** | CWE-525 |
| **测试状态** | 未执行 |
| **严重程度** | 低 / 中* |

> *如果应用程序经常从共享/公共自助服务机 (Kiosks) 访问，或者包含高度敏感的个人可识别信息 (PII) / 受保护健康信息 (PHI) 数据，则严重程度变为“中”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/06-Testing_for_Browser_Cache_Weaknesses  

**工具：** `Burp Suite (Proxy/Repeater)`, `Browser Developer Tools (Network Tab)`, `Zed Attack Proxy (ZAP)`

### 在经过身份验证或敏感的页面上是否存在适当的缓存控制响应头？
- [ ] 是 — `Cache-Control: no-store, no-cache` 和 `Pragma: no-cache` **存在**且**配置正确**  
- [ ] 是 — 存在部分响应头，但**缺失** `no-store`，允许磁盘缓存  
- [ ] 否 — 未**应用**任何缓存相关的响应头，依赖浏览器默认行为  

### 应用程序是否对用户特定数据使用 `private` 指令？
- [ ] 是 — 为所有个性化内容**启用**了 `Cache-Control: private`，以防止代理缓存 (Proxy Caching)  
- [ ] 否 — 内容被标记为 `public` 或缺少 `private` 指令，使其可被中间代理**缓存**  

### 成功登出后，是否可以通过浏览器“后退”按钮访问敏感信息？
- [ ] 否 — 浏览器提示重新身份验证，或者页面**未从**本地缓存加载  
- [ ] 是 — 在会话终止后，敏感信息通过后退按钮仍然**可见**  

### 敏感的非 HTML 文件（例如 PDF、CSV 导出、JSON API 响应）是否受到缓存保护？
- [ ] 否 — 应用程序不生成或处理敏感的非 HTML 文件  
- [ ] 是 — 对所有敏感文件下载和 API 终端节点均**应用**了严格的缓存控制响应头  
- [ ] 否 — 敏感下载或 API 响应被**存储**在本地浏览器缓存或临时目录中  

### 应用程序是否使用 `Expires: 0` 或过去的时间日期来防止使用过期数据？
- [ ] 是 — `Expires` 响应头被设置为 `0` 或历史日期，**确保**浏览器将内容视为立即过期  
- [ ] 否 — 敏感页面的 `Expires` 响应头**缺失**或被设置为未来日期  

---