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
- 数据库: data.db (不是resume.db)
- 技术栈: FastAPI + PyMuPDF + python-docx + DeepSeek API + SQLite
- 功能: PDF/Word上传分析、文本分析、流式输出、历史记录、导出PDF、暗色模式、VIP多次改写
- 商业化: 邮箱注册登录+JWT、免费3次/天、VIP套餐(数据库管理，可后台编辑)
- 后台: /admin 密码 admin2026（深色科技感主题，JWT双重认证）
- 邀请页: /invite（两步流程：权益展示→登录获取邀请码）
- 支付: 虎皮椒已集成(payment.py)，待填APPID/APPSECRET；领导考虑YunGouOS(0.38%费率/无需域名)或PayJS(1.4%费率/D+0到账)

### 已完成功能 (截至2026-03-03)
- 前端两栏布局（左评分/右优化简历）、可折叠历史面板
- VIP多次改写（/api/rewrite-stream）
- 续费功能：VIP状态卡显示续费按钮，点击跳转购买页，时长自动叠加
- 邀请机制：6位邀请码、邀请人+3天VIP、被邀请人+1天VIP、20%佣金
- 邀请链接 /invite?code=XXX 自动弹注册弹窗预填邀请码
- VIP套餐数据库管理（vip_plans表，后台CRUD）
- 公告系统（announcements表，后台发布/编辑/隐藏）
- 安全策略：CORS限制、安全响应头、登录限流(5分钟10次)、安全日志
- 管理后台：概览/用户/分析记录/订单/优惠码/邀请/套餐/公告（8个模块）
- 管理后台UI：深色科技感、SVG图标、多条件筛选、增强分页、移动端汉堡菜单

### VIP套餐价格 (2026-03-03调整，面向大学生)
**时长套餐：**
- 周卡：2.9元/7天 (日均0.41元)
- 月卡：9.9元/30天 (日均0.33元)
- 季卡：29.9元/90天 (日均0.33元)
- 年卡：79.9元/365天 (日均0.22元)

**次数包：**
- 5次包：2.9元 (单次0.58元)
- 20次包：9.9元 (单次0.49元)
- 50次包：19.9元 (单次0.4元)

### 数据库表
- users（含invite_code/invited_by/is_banned/commission_balance）
- usage_log、credit_packages、analysis_log、invitations
- vip_plans（套餐管理）、announcements（公告）
- orders、promo_codes（payment.py管理）

### 全量测试结果 (2026-03-02 19:28)
- 23项测试全部通过：页面/注册/登录/VIP/套餐/邀请/公告/分析/导出/安全头/游客/管理后台
- 总用户6人，VIP 2人，分析记录2条

### 待对接
- 支付密钥（虎皮椒或其他方案，领导考虑中）
- 腾讯广点通广告位ID

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
- 游客模式(全部20个): 免登录体验每天3次(IP限流)，用完弹注册引导 (2026-03-03完成)
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
