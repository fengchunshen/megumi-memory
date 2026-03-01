# MEMORY.md - Megumi 长期记忆

## 领导信息
- GitHub: fengchunshen
- 时区: UTC+8
- 风格偏好: 干练、不废话
- 服务器: 腾讯云 43.167.199.107 (OpenCloudOS)

## 智能体架构
- 🎯 产品经理 (pm) — 需求调研拆解，指挥高级开发
- 🔧 高级开发 (senior-dev) — 写代码，可通过 Claude Code CLI 执行
- ⚡ Megumi (main) — 总调度，负责部署测试验收

编排链路：领导 → Megumi → 产品经理 → 高级开发 → Megumi(部署测试)

## Claude Code CLI
- 安装在服务器上，版本 2.1.62
- root 不能用 --dangerously-skip-permissions，需用 lighthouse 用户
- node 实体文件在 /usr/local/bin/node
- Claude Code wrapper: /usr/local/bin/claude-code
- 中转站: https://code.newcli.com/claude (2026-02-28更换)
- 模型: claude-opus-4-6

## Obsidian Vault
- 开发文档: /root/obsidian/dev-docs/ → GitHub fengchunshen/dev-docs
- 记忆库: /root/obsidian/megumi-memory/ → GitHub fengchunshen/megumi-memory

## 工作习惯
- 子智能体每完成一个阶段都要主动向领导汇报进度
- 不要等全部做完才说，阶段性汇报
- 产品经理做完方案后，必须先给领导审批，批准后再派给高级开发
- 流程：领导提需求 → 产品经理出方案 → 领导审批 → 高级开发写代码 → Megumi部署测试
- 每次会话结束前，自动将重要内容更新到记忆文件（MEMORY.md + memory/日期.md）
- 应用体验优先：游客先体验再引导注册，不要一上来就要登录

## DeepSeek API
- 正确Key: sk-94102d4aca93494b886edadc9057eaef (2026-03-01修正)
- 旧Key已失效: sk-941d01e78f1a4a2c953e0e3e5c83eaef
- 环境变量: /etc/profile.d/resume-optimizer.sh
- 模型: deepseek-chat

## 简历助手 (resume-optimizer)
- 路径: /home/lighthouse/resume-optimizer
- 端口: 8081，地址: http://43.167.199.107:8081/
- 技术栈: FastAPI + PyMuPDF + python-docx + DeepSeek API
- 功能: PDF/Word上传分析、文本分析、流式输出、历史记录、导出PDF、暗色模式
- 商业化: 邮箱注册登录+JWT、免费3次/天、VIP套餐(月¥19.9/季¥49.9/年¥99.9/单次¥3.9)
- 后台: /admin 密码 admin2026
- 待对接: 虎皮椒支付密钥、腾讯广点通广告位ID

## 日程管理应用 (schedule-app)
- 路径: /home/lighthouse/schedule-app
- 端口: 8002，地址: http://43.167.199.107:8002/
- 管理端: http://43.167.199.107:8002/admin，密码 admin2026
- 技术栈: FastAPI + React + Vite + TailwindCSS + SQLite
- systemd服务: schedule-app.service (enabled)
- 状态: v3.0已上线，暂停开发 (2026-02-28 23:45)

### 已完成功能
- v1.0: 日程CRUD(日/周/月视图)、重复日程、优先级、分类标签
- v2.0: 商业化(用户系统+VIP+邀请分佣+管理端)、通知中心、个人中心、分页系统
- v3.0: 习惯打卡、子任务、番茄专注钟、积分成就系统、数据统计
- 交互优化: 21项P0-P2全部完成(动效/引导/确认弹窗/批量操作等)
- VIP: 动态套餐管理、续费时长叠加、免费版限制(50日程/3习惯/30天历史)
- 日程便捷: 过去时间校验、智能默认时间、一键完成、过期标记、优先级颜色

### 待对接
- 微信支付: 商户号+域名 (wechat_pay.py已写好)
- 阿里云短信/语音: AccessKey+模板ID (aliyun_notify.py已写好)

## 20个AI小工具应用 (8003-8022)
- 统一架构: FastAPI + SQLite + DeepSeek API + JWT鉴权
- 前端: 8003-8010用React打包，8011-8022用纯HTML/JS(暗色主题，独立配色)
- 功能: 注册登录、AI生成(流式SSE)、历史记录、收藏、VIP套餐
- 游客模式(8011-8022): 免登录体验每天3次(IP限流)，用完弹注册引导
- 部署: systemd托管，开机自启
- DeepSeek API Key: sk-94102d4aca93494b886edadc9057eaef (全部20个已修正)

### 端口清单
| 端口 | 名称 | 描述 |
|------|------|------|
| 8003 | title-genius | AI标题生成器 |
| 8004 | weekly-ai | AI周报生成器 |
| 8005 | contract-guard | AI合同风险检测 |
| 8006 | rednote-ai | 小红书文案生成器 |
| 8007 | speak-buddy | AI英语口语练习 |
| 8008 | easy-ledger | AI记账助手 |
| 8009 | mock-interview | AI模拟面试 |
| 8010 | script-flow | 视频脚本生成器 |
| 8011 | mind-hug | AI心理陪伴助手 |
| 8012 | name-craft | AI起名大师 |
| 8013 | dream-reader | AI解梦师 |
| 8014 | slogan-smith | AI广告语生成器 |
| 8015 | email-pro | AI邮件写手 |
| 8016 | code-explain | AI代码解释器 |
| 8017 | food-match | AI今天吃什么 |
| 8018 | story-weaver | AI故事生成器 |
| 8019 | travel-plan | AI旅行规划师 |
| 8020 | debate-king | AI辩论陪练 |
| 8021 | gift-guru | AI送礼顾问 |
| 8022 | pet-doctor | AI宠物顾问 |

## PDF查重工具 (pdf-checker)
- 路径: /home/lighthouse/pdf-checker
- 端口: 8080
- bug: 相同PDF查重率显示0%，待修复

## 博查搜索 Skill
- Skill路径: /root/.openclaw/workspace/skills/bocha-search/
- API Key: 已配到 /etc/profile.d/bocha.sh (sk-cd6...b8a4)
- 用法: node scripts/search.mjs "关键词" -n 数量 --summary
