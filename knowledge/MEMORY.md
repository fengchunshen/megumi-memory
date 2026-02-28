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

## 日程管理应用 (schedule-app)
- 项目路径: /home/lighthouse/schedule-app
- 端口: 8002
- 地址: http://43.167.199.107:8002/
- 管理端: http://43.167.199.107:8002/admin
- 技术栈: FastAPI + React + Vite + TailwindCSS + SQLite
- 状态: v3.0 已上线 + 商业化完善中
- systemd服务: schedule-app.service (enabled)
- 管理员密码: admin2026

### 用户端功能
- 日程CRUD: 日/周/月视图，重复日程，优先级，分类标签
- 日程搜索: 关键词搜标题/描述
- 通知中心: 已读/未读，全部已读
- 个人中心: 资料编辑、手机绑定、密码修改、账户状态
- VIP付费: 月卡¥9.9/季卡¥24.9/年卡¥79.9
- 邀请分佣: 邀请码+7天VIP奖励+20%佣金+满10元提现
- 公告横幅: 展示管理端发布的公告
- Toast全局提示 + 空状态引导
- 新用户引导: 4步引导流程(Guide组件)
- VIP转化引导: 接近限额自动弹浮窗(VipBanner组件)
- 多端兼容响应式，移动端底部导航

### 管理端功能
- 独立密码登录(admin2026)
- 仪表盘: 用户/VIP/日程/订单/收入 + 今日新增/本周新增/封禁数/活跃套餐(9项数据)
- 数据趋势: 近7天注册+收入柱状图
- 用户管理: 搜索/筛选/赠VIP(可选天数)/封禁/解封/邀请明细/批量操作(勾选+批量封禁+批量赠VIP)
- 订单明细: 全部订单列表+汇总
- 公告管理: 发布/删除公告(带确认弹窗)
- 操作日志: 管理员操作记录
- 套餐管理: 💎套餐tab，增删改/上下架/推荐标签/权益编辑

### 分页系统
- 后端: paginate()辅助函数，每页20条，返回{items,total,page,pages}
- 8个API已支持分页: schedules, search, notifications, admin/users, admin/users/search, admin/orders, admin/announcements, admin/logs
- 前端: Pager通用组件，用户端和管理端都已适配

### VIP权益+套餐管理系统 (2026-02-28 完成)
- 后端: VipPackage表(动态套餐)，替换硬编码VIP_PLANS
- 免费用户限制: 50条日程/5个分类/3个重复日程/无短信电话
- 管理端: 新增"💎套餐"tab，支持增删改/上下架/推荐标签/权益编辑
- 用户端VIP页: 权益对比表+用量仪表盘+动态套餐卡片
- API: /admin/packages CRUD + /vip/limits + /user/quota

### 交互体验优化 P0+P1+P2 (2026-02-28 全部完成)
- P0: 全局动效、按钮反馈、移动端底部导航、表单验证、加载状态、确认弹窗
- P1: 日历优化("今天"按钮+多日程点数)、个人中心loading、邀请页动效、登录页动效、管理端确认弹窗、批量操作、新用户引导(Guide)、视觉规范统一
- P2: VIP转化引导浮窗(VipBanner)、管理端仪表盘增强(9项数据+彩色卡片)
- 新增组件: Guide.jsx, VipBanner.jsx, Confirm.jsx, Loading.jsx

### 主题功能已移除 (2026-02-28)
- 领导要求去掉主题切换
- 只保留light(默认)+dark(管理端)
- 删除了ocean/forest/sunset主题、Sidebar主题切换器、/api/auth/theme API
- User表theme列保留不动(避免迁移)

### v3.0 Phase 1 (2026-02-28 完成)
- 习惯打卡: 创建习惯+每日打卡+连续天数统计+日历热力图
- 子任务/清单: 日程下挂子任务，可拖拽排序
- 番茄专注钟: 25分钟专注+5分钟休息，统计专注时长
- 积分系统: 打卡+5分、完成子任务+2分、完成专注+10分，每100分升1级
- 成就系统: 6个预设成就(初来乍到/日程达人/习惯养成/自律之星/专注新手/专注大师)
- 数据统计: StatsPage概览页
- 免费限制: 最多3个习惯、每日程最多5个子任务
- 新增页面: HabitsPage.jsx, FocusPage.jsx, StatsPage.jsx
- 侧边栏新增: ✅打卡、🍅专注、📊统计

### v3.0 产品规划
- 全网需求调研: 搜索知乎/豌豆荚/App Store/小红书/CSDN，归纳20项需求
- 方案文档: /root/.openclaw/workspace/schedule-app-v3-upgrade-plan.md
- Phase 2(待开发): 四象限视图、成就系统扩展
- Phase 3(待开发): 数据统计复盘、数据导出PDF/Excel

### VIP明细+通知明细 (2026-02-28 完成)
- VipDetailPage: 4个tab(总览/订单记录/短信明细/电话明细)
- 总览: VIP状态卡片+到期提醒+额度进度条+消费统计
- 个人中心VIP卡片: 渐变背景置顶+续费按钮+额度进度条+明细入口
- API: /vip/detail, /vip/orders, /reminder/sms, /reminder/call, /reminder/stats

### VIP续费逻辑 (2026-02-28 完成)
- 规则: 时长叠加+额度叠加（简单透明）
- 未到期续费在到期时间上叠加，已到期从当前开始算
- 额度直接累加，不清零不重置
- 续费预览API: GET /api/vip/renew-preview
- 前端: 点击开通/续费先弹预览弹窗，确认后支付

### 习惯打卡免费版限制 (2026-02-28 完成)
- 数量: 最多3个习惯
- 历史: 免费用户只看近30天，VIP看全部
- 统计页显示限制提示引导升级

### 日程板块便捷性优化 (2026-02-28 完成)
- 过去时间不能创建日程(前后端双重校验)
- 智能默认时间: 新建日程自动填充下一个整点
- 一键完成日程: 圆形按钮+完成后灰显删除线
- 过期日程: 降低透明度+红色"已过期"标签
- 优先级颜色: 左边框(高红/中黄/低绿)
- Schedule新增: is_completed, completed_at字段
- API: PUT /api/schedules/{id}/toggle

### 微信支付模块 (2026-02-28 代码就绪)
- 模块: backend/wechat_pay.py
- 功能: Native扫码支付下单、回调验签、订单查询
- 配置: WECHAT_APPID, WECHAT_MCH_ID, WECHAT_API_KEY, WECHAT_NOTIFY_URL
- 状态: 等领导提供商户号+域名

### 阿里云短信+语音通知 (2026-02-28 代码就绪)
- 模块: backend/aliyun_notify.py
- 功能: send_sms + make_call(TTS外呼播放2次)
- 后台调度: daemon线程每60秒扫描自动发送
- 配置(等领导提供): ALIYUN_ACCESS_KEY_ID/SECRET, SMS_SIGN_NAME, SMS_TEMPLATE_CODE, TTS_CODE

### 待对接
- 微信支付: 等领导配置域名和商户号
- 阿里云通知: 等领导提供AccessKey和模板ID

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
