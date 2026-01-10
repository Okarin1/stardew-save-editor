# 星露谷物语存档编辑器 - Web 版

这是星露谷物语存档编辑器的 Web 前端项目。

## 项目说明

本项目已与 Electron 桌面客户端分离：
- **Web 版（本项目）**: 纯前端 Vue.js 应用，可部署到 Web 服务器
- **Electron 版**: 独立的桌面客户端项目，位于 `../stardew-save-editor-electron`

## 功能

- 星露谷物语存档文件解析和编辑
- 玩家信息管理
- 金钱、技能等属性修改
- 支持浏览器环境和 Electron 环境

## 开发

```bash
# 安装依赖
npm install

# 启动开发服务器
npm run dev

# 构建生产版本
npm run build

# 预览构建结果
npm run preview
```

## 部署

构建后的文件在 `dist` 目录中，可以部署到任何静态文件服务器。

当前线上版本: https://stardewsave.okarin.cn/

## 与 Electron 版的关系

Electron 版本会加载线上的 Web 应用 URL，并为其提供本地文件系统访问能力。Web 版本通过检测 `window.electronAPI` 来判断是否在 Electron 环境中运行，从而使用不同的文件操作方式。

## 技术栈

- Vue 3
- Vite
- Element Plus
- fast-xml-parser
