---
name: media-resource-automation
description: 影视资源自动化——资源站搜片/提取磁力→网盘离线缓存→在线观看。触发：电影资源、磁力链接、115离线。
---

# 影视资源自动化（Media Resource Automation）

帮用户从电影资源站自动找片→提取下载链接→（可选）离线缓存到网盘→在线观看的完整链路。

## 触发条件
- 用户要求"缓存/下载/找某部电影、剧集、动漫"
- 提到电影资源网站、磁力链接、115网盘离线、云盘缓存
- 提到资源站域名变化（动态域名）

## 链路总览
```
用户发消息"想看XX" → ①登录资源站搜索 → ②提取磁力/网盘链接 → ③离线缓存到网盘 → ④在线播放
```

## 资源站操作（已验证 2026-08-13）

### 站点信息
- 主入口：`https://www.xn--ykq321c.com/?count=1`（实时更新可用域名）
- 当前可用：`https://www.xn--wcv59z.com/`（域名会变，用主入口找最新）
- 账号：PHONE / 密码存本地文件（**禁止明文进对话**，参考 api-key-masking-workaround）
- 登录后账号显示"666668855555"

### 操作步骤
1. **访问**：`browser_navigate` 到当前可用域名 → 可能先遇"浏览器安全验证"（JS挑战，等待自动计算）→ 再遇"登录后访问受限内容" → 点"立即登录"
2. **登录**：填入用户名+密码 → 点登录 → 若弹"最新网址"提示可点"14天内不再提醒"
3. **搜索**：顶部 Search 框输入片名 → Enter → 搜索结果页有 全部/电影/剧集/动漫/种子/网盘 分类标签 + 模糊/适中/精准匹配模式
4. **详情页**：点结果链接（URL 形如 `/mv/<id>`）→ 详情页含导演/主演/类型/简介/评分
5. **提取下载链接**：资源列表**异步加载**，等 2-3 秒后用 JS 查询：
   ```js
   Array.from(document.querySelectorAll('a')).filter(a => a.href && (a.href.includes('magnet') || a.href.includes('ed2k') || a.href.includes('pan.') || a.href.includes('115') || a.href.includes('aliyun') || a.href.includes('quark')))
   ```
   返回 93+ 条：磁力（magnet:）、离线跳转（keepshare.org 中转）、网盘链接。磁力名含清晰度标识（1080p/2160p/4K/60帧率）。

### 资源站避坑
- 详情页点结果链接后浏览器可能**不跳转**（Vue 单页应用）——直接 `browser_navigate` 到 href 提取的 `/mv/<id>` URL
- 下载链接列表异步加载，需等待后再查询 DOM
- 域名动态变化：每次先访问主入口拿最新可用域名

## 115 网盘离线缓存（⚠️ 高风控，谨慎）

### 技术方案
- **115driver MCP Server**（GitHub SheltonZhu/115driver，Go）：提供 `addOfflineTaskURIs`（磁力/HTTP/ED2K 离线任务）、`listOfflineTasks`、`search` 等工具，可注册为 Hermes MCP 工具
- 配置：`mcp --cookie="UID=xxx;CID=xxx;SEID=xxx;KID=xxx" --allow-destructive-tools`（cookie 有效期约7天，需从浏览器/App 抓包获取）
- cookie 获取：网页 F12→Application→Cookies 复制 UID/CID/SEID/KID；或 AList/115driver 扫码登录

### ⚠️ 风控红线（2026-08-13 评估，重要）
- **115 正在专项整治期**：2026-06-01 起《违规专项治理公告》（配合"剑网2026"，整治期 6月-11月）
- 三条红线：①内容违规（存储未授权海外影视）②**技术违规（第三方工具/脚本/非官方API批量操作）**③商业违规（出租合买账号）
- 处罚：功能限制→封号→永久注销（阶梯式）；检测手段=哈希比对+AI识别+人工审核，重命名/压缩/子文件夹藏文件仍可被识别
- **账号共用风险**：用户的115账号与哥哥共用（两个IP）——封号损失不止一人，风险收益比差
- **结论（用户已接受）**：整治期内不做自动化（C方案），用 B 方案：Hermes 只负责搜索+给磁力，用户手动粘到115官方离线页面

### 其他可用工具（参考）
- AList 115 驱动（cookie 约7天有效、QRCodeToken 登录、API 限流建议 2次/秒）
- CloudDrive2（挂载115→NAS本地盘→极影视/Emby播放）
- 115proxy-for-kodi / 115 官方播放器（在线播放）

## 隐私与合规
- 账号密码、cookie 一律存本地文件，不内联进命令/对话（见 api-key-masking-workaround）
- 磁力来源版权由用户自行判断，工具本身中立
- 用户强调"缓存"≠"下载"：115 离线任务在云端执行，不占本地硬盘

## 相关 Skill
- `api-key-masking-workaround`：敏感信息文件传递
- `ev-news-bot-ops`：Docker 新闻机器人（不同领域但同属推送自动化）
