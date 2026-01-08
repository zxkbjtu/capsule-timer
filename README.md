# CapsuleTimer

一个简洁优雅的桌面计时器应用，基于 Electron + Vue 3 + TypeScript 构建。

## 功能特点

- 🎯 简洁的计时器界面，支持分钟和秒的设置
- ⏰ 支持鼠标滚轮快速调整时间
- 🔔 计时结束时系统通知提醒
- 💫 流畅的动画效果和交互体验
- 🎨 现代化的 UI 设计，使用 Tailwind CSS

## 技术栈

- **框架**: Electron + Vue 3
- **语言**: TypeScript
- **样式**: Tailwind CSS
- **构建工具**: Vite
- **图标**: Lucide Vue Next

## 开发环境要求

- Node.js >= 16
- npm 或 pnpm

## 安装与运行

1. 克隆项目

```bash
git clone [repository-url]
cd capsule-timer
```

2. 安装依赖

```bash
npm install
```

3. 开发模式运行

```bash
npm run dev
```

## 构建与打包

### 开发构建

```bash
npm run build
```

### 生产构建

#### Windows

```bash
npm run build:win
```

#### macOS

```bash
npm run build:mac
```

#### Linux

```bash
npm run build:linux
```

## 项目结构

```
src/
├── main/          # Electron 主进程
├── preload/       # 预加载脚本
└── renderer/      # Vue 渲染进程
    ├── src/
    │   ├── components/
    │   └── assets/
```

## 开发脚本

- `dev`: 启动开发环境
- `build`: 构建应用
- `typecheck`: 类型检查
- `lint`: 代码检查
- `format`: 代码格式化

## 许可证

MIT License

## 贡献

欢迎提交 Issue 和 Pull Request！
