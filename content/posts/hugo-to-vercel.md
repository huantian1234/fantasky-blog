+++
date = '2025-09-25T21:43:14+08:00'
draft = false
title = 'Hugo部署到Vercel完整指南'
tags = ['Hugo', 'Vercel', '部署', '静态网站']
categories = ['教程']
+++

本文将详细介绍如何使用Hugo创建静态网站并部署到Vercel平台，包括完整的安装、配置和部署流程。

## 📦 Hugo安装与环境配置

### Windows环境安装
使用Windows包管理器安装Hugo Extended版本：
```powershell
winget install Hugo.Hugo.Extended
```

### 验证安装
安装完成后，验证Hugo版本：
```bash
hugo version
```

## 🚀 创建和配置Hugo站点

### 1. 创建新站点
```bash
hugo new site my-hugo-site
cd my-hugo-site
```

### 2. 创建第一篇文章
```bash
hugo new posts/my-first-post.md
```

### 3. 编辑文章内容
打开生成的文章文件，修改头部信息：
```yaml
+++
title = '我的第一篇文章'
date = 2025-01-20T10:00:00+08:00
draft = false 
tags = ['Hugo', '开始']
+++

这是我的第一篇Hugo文章内容...
```

## 🔍 本地预览和测试

### 启动开发服务器
```bash
hugo server -D #-D代表显示草稿文章也就是draft = true的文章
```


访问 `http://localhost:1313` 预览网站效果。

### 生成静态文件
```bash
hugo
```
生成的静态文件位于 `public/` 目录下。

## 📋 Git仓库配置

### 1. 初始化仓库并提交代码
```bash
git add .
git commit -m "Initial Hugo site"
```

### 2. 推送到GitHub
```bash
git remote add origin https://github.com/yourusername/your-repo.git
git branch -M main
git push -u origin main
```

## 🌐 Vercel部署配置

### 1. 创建Vercel账号
- 访问 [Vercel官网](https://vercel.com/)
- 使用GitHub账号登录（推荐）

### 2. 导入GitHub项目
- 点击 **"New Project"** 按钮
- 选择 **"Import Git Repository"**
- 选择你的Hugo项目仓库
- 点击 **"Import"**




### 3. 环境变量设置
在 **Settings > Environment Variables** 中添加：

| 变量名            | 值         | 说明       |
| -------------- | --------- | -------- |
| `HUGO_VERSION` | `0.150.0` | 指定Hugo版本 |

### 4. 配置baseURL
编辑项目根目录的 `config.toml` 文件：
```toml
baseURL = 'https://your-project.vercel.app'
languageCode = 'zh-cn'
title = '我的Hugo博客'
```



## 📚 资料

- [Hugo官方文档](https://gohugo.io/documentation/)
- [Vercel部署文档](https://vercel.com/docs/concepts/deployments/overview)
- [Hugo主题库](https://themes.gohugo.io/)

