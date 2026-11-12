# AGENTS.md

## 0. 这个项目是什么

CAT105TC / CAT204TC / CAT207TC 三门课的**「不用开口的助教」**：学生卡住时能自己翻到答案的
补充材料站。以 **AY26-27** 为参照学年。

它不是课程材料的副本，也不是课程的第二份讲义。课程本体的规则在
`/Users/gyk/Documents/Work/AY26-27/AGENTS.md`，本文件只写本站自己的规矩。

## 1. 与课程本体的关系：索引，不复制

课程本体在 `/Users/gyk/Documents/Work/AY26-27`（git repo `SonettoGu14/AY26-27`，private）。

| 需要判断的事 | 去课程本体读 |
| --- | --- |
| 某一周到底讲了什么 | `bulletpoints/<COURSE>/W{N}-<type>.bullets.md`（109 个，source of truth） |
| 模块的官方要求 | `CAT105TC/CAT105TC_AY2627.pdf`、`CAT204TC/CAT204TC_AY2627.pdf`、`CAT207TC/CAT207TC_AY2627.pdf` |
| 课件实际长什么样 | `CAT*/Slides/` |
| 评估设计 | `学年共享文件/assessment-plan-ay2627.md`、`<course>/Assessment/` |

方向是**单向**的：本站需要判断「这个点课上讲没讲过」时去读课程本体；课程本体不引用本站，
本站也绝不复制课程正文。两边只共享**术语**（glossary），不共享内容。

## 2. 边界（硬规则）

- 只写课上没有、或课上放不下的东西：前置数学、渲染与 XR 原理、术语、速查、配置排错。
- 不复制 lecture 的措辞、例子和时序。课件里的例子不搬过来。
- 不写任何 assessment 相关的内容：rubric、brief、canary、分数、提交时间，一律不出现。
- 本站不计分、不替代课堂；`start/scope` 那一页负责把这句话说清楚。
- 学生面向页面的措辞禁忌（与课程本体一致）：
    - 不出现 `TA` / 助教 / teaching assistant（本课程不配助教）。
    - 不出现 submit / submission / upload / deadline / 迟交。
    - CAT207TC 相关例子不假设博物馆 / 展厅主题。
- 本站公开，**学生数据永不入库**。

## 3. 双语规则

- 每页必须成对：`<stem>.en.md` + `<stem>.zh.md`，同目录、同 stem。
- 构建配了 `fallback_to_default: true`，缺中文会回退英文——那是安全网，**不是许可**。只写英文
  等于破规则。
- 中文按中文写，不逐行翻译：同结构、同例子、同顺序，措辞自然。
- 术语以 `docs/glossary/index.{en,zh}.md` 为准；两版冲突以**英文**为准，并改中文版。
- 两版结构必须对应（小节标题一一对上），否则学生切换语言会迷路。

## 4. 目录结构

```
docs/
  index.{en,zh}.md          首页：定位 + 「先看哪一页」表
  start/scope.{en,zh}.md    使用说明与边界
  maths/                    游戏里真正用到的数学（向量与点乘已写）
  shaders/                  着色器原理与效果分解
  narrative/                叙事手法：麦高芬、英雄之路、世界观、高概念（已写）
  xr/                       XR 尺度、舒适度、词汇
  glossary/                 四张术语表（数学与空间 / 渲染与着色器 / XR / C# 与 Unity）
  reference/                速查与配置（尚未写）
mkdocs.yml                  nav、主题、i18n 插件
pyproject.toml + uv.lock    依赖（uv 管理，勿用 pip/conda）
.github/workflows/deploy.yml  push main → GitHub Pages
```

**板块是主题，不是课程。** 每个 section 是一个独立主题 —— `maths/`（数学）、`shaders/`
（着色器）、`narrative/`（叙事手法）、`xr/`。新增板块按主题命名，不要按课程或周次命名。正文也
不依赖课程进度：一页只在「这个概念第一次出现在学生手上」时有用，跟它是第几周讲的无关。
`docs/index.*.md` 的「先看哪一页」表是**唯一**允许把主题映射到课程情境的地方 —— 它是索引，不是
结构。

**叙事板块用主题内的子组组织。** `nav:` 里 `Narrative` 下挂 `Overview` 和一个子组
`Narrative structure`（`nav_translations` 写 `叙事结构`），四页叙事装置放在子组里；`index.en.md`
的 H1 是 `Narrative`、`index.zh.md` 是 `叙事手法`。

## 5. 加一页

1. 写 `docs/<section>/<slug>.en.md` 和 `<slug>.zh.md`
2. `mkdocs.yml` 的 `nav:` 加一行，路径**不带 locale 后缀**（写 `<slug>.md`，不是 `.en.md`）
3. 新标题加进 `plugins.i18n.languages[zh].nav_translations`
4. 该 section 的 index 表格里加一行（状态写 已写 / 计划中）
5. 相关页面双向加内链；内链同样写不带后缀的 `.md`
6. `uv run mkdocs build --strict`

## 6. 命令

```bash
uv sync
uv run mkdocs serve         # http://127.0.0.1:8000
uv run mkdocs build --strict
```

CI 跑的是同一个 `build --strict`。

## 7. 部署与仓库

- 仓库：`SonettoGu14/cat-companion`
- 站点：`https://sonettogu14.github.io/cat-companion/`
- 首次推送后：仓库 Settings → Pages → Build and deployment → Source 选 **GitHub Actions**
- **改名或换 GitHub 用户名要同步改三处**：`mkdocs.yml` 的 `site_url`、`repo_url`、`repo_name`。

## 8. 技术约束（踩过的坑，别再试）

- i18n 是 `suffix` 结构：`nav:` 必须写不带 locale 的 `.md`；写成 `.en.md` 会 strict 构建失败。
- `navigation.instant` 与 mkdocs-static-i18n 不兼容，主题 features 里不要加。
- `theme.font: false`：Google Fonts 在大陆不通，用系统字体。
- 不用 LaTeX / MathJax：那是 CDN 依赖（jsdelivr / unpkg 在大陆不稳）。数学写成代码块或纯文本。
- 不用 mermaid，理由同上。要图就用表格或 ASCII。
- `--strict` 下坏链、未知 nav、未进 nav 的页面都会失败，这是故意的。
- Material 9.x 官方标注 2027-05-05 EOL（后继是 Zensical）。现在不迁移，2027 年再看。

## 9. 学年推进（AY26-27 → AY27-28）

正文与课程解耦，多数页面跨年复用。换学年只需要：

1. 更新 `docs/index.*.md` 的「先看哪一页」表里的课程编号和周次
2. 按新出现的缺口加页（对应 section 表格里 planned → 已写）
3. `maths/` `shaders/` `glossary/` 的正文不需要动

## 10. 写作口径

- 每页只答一个问题，十分钟读完；超过就拆。
- 结尾必有「自检」（`??? question` 折叠答案），让人能自己确认有没有落地。
- 直接、给结论、不铺垫；示例用最小可运行的片段。
- 学生页面不写只有教师看得懂的话（不引用 `bulletpoints`、不引用内部文件路径）。
