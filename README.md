# 学术主页 · Academic Homepage

基于 [al-folio](https://github.com/alshedivat/al-folio) 搭建的个人学术主页。
论文列表由 `.bib` 文件自动生成，正文用 Markdown 写，模板与样式由 gem 提供——**你只需要改内容，不需要碰代码**。

---

## 一、上线：四步

### 1. 在 GitHub 建仓库

新建一个 **public** 仓库，名字必须**正好等于**「你的用户名 + `.github.io`」。例如用户名是 `zhangjp`，仓库就叫 `zhangjp.github.io`。
名字错了站点就不在根路径，后面全要改。

### 2. 把本目录推上去

在**本目录**里执行（把 `USERNAME` 换成你的 GitHub 用户名）：

```bash
git remote add origin git@github.com:USERNAME/USERNAME.github.io.git
git push -u origin main
```

没配 SSH key 的话用 HTTPS：

```bash
git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
git push -u origin main
```

### 3. 打开 Pages

仓库 → **Settings → Pages → Source** 选 **Deploy from a branch**，分支选 **`gh-pages`**，目录选 **`/ (root)`**，保存。

> 注意：不是「GitHub Actions」。这个工作流是把构建结果推到 `gh-pages` 分支，再由 Pages 发布那个分支。

### 4. 改掉配置里的地址

`_config.yml` 第 22 行左右：

```yaml
url: https://USERNAME.github.io   # 改成你的真实用户名，结尾不要加斜杠
baseurl: ""                        # 用户主页仓库必须留空
```

改完 push 一次。**第一次构建大约 2–4 分钟**，在仓库的 **Actions** 页看进度；绿勾之后访问 `https://USERNAME.github.io`。

---

## 二、日常改内容：哪个文件管什么

| 想改什么 | 改哪个文件 |
|---|---|
| 姓名、单位、简介、招生说明 | `_pages/about.md`（顶部 YAML + 正文） |
| 侧边栏头像 | 覆盖 `assets/img/prof_pic.jpg`（**文件名不要改**） |
| 论文列表 | `_bibliography/papers.bib` |
| 首页「最新动态」 | `_news/` 下新建一个 `.md` |
| 教学课程 | `_teachings/` 下新建一个 `.md` |
| 简历页 CV | `_data/cv.yml` |
| 邮箱、Scholar、ORCID、GitHub 链接 | `_data/socials.yml` |
| 全站标题、描述、favicon、页脚 | `_config.yml` |
| 简历 PDF | 覆盖 `assets/pdf/cv.pdf` |

### 加一篇论文

从 Google Scholar 点 **Cite → BibTeX** 复制，粘到 `papers.bib` 末尾：

```bibtex
@inproceedings{zhang2027newpaper,
  abbr        = {VENUE},
  bibtex_show = {true},
  title       = {你的论文标题},
  author      = {Zhang, Junpeng and Others, A.},
  booktitle   = {Proceedings of XXXX},
  year        = {2027},
  selected    = {true},
  pdf         = {https://example.com/paper.pdf},
  code        = {https://github.com/USERNAME/repo},
  arxiv       = {2701.00001}
}
```

- 你自己的名字请写成 `Zhang, Junpeng`——配置里已设定用这个格式加粗。
- 只有 `selected = {true}` 的会出现在首页；全部论文在 `/publications/`。
- `abbr` 是列表里的小标签，随意取；`pdf` / `code` / `arxiv` 没有就删掉那行。

### 加一条动态

在 `_news/` 下建文件，文件名建议 `2027-01-xxx.md`：

```markdown
---
layout: post
date: 2027-01-15 09:00:00+0800
inline: true
related_posts: false
---

一句话说清发生了什么，可以用 **粗体** 和 [链接](https://example.com)。
```

---

## 三、现在还是占位符的地方

用编辑器的全局搜索逐个替换：

| 占位符 | 含义 | 出现在 |
|---|---|---|
| `USERNAME` | GitHub 用户名 | `_config.yml`、`_data/socials.yml`、`papers.bib` |
| `XXXX` / `XXXX University` | 学校、学院、课名、地址 | 几乎所有内容文件 |
| `AREA_A` / `AREA_B` / `AREA_C` | 研究方向 | `about.md`、`papers.bib` |
| `CONCRETE_PROBLEM` | 你正在解决的具体问题 | `about.md` |
| `REAL_WORLD_SETTING` | 你的实验场景 | `about.md` |
| `zhangjp@xxxx.edu.cn` | 邮箱 | `socials.yml`、`cv.yml`、`about.md` |
| `XXXXXXXXXXXX` | Google Scholar user ID | `socials.yml` |
| `0000-0000-0000-0000` | ORCID | `socials.yml` |
| `STUDENT_NAME` / `TALK_TITLE` / `PROJECT_NAME` | 动态里的具体信息 | `_news/*.md` |
| `AWARD_NAME` / `AWARDER` | 获奖信息 | `_data/cv.yml` |

替换完 commit + push，Actions 会自动重新构建。

---

## 四、常见问题

**构建失败**
去仓库 **Actions** 点开失败的那次运行，看红色步骤的日志。九成是 YAML 缩进用了 Tab，或 BibTeX 少了逗号/右括号。

**改了没生效**
Actions 还在跑，或者浏览器缓存。等绿勾后强制刷新（Mac：`Cmd+Shift+R`）。

**想本地预览**
需要 Ruby 3 + Node，然后 `bundle install && bundle exec jekyll serve`。**不太建议**——每次推送让 Actions 构建更省事，本地环境装歪了反而更难排查。

**想加博客**
从上游仓库把 `_pages/blog.md` 复制回来，`_config.yml` 里 `latest_posts.enabled` 改 `true`，在 `_posts/` 下写文章。

**主题文档**
`docs/` 是上游完整文档，其中 `docs/CUSTOMIZE.md` 和 `docs/FAQ.md` 最有用。
