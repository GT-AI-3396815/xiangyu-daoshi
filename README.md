# 香愈导师 — 多特瑞芳疗师智能工作台

从 Kimi 平台（https://uernvnl7seose.ok.kimi.link/）完整迁移至 WorkBuddy 的站点。
这是一个 React + Vite 构建的单页应用（SPA），面向多特瑞女性健康倡导者的「芳香事业数字合伙人」。

## 项目结构

```
doterra-site/
├── index.html              # 入口页（已移除 Kimi 平台 SDK 脚本）
├── assets/
│   ├── index-DCx79VpA.js   # 主 JS 包（576 KB，React 应用）
│   └── index-DkP6Q7Iw.css  # 样式表（94 KB）
└── images/                 # 6 张页面图片资源
```

## 原始源码模块（构建包内含 code-path 调试标记）

- `src/App.tsx` / `src/main.tsx`
- `src/components/`：AIChat.tsx、Header.tsx、Modal.tsx、UsageTracker.tsx
- `src/pages/`：Home.tsx
- `src/sections/`：AromaDailySign（芳香日签）、PainPointTheater（痛点剧场）、HotspotHelper（热点借势）、BrandStory（品牌故事）、SmartDiagnosis（智能问诊）、ProductScript（产品话术）、ObjectionHandler（异议处理）、ClientManager（客户档案）、TeamCenter（团队中心）、VisionAISection 等

## 在线部署（GitHub Pages）

- 仓库：https://github.com/GT-AI-3396815/xiangyu-daoshi
- 线上地址：https://gt-ai-3396815.github.io/xiangyu-daoshi/

## 2026-09-11 内容与安全升级

- **芳疗师工具箱**（悬浮入口进入）新增：精油专业档案（12 款主力油拉丁学名/化学成分/安全要点）、安全速查（光敏完整清单/孕哺期禁忌/儿童年龄分级/宠物禁忌/癫痫高血压/口服合规口径）、合规话术转换器 + 标准免责声明库、事业宝典（会员制度/套组框架/晋级架构/价格管理器）、客户教育素材库（CPTG/单方vs复方/椰子油等 6 篇可转发）、节气用油速查、滚珠瓶换算、AI 情绪处方。
- **去虚构人设**：AI 不再自称"深耕 8 年"；系统提示注入合规底线（不宣称疗效、提示就医、以官方资料为准）。
- **SEO**：完整 meta description/keywords/OG 标签。
- **AI 服务端架构（doterra-app）**：完全无状态——token 为 HMAC 签名（Authorization / X-Auth-Token / ?token= 三通道），用户数据存客户端 localStorage（含导出/导入迁移），AI 代理保留：Key 服务端持有、模型强制、额度限制、合规过滤、失败退还额度。
- ⚠️ DeepSeek 账户余额不足（402）时 AI 功能返回友好提示，充值后自动恢复。

### 子路径白屏修复（重要）

原包使用 React Router 声明式路由 `<Route path="/">`，部署到 GitHub Pages 子路径
`/xiangyu-daoshi/` 时报 `No routes matched location "/xiangyu-daoshi/"` 导致整页白屏。
已在 JS 包中将路由改为通配 `path:"/*"`（单页应用任意路径均可匹配），修复后线上验证通过。

## 本地运行

```bash
cd doterra-site
python -m http.server 8945 --bind 127.0.0.1
# 浏览器打开 http://127.0.0.1:8945
```

注意：必须通过 HTTP 服务器访问，直接双击 index.html（file:// 协议）会因 ES Module 跨域限制而白屏。

## 迁移检查与测试结果（2026-09-09）

| 测试项 | 结果 |
|---|---|
| 页面加载 / React 挂载 | ✅ 通过（3/3 次导航稳定，控制台 0 报错） |
| 6 张图片加载 | ✅ 全部 OK |
| 注册弹窗（手机号+密码+昵称表单） | ✅ 正常 |
| 本地日签生成（选主题→生成配方文案） | ✅ 正常 |
| AI 深度生成（DeepSeek 接口） | ✅ 正常，额度计数 1/100 正确累加 |
| 每日额度限制（localStorage 按天重置） | ✅ 逻辑正常 |

### 已修复
- 移除 Kimi 平台专属脚本 `https://www.kimi.com/sdk-seed.js`（迁移到新环境后无用且可能报错）。
- **API Key 已不再暴露（2026-09-09）**：前端 JS 包中的明文 Key 已彻底移除，AI 请求改为走服务端代理。模型已切换为多模态视觉理解模型 `deepseek-v4-flash-vision-exp`。
- 代理后端（持有 Key，仅服务端）：见 `../doterra-app/`（Node 零依赖，`POST /api/chat` 转发 DeepSeek），已发布到 https://db478171155645199b6c383ae53a0506.app.workbuddy.link
- ⚠️ 历史遗留：旧 Key `sk-4785...` 曾公开暴露过（含 git 历史），建议去 DeepSeek 后台作废。

### 备注
- 代理接口当前为公开调用（无鉴权），配合前端每日 100 次的额度限制使用；如额度消耗异常可在代理层加访问限制。
2. 字体依赖第三方镜像 CDN（fonts.loli.net / gstatic.loli.net），CDN 不可用时字体会回退，页面仍可用。
3. 测试中曾出现"白屏"现象，经排查为浏览器自动化工具在 reload 后丢失标签页的误报，与站点代码无关（正常导航 3/3 稳定）。
