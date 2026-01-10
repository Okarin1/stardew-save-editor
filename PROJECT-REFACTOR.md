# 项目重构说明

## 重构内容

项目已成功拆分为两个独立的项目：

### 1. Web 前端项目（原项目）
**位置**: `c:\Users\Deng\code\stardew-save-editor`

**改动**:
- ✅ 删除了 `electron/` 目录
- ✅ 清理了 `package.json` 中的 electron 相关依赖和脚本
- ✅ 删除了 `ELECTRON-FEATURES.md` 和 `README-ELECTRON.md`
- ✅ 添加了 `README-WEB.md` 说明文档

**保留的脚本**:
```json
{
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
}
```

**技术栈**:
- Vue 3 + Vite
- Element Plus
- fast-xml-parser

### 2. Electron 桌面客户端项目（新项目）
**位置**: `c:\Users\Deng\code\stardew-save-editor-electron`

**包含文件**:
- `package.json` - 项目配置（仅包含 electron 相关依赖）
- `main.cjs` - Electron 主进程（加载远程 URL: https://stardewsave.okarin.cn/）
- `preload.cjs` - Preload 脚本（提供文件系统 API）
- `README.md` - 项目说明文档
- `.gitignore` - Git 忽略配置

**可用脚本**:
```json
{
  "start": "electron .",
  "build": "electron-builder"
}
```

## 工作原理

1. **Web 版本**: 
   - 纯前端应用，可独立部署
   - 部署地址: https://stardewsave.okarin.cn/
   - 在浏览器中运行时使用浏览器的文件 API

2. **Electron 版本**:
   - 通过 `loadURL()` 加载远程 Web 应用
   - 提供本地文件系统访问能力
   - Web 应用通过 `window.electronAPI` 检测运行环境

## 使用指南

### 开发 Web 版本
```bash
cd c:\Users\Deng\code\stardew-save-editor
npm install
npm run dev
```

### 开发 Electron 版本
```bash
cd c:\Users\Deng\code\stardew-save-editor-electron
npm install
npm start
```

### 构建

**Web 版本**:
```bash
npm run build
# 输出到 dist/ 目录
```

**Electron 版本**:
```bash
npm run build
# 输出到 dist/ 目录（Windows: .exe 安装程序和便携版）
```

## 优势

✅ **关注点分离**: Web 和桌面端代码完全独立
✅ **独立部署**: Web 版本可以独立更新，无需重新打包 Electron
✅ **简化维护**: Electron 项目极简，只负责提供桌面环境和文件系统访问
✅ **灵活性**: 可以随时更换 Web 应用的 URL
✅ **减小体积**: Electron 应用不再包含 Web 应用的构建产物

## 注意事项

- Electron 版本需要保持 `preload.cjs` 中的 API 与 Web 应用中使用的 `window.electronAPI` 一致
- 如果 Web 应用的域名变更，需要修改 Electron 项目中 `main.cjs` 的 `WEB_URL` 常量
- Web 应用需要能够检测并适配 Electron 环境（通过 `window.electronAPI` 判断）
