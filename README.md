# My Blog

这是一个基于 [Hexo](https://hexo.io/) 框架构建的个人博客项目，使用了 [Aurora](https://github.com/auroral-ui/hexo-theme-aurora) 主题。

## 环境准备

- Node.js
- pnpm (推荐) 或 npm

## 安装

1. 克隆仓库：
   ```bash
   git clone <repository-url>
   cd my-blog
   ```

2. 安装依赖：
   ```bash
   pnpm install
   # 或者
   npm install
   ```

## 使用指南

### 本地运行

启动本地开发服务器：

```bash
pnpm server
# 或者
hexo server
```

访问 `http://localhost:4000` 查看博客。

### 构建

生成静态文件：

```bash
pnpm build
# 或者
hexo generate
```

### 清理

清理缓存文件 (`db.json`) 和生成的静态文件 (`public`)：

```bash
pnpm clean
# 或者
hexo clean
```

### 部署

部署到远程站点（需配置 `_config.yml` 中的 deploy 部分）：

```bash
pnpm deploy
# 或者
hexo deploy
```

## 配置说明

- **站点配置**: `_config.yml` - 配置网站标题、作者、语言等基本信息。
- **主题配置**: `_config.aurora.yml` - 配置 Aurora 主题的特定选项，如菜单、颜色、插件等。

## 目录结构

- `source/`: 存放博客文章 (`_posts`) 和页面 (`about`, `links` 等) 的 Markdown 源文件。
- `themes/`: 存放主题文件。本项目使用的是 `hexo-theme-aurora`。
- `public/`: 执行 `hexo generate` 后生成的静态文件目录。
- `scaffolds/`: 文章模版文件夹。

## 常用命令

- `hexo new "文章标题"`: 新建一篇文章。
- `hexo new page "页面名称"`: 新建一个页面。
