# lightpage（轻页）

**让 AI 做出来的网页和文档，变成可以直接发给别人的链接。**

lightpage 是一个免费的网页和文档发布 Skill。你可以让支持 Skill 的 AI 助手制作小游戏、小工具或交互页面，再用轻页发布并分享。发布后的网页可以在浏览器里直接打开，访问者无需安装 Skill。

[轻页官网](https://lightpage.hqdx.store/) · [管理我的作品](https://lightpage.hqdx.store/manage/) · [Skill 使用指南](SKILL.md)

## 怎么开始

把下面这段话发给支持 Skill 安装的 AI 助手：

> 请从 https://github.com/zhuchaokn/lightpage 安装 lightpage Skill 到你实际使用的 skills 目录，核对完整包包含 SKILL.md、references 和 scripts，然后告诉我是否需要重新加载 Skill。我想用它发布和分享网页。

安装方式取决于你使用的 AI 工具；工具需要支持加载 Skill 和执行本地 Python 脚本。下载项目后，也可以将整个 `lightpage` 文件夹放到该工具的 skills 目录，按工具的说明重新加载。

使用支持的 Skills CLI 时，也可以运行：

```bash
npx skills add zhuchaokn/lightpage --skill lightpage
```

目前脚本支持 macOS、Linux 的 Python 3.6–3.12；Windows 可在 WSL 中使用。日常发布无需安装 uv、pip 或第三方 Python 依赖。

## 可以这样和 AI 说

- “帮我做一个接水果小游戏，用 lightpage 发布，给我一个能发到微信里的链接。”
- “把这个 HTML 页面发布出去，标题叫旅行计划。”
- “把这份 Markdown 文档发成分享链接。”
- “更新我之前发布的这个作品，保留原来的链接。”
- “给这个页面设置访问密码，分别告诉我链接和密码。”
- “把这个页面设置为需要审批，收到申请后先问我，再批准指定人。”
- “看看我这个作品最近 7 天的访问情况。”

首次发布无需提前扫码登录。发布后，AI 会提供作品链接和只给你的微信认领入口；你可以自行确认认领，之后在官网管理作品。认领入口不能分享给访客。新作品默认仅链接访问，不出现在首页；需要公开展示或密码保护时，可在作品管理中调整。

标准 UI／SDK 不满足需求时，Skill 可按需读取当前实例提供的 SSO 登录、评论和访问授权 Markdown API 文档，直接调用接口自定义交互。只读取本次需要的能力，保留用户确认与权限边界。

## 能做什么

- 发布 HTML、Markdown、TXT 或静态网站目录，生成分享链接。
- 按需读取作品原文，支持分片续读。
- 更新已有作品并保留链接，管理分享方式、密码和下架。
- 查看访问申请，按用户许可批准、拒绝、主动授权或撤销。审批方式要求平台实例配置独立作品域名；未配置时无法启用。
- 查看自己的作品和访问统计。
- 按需生成作品分享二维码。
- 按需为页面接入微信登录和位置评论。

平台免费，作品不设置到期时间；上传大小、文件数和平台资源有上限，具体以当前服务返回为准。平台托管静态产物，构建工程需先在本地构建。页面交互与外部服务能否正常运行，需要在发布前检查。

## 手动使用

下列命令从本仓库根目录运行，文件路径替换为你自己的作品。先本地检查，再发布：

```bash
python3 scripts/litepage.py publish /你的作品/index.html --check
python3 scripts/litepage_preview.py /你的作品/index.html
python3 scripts/litepage.py publish /你的作品/index.html --title "我的作品"
```

预览命令启动本机预览服务，按输出的 `preview_url` 打开并检查页面。完整操作、账号恢复及失败重试见 [SKILL.md](SKILL.md) 和 [常见问答](references/faq.md)。

## 版本与项目范围

当前 Skill 版本：**0.10.1**。

本仓库发布 lightpage Skill、参考文档和运行必需的客户端脚本。后端服务、官网工程、业务数据库和部署配置由轻页平台维护。请通过 [Issues](https://github.com/zhuchaokn/lightpage/issues) 提交使用问题，不要上传账号凭证或私有认领入口。

## 开源许可

本仓库采用 [MIT-0](LICENSE) 许可证，第三方二维码实现继续遵循其原有 MIT 许可，详见 [第三方声明](THIRD_PARTY_NOTICES.md)。

## English

lightpage is an Agent Skill for publishing and sharing HTML, Markdown, TXT and static websites through the free LightPage service. Install this repository as a skill, ask your agent to create or publish a page, and share the resulting URL. It includes the skill instructions, references, and the Python client required to run it.

Requires an agent with local Python execution. Supports Python 3.6–3.12 on macOS/Linux, or Windows through WSL. Routine publishing uses the Python standard library; optional screenshot processing may require additional tools. See [SKILL.md](SKILL.md) for the complete workflow.
