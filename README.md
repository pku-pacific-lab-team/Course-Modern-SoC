# 现代 SoC 芯片架构设计

北京大学现代 SoC 芯片架构设计课程实验指导网站。

## 本地预览

安装依赖后运行：

```bash
python -m pip install -r requirements.txt
mkdocs serve
```

然后打开终端提示的本地地址。网站内容位于 `2026-spring/`，每年可以复制一个新的年度目录并更新 `mkdocs.yml` 中的 `docs_dir` 和导航。

## 发布

推送到 `main` 或 `master` 分支后，GitHub Actions 会运行 `mkdocs gh-deploy --force`，将网站发布到 GitHub Pages。

## 目录结构

```text
.
├── 2026-spring/       # 2026 春季课程内容
├── .github/workflows/  # GitHub Pages 自动部署
├── mkdocs.yml         # MkDocs 配置
└── requirements.txt    # Python 依赖
```
