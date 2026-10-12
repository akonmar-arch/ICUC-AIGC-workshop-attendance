# ICUC AIGC 课堂签到 · GitHub Pages 前端

复用现有教师登录、16 周签到管理、二维码、实时统计、Excel 导出和重置确认 UI。原 Sites 服务继续保存班级名单、签到配置和记录；学生使用各课次的飞书原生表单签到。

本静态入口位于 `github-pages/`，独立于 Sites 的 `app/` 路由。

## 构建

在 `icuc-attendance` 目录执行：

```powershell
& 'C:/Program Files/nodejs/node.exe' node_modules/vite/bin/vite.js build --config vite.pages.config.ts
```

静态产物写入 `pages-dist/`。发布该目录中的全部文件（包括 `.nojekyll`），目标仓库名称为 `ICUC-AIGC-workshop-attendance`，访问路径为 `/ICUC-AIGC-workshop-attendance/`。同时生成 `admin/index.html`、`admin.html` 和 `404.html`，兼容教师后台入口与刷新。

## 登录与连接

- API 固定为 `https://icuc-aigc-attendance.akon-mar.chatgpt.site`，由后端允许该 GitHub Pages 来源的 CORS。
- 使用原教师密码登录。静态页面不提供密码初始化或修改。
- 教师会话令牌仅保存在当前网页 JavaScript 内存，不写入 cookie、localStorage、sessionStorage、URL 或静态文件。
- 页面刷新或关闭、令牌到期、API 返回 401/403 时，后台卸载并停止自动轮询，重新显示密码登录。
- 跨站 API 使用 `Authorization: Bearer` 和 `credentials: omit`；原 Sites 同站 UI 继续使用既有 cookie 登录。
- 静态产物没有学生姓名、学号、应用密码、飞书密钥或教师令牌；登录后才从认证 API 读取实际数据。

部署前须先上线兼容 Bearer 和精确 CORS 来源的后端，再发布本目录产物。源码构建不会自动部署或修改线上数据。
