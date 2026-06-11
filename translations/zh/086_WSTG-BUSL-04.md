## WSTG-BUSL-04 — 流程耗时测试 (Test for Process Timing)

流程耗时攻击 (Process Timing Attacks) 涉及测量应用程序处理特定请求所需的时间，从而推断有关内部状态或数据是否存在等敏感信息。通过分析响应时间中毫秒级的差异（通常由提前退出条件逻辑、数据库查询或非恒定时间加密比较引起），攻击者可以进行侧信道分析 (Side-channel Analysis)，以枚举有效用户名、验证密码片段或确认特定记录的存在。这些漏洞通常出现在身份验证端点 (Authentication Endpoints)、密码重置表单和基于输入有效性进行后端逻辑分支的资源密集型 API 过滤器中。从攻击者的角度来看，即使应用程序向用户返回完全相同的通用错误消息，这些耗时差异也可以充当“布尔预言机 (Boolean Oracle)”，揭示后端数据的真实情况。


| 字段 | 值 |
|---|---|
| **WSTG ID** | WSTG-BUSL-04 |
| **CWE** | CWE-208 |
| **测试状态** | 未执行 |
| **严重程度** | 中 (Medium) |

**参考资料：**
* https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/10-Business_Logic_Testing/04-Test_for_Process_Timing  
* https://hacktricks.wiki/en/pentesting-web/timing-attacks.html  
* https://portswigger.net/web-security/race-conditions  

**工具：** `Burp Suite (Timing Advisor / Logger++)`, `ffuf`, `curl`, `Python (time module)`, `THC-Hydra`

### 应用程序对于有效与无效的资源请求是否表现出不同的响应时间？
- [ ] 否 — 无论输入是否有效，响应时间均完全一致  
- [ ] 是 — 存在可忽略的耗时差异，但在统计学上**不**显著  
- [ ] 是 — 存在可**观察**到的持续耗时差异，并提供了一个可靠的预言机  

### 是否实现了恒定时间比较函数或人工延迟以规范化响应时间？
- [ ] 是 — 对所有敏感比较操作均**应用**了恒定时间 (Constant-time) 算法 *(最安全)*  
- [ ] 是 — **启用**了人工延迟或抖动 (Jitter)，但底层逻辑仍为非恒定时间  
- [ ] 否 — 未**应用**任何耗时缓解措施，允许直接测量逻辑分支  

### 攻击者能否使用统计分析从网络噪音中分离出耗时信号？
- [ ] 否 — 网络抖动 (Network Jitter) 和应用程序噪音导致耗时分析**无法实现**  
- [ ] 是 — 在样本量足够的情况下，可以从噪音中**分离**出耗时信号  
- [ ] 是 — 耗时差值 (Timing Delta) 足够大，**不**需要进行统计规范化  

### 是否可以通过登录或密码重置功能进行账号枚举 (Account Enumeration)？
- [ ] 否 — 无法通过耗时确定账户是否存在  
- [ ] 是 — 耗时差异允许远程攻击者**枚举**有效的用户名或电子邮件  

### 资源密集型操作（如文件处理、复杂查询）是否通过耗时泄露数据是否存在？
- [ ] 否 — 无论是否找到记录，资源消耗均保持一致  
- [ ] 是 — 通过测量后端处理时长，**可以**推断出敏感记录的存在  

---