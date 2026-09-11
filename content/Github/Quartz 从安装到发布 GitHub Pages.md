这份笔记记录如何在 macOS 上从零安装 Quartz、创建本地知识博客，并发布到
GitHub Pages。

------------------------------------------------------------------------

## 1. 整体工作流

``` text
Bear
  ↓ 导出 Markdown
Quartz content/
  ↓
本地预览
  ↓
GitHub Repository
  ↓
GitHub Actions
  ↓
GitHub Pages
  ↓
公开 Blog
```

Quartz 不是笔记软件本身。它主要负责把 Markdown
笔记转换成可以在浏览器中访问的网站。

------------------------------------------------------------------------

## 2. 准备环境

Quartz 需要 Git 和 Node.js。

在 Terminal 中检查：

``` bash
git --version
node -v
npm -v
```

如果已经显示版本号，可以继续。

如果使用 Homebrew，可以安装：

``` bash
brew install git
brew install node
```

------------------------------------------------------------------------

## 3. 安装 Quartz

进入希望保存 Quartz 的目录：

``` bash
cd ~/Documents
```

Clone Quartz：

``` bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm install
```

初始化：

``` bash
npx quartz create
```

根据提示完成初始化。第一次使用可以采用默认设置。

安装完成后，目录大致如下：

``` text
quartz/
├── content/
├── quartz/
├── quartz.config.ts
├── quartz.layout.ts
└── package.json
```

其中最重要的是：

``` text
content/
```

以后需要发布的 Markdown 笔记主要放在这里。

------------------------------------------------------------------------

## 4. 创建首页

确保存在：

``` text
content/index.md
```

例如：

``` markdown
---
title: My Research Notes
---

# My Research Notes

Welcome to my personal knowledge base.

## Topics

- [[SMA]]
- [[Ferroelectrics]]
- [[Machine Learning]]
- [[Paper Notes]]
```

`index.md` 会作为网站首页。

如果没有首页，访问根地址时可能看到 Quartz 的 404 页面。

------------------------------------------------------------------------

## 5. 本地运行 Quartz

进入 Quartz 根目录：

``` bash
cd ~/Documents/quartz
```

运行：

``` bash
npx quartz build --serve
```

Terminal 会显示本地地址，通常为：

``` text
http://localhost:8080
```

在浏览器打开这个地址即可预览。

注意：

``` text
localhost:8080
```

只是本地预览地址，其他人无法通过互联网访问。

------------------------------------------------------------------------

## 6. 从 Bear 添加笔记

在 Bear 中选择需要发布的笔记，然后导出为 Markdown。

建议不要一次把整个 Bear 笔记库全部公开，而是只选择已经整理好的内容。

例如建立：

``` text
content/
├── index.md
├── SMA/
├── Ferroelectrics/
├── Machine-Learning/
└── Paper-Notes/
```

把 Bear 导出的笔记放入对应目录，例如：

``` text
content/SMA/Transformation Stress.md
```

可以在笔记顶部加入 Quartz frontmatter：

``` markdown
---
title: Transformation Stress in Shape Memory Alloys
tags:
  - SMA
  - phase-transformation
  - mechanical-properties
---

# Transformation Stress in Shape Memory Alloys

Transformation stress is ...

## Related Notes

- [[Transformation Strain]]
- [[Superelasticity]]
```

Quartz 支持 `[[WikiLinks]]`，可以把不同笔记连接起来。

------------------------------------------------------------------------

## 7. 推荐的 Bear → Quartz 工作方式

不建议：

``` text
Bear
  ↓
全部导出
  ↓
Quartz
```

更推荐：

``` text
Bear
私人 / Working Notes
  ↓
选择 + 整理
  ↓
Quartz
Public Knowledge Base
```

也就是说：

-   Bear 用于日常笔记、草稿和未整理内容。
-   Quartz 用于经过整理、准备公开的知识笔记。

------------------------------------------------------------------------

## 8. 创建 GitHub Repository

登录 GitHub：

<https://github.com/>

创建一个新的 repository。

### 方案 A：个人主页

如果 GitHub 用户名是：

``` text
YOUR_USERNAME
```

Repository 命名为：

``` text
YOUR_USERNAME.github.io
```

最终网站地址通常是：

``` text
https://YOUR_USERNAME.github.io/
```

### 方案 B：项目主页

Repository 也可以命名为：

``` text
research-notes
```

最终地址通常是：

``` text
https://YOUR_USERNAME.github.io/research-notes/
```

对于个人知识 Blog，`YOUR_USERNAME.github.io` 通常最简洁。

------------------------------------------------------------------------

## 9. 将本地 Quartz 连接到自己的 GitHub Repository

进入 Quartz：

``` bash
cd ~/Documents/quartz
```

检查当前 remote：

``` bash
git remote -v
```

因为 Quartz 是从官方 repository clone 的，`origin` 可能仍然指向 Quartz
官方 repository。

如果是这样，可以移除：

``` bash
git remote remove origin
```

然后连接自己的 repository：

``` bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_USERNAME.github.io.git
```

再次检查：

``` bash
git remote -v
```

------------------------------------------------------------------------

## 10. 第一次上传到 GitHub

执行：

``` bash
git add .
git commit -m "Initial Quartz blog"
git branch -M main
git push -u origin main
```

完成以后，刷新 GitHub repository 页面，应该可以看到 Quartz 文件。

注意：上传源代码到 GitHub 后，还没有完成 GitHub Pages 部署。

------------------------------------------------------------------------

## 11. 配置 Quartz 网站信息

打开：

``` text
quartz.config.ts
```

找到 configuration，例如：

``` ts
configuration: {
  pageTitle: "Quartz 5",
```

可以修改：

``` ts
pageTitle: "My Research Notes",
```

找到 `baseUrl`。

如果使用：

``` text
YOUR_USERNAME.github.io
```

通常设置为：

``` ts
baseUrl: "YOUR_USERNAME.github.io",
```

不要在这里加入 `https://`。

修改后提交：

``` bash
git add .
git commit -m "Configure site"
git push
```

------------------------------------------------------------------------

## 12. 配置 GitHub Pages

进入 GitHub repository：

``` text
Settings
→ Pages
```

在 Build and deployment 中选择：

``` text
Source
→ GitHub Actions
```

Quartz 使用 GitHub Actions 构建并部署静态网站。

由于 Quartz 的 workflow 可能随版本更新，部署文件建议直接参考当前官方
Hosting 文档：

<https://quartz.jzhao.xyz/hosting>

按照当前 Quartz 5 官方说明添加 GitHub Pages workflow。

------------------------------------------------------------------------

## 13. 检查 GitHub Actions

进入 repository：

``` text
Actions
```

等待 workflow 完成。

理想情况下会看到类似：

``` text
Build   ✓
Deploy  ✓
```

如果 workflow 失败，点击失败的 job 查看具体错误信息。

部署成功以后，可以访问：

``` text
https://YOUR_USERNAME.github.io/
```

这才是真正公开的网站，可以直接分享给其他人。

------------------------------------------------------------------------

## 14. localhost 与 GitHub Pages 的区别

  项目                 localhost   GitHub Pages
  -------------------- ----------- --------------
  本地预览             ✓           ✓
  其他人访问           ✗           ✓
  Mac 关机后仍可访问   ✗           ✓
  HTTPS                ---         ✓
  免费托管             ---         ✓
  可作为公开 Blog      ✗           ✓

------------------------------------------------------------------------

## 15. 以后如何发布新笔记

日常工作流非常简单。

### Step 1：在 Bear 写笔记

例如：

``` text
Transformation Stress
```

### Step 2：导出 Markdown

放到：

``` text
quartz/content/SMA/Transformation Stress.md
```

### Step 3：本地检查

``` bash
cd ~/Documents/quartz
npx quartz build --serve
```

打开：

``` text
http://localhost:8080
```

### Step 4：发布

确认没有问题后：

``` bash
git add .
git commit -m "Add transformation stress note"
git push
```

GitHub Actions 会自动重新构建网站。

因此以后完整流程是：

``` text
Bear
 ↓
Export Markdown
 ↓
Quartz content/
 ↓
Local Preview
 ↓
git push
 ↓
GitHub Actions
 ↓
Blog 自动更新
```

------------------------------------------------------------------------

## 16. 常用命令速查

### 启动本地预览

``` bash
cd ~/Documents/quartz
npx quartz build --serve
```

### 查看 Git 状态

``` bash
git status
```

### 发布修改

``` bash
git add .
git commit -m "Update notes"
git push
```

### 查看远程 repository

``` bash
git remote -v
```

### 查看最近 commit

``` bash
git log --oneline -5
```

------------------------------------------------------------------------

## 17. 推荐的 Blog 结构

例如：

``` text
content/
├── index.md
│
├── Materials-Science/
│   ├── SMA/
│   │   ├── Transformation-Temperature.md
│   │   ├── Transformation-Stress.md
│   │   ├── Transformation-Strain.md
│   │   └── Superelasticity.md
│   │
│   └── Ferroelectrics/
│       ├── BaTiO3.md
│       ├── Pyroelectric-Effect.md
│       └── Olsen-Cycle.md
│
├── Machine-Learning/
│   ├── Regression-Metrics.md
│   ├── PCA.md
│   ├── Random-Forest.md
│   └── Text-Embedding.md
│
└── Paper-Notes/
    └── ...
```

------------------------------------------------------------------------

## 18. 最终架构

``` text
                    PRIVATE
                       │
                     Bear
                       │
              Select + Organize
                       │
                       ▼
                 Markdown Notes
                       │
                       ▼
                    Quartz
                       │
             ┌─────────┴─────────┐
             │                   │
             ▼                   ▼
      localhost:8080          GitHub
       Local Preview             │
                                 ▼
                          GitHub Actions
                                 │
                                 ▼
                          GitHub Pages
                                 │
                                 ▼
                           Public Blog
```

这样可以继续使用 Bear 作为主要笔记工具，同时利用 Quartz
建立一个公开、可搜索、支持 WikiLinks 和知识图谱的个人学习 Blog。
