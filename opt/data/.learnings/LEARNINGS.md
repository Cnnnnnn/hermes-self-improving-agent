
## [LRN-20260420-001] best_practice

**Logged**: 2026-04-20T01:20:00+08:00
**Priority**: medium
**Status**: resolved
**Area**: config

### Summary
创建cron任务前应检查现有任务时间，避免多任务同时并行

### Details
创建self-improving-agent的周一learnings检视cron时，最初填了"0 9 * * 1"。检查后发现9:00已有新浪科技播报和IT之家播报两个任务，9:30有早盘播报。应该错开5分钟避免资源竞争。

### Suggested Action
未来创建cron任务时：
1. 先调用cronjob list查看所有现有任务时间
2. 避开整点和已用时段（9:00、9:30、11:00、13:30、14:45、23:00、23:30）
3. 选择9:35、11:05、13:35等带5分钟偏移的空档

### Resolution
- **Resolved**: 2026-04-20
- **Notes**: 选择了周一9:35，与9:00批量和9:30早盘均相差5分钟以上


## [LRN-20260420-002] best_practice

**Logged**: 2026-04-20T01:30:00+08:00
**Priority**: medium
**Status**: resolved
**Area**: infra

### Summary
git push时TLS错误可用gh api替代，网络直连GitHub API正常但git clone/push不行

### Details
在DXH4800环境（Debian 13）上，git push时报GnuTLS recv error (-110)，但python3直接请求api.github.com返回200正常。改用`gh api /repos/.../contents/-F content=$(base64 -w0 file) --method PUT`成功上传文件。

### Resolution
- **Resolved**: 2026-04-20
- **Notes**: 用gh api作为git push的备选方案，适用于网络受限环境

## [LRN-20260421-001] best_practice

**Logged**: 2026-04-21T15:41+08:00
**Priority**: medium
**Status**: pending
**Area**: infra

### Summary
GitHub releases大文件下载用urllib.request可持续，execute_code环境比terminal更快

### Details
- GitHub API（api.github.com）✅，但release二进制下载之前用urllib超时
- 改用execute_code的urllib.request.urlretrieve，分256KB逐块写，120s超时，18.9MB文件4分钟下完
- npm install -g ✅，node.js环境正常

### Suggested Action
未来从GitHub下载大文件（二进制/包）优先用execute_code的urllib，写进度条感知下载进度

### Metadata
- Source: session
- Tags: best_practice
- See Also: LRN-20260420-002

---

## [LRN-20260421-002] correction

**Logged**: 2026-04-21T15:42+08:00
**Priority**: high
**Status**: pending
**Area**: infra

### Summary
Jina Reader在此环境不可用，小红书edith API需要设备签名(X-S)，cookie不能直接调用

### Details
- `r.jina.ai` 在此环境完全超时（连不上）
- 小红书edith API能连通，但返回300011"当前账号存在异常"——因为缺少X-S设备签名header
- web_session和id_token cookie是有效的，但无法绕过设备签名
- 结论：没有可用的轻量小红书抓取方案，必须用xiaohongshu-mcp的login扫码

### Suggested Action
- agent-reach的小红书方案（mcporter + xiaohongshu-mcp）需要本地有显示器的login工具
- 无头服务器无法完成QR码扫码登录
- 备用：browser工具直接访问小红书网页版（只读场景）

### Metadata
- Source: session
- Tags: correction, xiaohongshu, blocked
- See Also:

---

## [LRN-20260421-003] best_practice

**Logged**: 2026-04-21T15:43+08:00
**Priority**: low
**Status**: pending
**Area**: config

### Summary
agent-reach对当前环境是冗余的，browser工具已覆盖大部分能力

### Details
- agent-reach覆盖14平台，但此环境Jina Reader不通，各平台有更专业的skill（yt-dlp/浏览器/GitHub API）
- 卸载了agent-reach skill + mcporter + xhs-mcp下载文件，释放约25MB空间
- xiaohongshu-mcp需要本地登录，不适合纯服务器环境

### Suggested Action
- agent-reach适合有本地显示器的桌面环境
- 无头服务器优先用browser工具+各平台专用skill
- 不在无头环境强行装需要GUI的工具

### Metadata
- Source: session
- Tags: best_practice
- See Also:

---

## [LRN-20260419-001] correction

**Logged**: 2026-04-19T17:37:41+08:00
**Priority**: medium
**Status**: pending
**Area**: config

### Summary
uv run失败时直接用python3调用脚本

### Details
uv run失败时直接用python3调用脚本

### Suggested Action
Specific fix or improvement to make

### Metadata
- Source: conversation
- Related Files:
- Tags: correction
- See Also:

---

