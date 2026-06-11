## WSTG-ATHN-08 — 测试弱安全问题答案 (Testing for Weak Security Question Answer)

测试弱安全问题答案涉及评估用于验证用户身份的基于知识的身份验证 (Knowledge-based authentication, KBA) 机制，通常在密码恢复或多因素身份验证 (Multi-factor authentication, MFA) 流程中使用。这一点至关重要，因为安全问题通常依赖于静态、非机密的信息，这些信息很容易通过开源情报 (Open-source intelligence, OSINT) 或社会工程学 (Social engineering) 获取，从而导致未经授权的账户接管 (Account takeover)。这些漏洞通常出现在“忘记密码”或“账户解锁”模块中，用户在这些模块中提供预设问题的答案。从攻击者的角度来看，这是自动化暴力破解 (Brute-forcing) 或针对性研究的主要目标，因为许多用户对常见问题（如“您母亲的婚前姓氏是什么？”或“您就读的高中是哪所？”）提供的答案是可预测的或可公开验证的。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-ATHN-08 |
| **CWE** | CWE-640 |
| **测试状态** | 未执行 |
| **严重性** | 中 / 高* |

> *如果安全问题是在没有进一步验证的情况下进行完整密码重置和账户接管的唯一要求，则严重性变为“高”。

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/08-Testing_for_Weak_Security_Question_Answer  
* https://hacktricks.wiki/en/pentesting-web/login-bypass/index.html  

**工具：** `Burp Suite (Intruder)`, `ffuf`, `Maltego`, `Sherlock`, `Social Engineering Toolkit (SET)`

### 应用程序是否实施了用于身份验证或密码恢复的安全问题？
- [ ] 否 — 安全问题**未被用于**身份验证或恢复  
- [ ] 是 — 安全问题作为可选的二次因子**被使用**  
- [ ] 是 — 安全问题被作为主要/唯一的恢复机制**使用** *(高风险)*  

### 安全问题是从固定列表中选择的还是用户自定义的？
- [ ] 否 — 问题完全由用户自定义并要求高熵  
- [ ] 是 — 问题是固定的，但包括非 OSINT 可发现的选项  
- [ ] 是 — 问题来自常见的、OSINT 可发现的主题固定列表 *(存在漏洞)*  

### 是否对安全问题答案尝试应用了速率限制或账户锁定？
- [ ] 是 — **已应用**严格的速率限制 (Rate Limiting) 和账户锁定 (Account Lockout) 以防止暴力破解  
- [ ] 是 — **已应用**速率限制，但**可能**通过 IP 轮换或标头操纵绕过  
- [ ] 否 — 答案尝试**未实施速率限制**  

### 应用程序是否对答案内容强制执行复杂度或校验？
- [ ] 是 — 校验机制可防止常用词汇并确保答案的复杂度/唯一性  
- [ ] 是 — 存在基础校验，但**可以**通过通用词表或字典攻击 (Dictionary attacks) 绕过  
- [ ] 否 — **未进行校验**；**允许**简单或单字符答案  

### 是否可以通过逻辑缺陷绕过安全问题挑战？
- [ ] 否 — 恢复流程严格要求有效的答案才能继续  
- [ ] 是 — 恢复流程**可以**通过操纵 HTTP 响应代码或会话参数绕过  
- [ ] 是 — 答案在页面的源代码或隐藏字段中反映  

---