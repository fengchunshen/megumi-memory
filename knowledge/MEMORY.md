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
- 中转站: https://code.newcli.com/claude/droid

## Obsidian Vault
- 开发文档: /root/obsidian/dev-docs/ → GitHub fengchunshen/dev-docs
- 记忆库: /root/obsidian/megumi-memory/ → GitHub fengchunshen/megumi-memory

## 工作习惯
- 子智能体每完成一个阶段都要主动向领导汇报进度
- 不要等全部做完才说，阶段性汇报
- 产品经理做完方案后，必须先给领导审批，批准后再派给高级开发
- 流程：领导提需求 → 产品经理出方案 → 领导审批 → 高级开发写代码 → Megumi部署测试
- **每次会话结束前，自动将重要内容更新到记忆文件（MEMORY.md + memory/日期.md）**
- PDF 查重工具: /home/lighthouse/pdf-checker/，端口 8080
  - bug: 相同 PDF 查重率显示 0%，高级开发正在修复中

## 简历助手 (resume-optimizer)
- 项目路径: /home/lighthouse/resume-optimizer
- 端口: 8081
- 地址: http://43.167.199.107:8081/
- 技术栈: FastAPI + uvicorn + PyMuPDF + python-docx + DeepSeek API (openai SDK)
- AI模型: deepseek-chat
- 环境变量配置: /etc/profile.d/resume-optimizer.sh
- 状态: 运行中

### 功能清单 (2026-02-27 全面优化完成)
- 上传 PDF 或 Word(.docx) 简历 → AI 五维度评分+建议+优化改写
- 粘贴文本分析
- 多语言支持: 自动识别中英文等语言，用对应语言输出分析
- 流式输出: SSE 打字机效果，实时显示 AI 分析过程
- 历史记录: localStorage 保存最近20条，可回看/删除/清空
- 导出 PDF: 浏览器原生打印导出，中文完美支持
- 暗色模式: light/dark 切换，localStorage 记住偏好

### API 接口
- POST /api/analyze-pdf — 文件上传分析(PDF/Word)
- POST /api/analyze-text — 文本分析
- POST /api/analyze-pdf-stream — 文件上传流式分析
- POST /api/analyze-text-stream — 文本流式分析
- POST /api/register — 邮箱+密码注册
- POST /api/login — 登录，返回JWT token
- GET /api/user/info — 用户信息+今日用量
- GET /api/vip/plans — VIP套餐列表
- POST /api/vip/activate — 模拟开通VIP
- POST /api/pay/create — 创建支付订单（虎皮椒）
- POST /api/pay/notify — 支付异步回调
- GET /api/pay/status — 查询订单状态
- GET /api/admin/dashboard — 后台仪表盘
- GET /api/admin/users — 用户列表
- POST /api/admin/user/vip — 赠送VIP
- POST /api/admin/user/cancel-vip — 取消VIP

### UI/UX
- SVG 环形分数动画 + 数字递增动效
- 各维度进度条动画 + 卡片入场动画
- 三环 loading 动画 + 脉冲指示灯
- 渐变按钮、卡片悬浮微交互
- 移动端适配

### 后端加固
- API Key 走环境变量 (os.environ.get)
- IP 速率限制: 每分钟5次，超限返回429
- AI 调用错误自动重试最多2次，递增等待

### 商业化系统 (2026-02-27 晚)
- 用户系统: 邮箱+密码注册登录，JWT鉴权(72h)，SQLite存储(data.db)
- 免费用户限制: 每天3次分析、1次改写
- VIP套餐: 月卡¥19.9(30天)、季卡¥49.9(90天)、年卡¥99.9(365天)、单次包¥3.9(1天)
- 支付: 虎皮椒聚合支付框架已集成，待配置密钥
- 广告: 广点通SDK已集成，待配置广告位ID，有备用广告兜底
- 后台管理: /admin 页面，密码 admin2026
- 核心文件: auth.py, admin.py, payment.py

### 待对接（领导2月28日提供）
- 腾讯广告联盟: GDT_APP_ID, GDT_PLACEMENT_ID (https://e.qq.com)
- 虎皮椒支付: XUNHU_APPID, XUNHU_APPSECRET (https://www.xunhupay.com)

## PDF查重工具 (pdf-checker)
- 项目路径: /home/lighthouse/pdf-checker
- 端口: 8080
- 技术栈: Python (FastAPI)
- bug: 相同PDF查重率显示0%，高级开发写了多轮测试(test_bug~test_bug7.py)尝试修复
- 状态: 有bug待修复

## 博查搜索 Skill
- Skill路径: /root/.openclaw/workspace/skills/bocha-search/
- API Key: 已配到 /etc/profile.d/bocha.sh
- 用法: node scripts/search.mjs "关键词" -n 数量 --summary
- 用途: 产品经理做竞品调研和市场信息检索

## 服务器环境
- 已安装 Google Chrome (给浏览器自动化用)
- DeepSeek API Key: sk-941...eaef (简历助手在用)
- 博查 API Key: sk-cd6...b8a4
