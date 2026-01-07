# MkDocs 博客

基于 MkDocs + Material 主题的个人技术博客，支持自动部署到 GitHub Pages。

## 项目简介

- **框架**: MkDocs (Python 静态网站生成器)
- **主题**: Material for MkDocs
- **部署**: GitHub Pages
- **CI/CD**: GitHub Actions 自动化部署
- **作者**: Chester Zhao (saymealoud)

## 快速开始

### 1. 环境准备

确保已安装 Python 3.9 或更高版本，然后安装依赖：

```bash
# 克隆仓库
git clone https://github.com/saymealoud/saymeevetime.icu.git
cd saymeevetime.icu

# 安装依赖
pip install -r requirements.txt
```

### 2. 本地预览

```bash
# 启动本地开发服务器
mkdocs serve

# 访问 http://127.0.0.1:8000 预览博客
```

## 写博文步骤

### 第一步：创建博文文件

在 `docs/posts/` 目录下创建新的 Markdown 文件：

```bash
# 文件名建议使用英文，如：
touch docs/posts/my-new-post.md
```

### 第二步：编写博文元数据

在文件开头添加 YAML Front Matter 元数据：

```yaml
---
draft: false                    # false=发布, true=草稿
date: 2024-03-14               # 发布日期
categories:
  - tech                       # 分类：tech(技术) 或 book(书籍)
comments: true                 # 是否启用评论
authors: [saymealoud]          # 作者（需在 docs/.authors.yml 中配置）
---
```

### 第三步：编写博文内容

元数据下方开始编写正文内容，支持丰富的 Markdown 语法：

````markdown
# 文章标题

文章摘要或简介...

## 章节标题

正文内容...

### 代码块示例

```python
def hello():
    print("Hello, World!")
```

### 注解示例

!!! note "提示"
    这是一个注解块

### 标签页示例

=== "Python"
    ```python
    print("Hello")
    ```

=== "JavaScript"
    ```javascript
    console.log("Hello")
    ```
````

### 第四步：添加图片（可选）

如果博文需要图片，建议在 `docs/posts/images/` 下创建对应目录：

```bash
# 创建图片目录
mkdir -p docs/posts/images/my-new-post

# 将图片放入该目录，然后在博文中引用
![图片描述](images/my-new-post/example.png)
```

### 第五步：本地预览

在终端运行开发服务器，实时预览效果：

```bash
mkdocs serve
```

访问 http://127.0.0.1:8000 查看博文效果。

### 第六步：提交并发布

确认无误后，提交代码到 GitHub：

```bash
# 添加文件
git add .

# 提交更改
git commit -m "新增博文: 文章标题"

# 推送到 main 分支
git push origin main
```

推送后，GitHub Actions 会自动构建并部署到 GitHub Pages。

## 项目结构

```
.
├── docs/                      # 文档目录
│   ├── index.md              # 博客首页
│   ├── .authors.yml          # 作者配置
│   └── posts/                # 博文目录
│       ├── *.md             # 博文文件
│       └── images/          # 图片资源
├── overrides/                # 主题自定义
│   └── partials/            # 模板覆盖
├── .github/
│   └── workflows/
│       └── MkDocs.yml       # CI/CD 配置
├── mkdocs.yml               # MkDocs 配置文件
├── requirements.txt         # Python 依赖
└── README.md               # 本文件
```

## 配置说明

### 作者配置

编辑 `docs/.authors.yml` 添加或修改作者信息：

```yaml
authors:
  saymealoud:
    name: Chester Zhao
    description: Creator
    avatar: https://github.com/saymealoud.png
```

### 站点配置

主要配置在 `mkdocs.yml` 中：

- `site_name`: 站点名称
- `site_author`: 作者名称
- `repo_url`: GitHub 仓库地址
- `theme`: 主题配置（Material）
- `plugins`: 插件配置（搜索、标签、博客）
- `markdown_extensions`: Markdown 扩展功能

## 支持的 Markdown 功能

- ✅ 代码高亮（支持行号）
- ✅ 代码复制按钮
- ✅ 标签页（Tabbed）
- ✅ 注解块（Admonitions）
- ✅ 脚注（Footnotes）
- ✅ 键盘按键（Keys）
- ✅ 高亮标记（Mark）
- ✅ 删除线、下划线等
- ✅ 明暗主题切换

## 常用命令

```bash
# 本地开发
mkdocs serve

# 构建静态网站
mkdocs build

# 部署到 GitHub Pages（手动）
mkdocs gh-deploy

# 查看 MkDocs 版本
mkdocs --version
```

## 常见问题

### Q: 如何将博文设置为草稿？

A: 在博文元数据中设置 `draft: true`，该文章将不会在生产环境显示。

### Q: 如何更改博客名称？

A: 编辑 `mkdocs.yml` 中的 `site_name` 字段。

### Q: 如何添加新的分类？

A: 在博文元数据的 `categories` 中添加新分类名称即可，MkDocs 会自动识别。

### Q: 部署失败怎么办？

A: 检查 GitHub Actions 运行日志，确保：
- Python 依赖安装成功
- `mkdocs build` 命令无错误
- GitHub Pages 权限配置正确

## 许可证

个人博客项目，仅供学习记录使用。

## 联系方式

- GitHub: [@saymealoud](https://github.com/saymealoud)
- 仓库地址: [saymeevetime.icu](https://github.com/saymealoud/saymeevetime.icu)
