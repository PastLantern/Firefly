# 投稿规范（给 AI 助手的说明）

这份文档写给负责写文章的 AI 助手。读完后你要能独立完成「写一篇文章并提交」这件事，不需要再问人。

## 一、这个仓库是什么

- 一个 **Astro** 静态博客，主题是 Firefly
- 部署在 **Cloudflare Pages**：`push` 到 `master` 就自动构建上线，不需要你管部署
- 线上地址：https://blog.15242001.xyz
- **它不是** GitHub Pages 的 Jekyll/Hugo 项目，没有 `_posts` 目录，也不能把 HTML 放根目录

## 二、你要做的一件事

在 `src/content/posts/` 下新建一个 `.md` 文件，内容符合下面的规范，然后提交到 `master` 分支。

## 三、文章放在哪、叫什么

```
src/content/posts/<slug>.md
```

`slug` 命名规则：

- 只能用小写英文字母、数字、连字符
- 不能有中文、空格、下划线
- 日期类文章用 `类型-YYYY-MM-DD`，例如 `ai-news-2026-11-16`
- 其他文章用英文短横线串，例如 `how-i-built-this-blog`

文件名去掉 `.md` 就是 `slug`，**两者必须一致**。

## 四、frontmatter 字段

文件开头必须是 YAML frontmatter，下面这些字段名和格式不能改：

| 字段 | 必填 | 类型 | 说明 |
| --- | :---: | --- | --- |
| `title` | ✅ | 字符串 | 文章标题 |
| `published` | ✅ | 日期 | **只能是 `YYYY-MM-DD`**，不能带时间 |
| `description` | ✅ | 字符串 | 摘要，60–120 字，显示在列表页和分享卡片 |
| `image` | ✅ | 字符串 | 封面图，见第六节。没有就写 `''` |
| `tags` | ✅ | 数组 | 必须方括号，如 `[AI, 资讯]` |
| `category` | ✅ | 字符串 | 只能从第七节的分类里选 |
| `draft` | ✅ | 布尔 | `true` 草稿不发布，`false` 正式发布 |
| `lang` | | 字符串 | 留空即可 |
| `slug` | ✅ | 字符串 | 与文件名一致，不含 `.md` |

以下字段可选，一般不用写：`updated`、`pinned`、`author`、`sourceLink`、`licenseName`、`licenseUrl`、`comment`、`password`、`series`。

## 五、硬性规则（都是踩过的坑）

1. **时间必须转义**：正文里写时间（如 `08:00`）必须写成 `08\:00`，**反斜杠不能省**。否则会被 markdown 插件吞成 `08<00>`。
2. **正文不能有 `<html>` `<style>` `<script>` 标签**。这是博客，不是网页模板。
3. **绝对不要写任何 token、密钥、密码、邮箱密码**。仓库是公开的，写进去等于泄露。
4. **`tags` 是数组不是字符串**：写 `[AI, 资讯]`，不要写 `"AI, 资讯"`。
5. **`published` 不要带时间**：写 `2026-11-16`，不要写 `2026-11-16 08:00:00`。
6. **链接用标准 Markdown**：`[文字](https://...)`。
7. **第一次投稿先写 `draft: true`**，等人确认后再改 `false`。

## 六、图片怎么处理

你不能往仓库里传二进制文件，所以：

- **封面**：`image:` 可以留空 `''`，也可以填一个**公开可访问的图片 URL**（`https://...`）。不要填本地相对路径。
- **正文配图**：用外链，`![描述](https://...)`。不要写 `./images/xxx.png` 这种本地路径——仓库里没有那个文件，构建后是坏图。

## 七、分类与标签白名单

**分类只能从这三个里选**，不要在 `category` 里造新词：

| 分类 | 用在哪 |
| --- | --- |
| `随笔` | 日常记录、想法、杂谈 |
| `论文日报` | arXiv 论文汇总类 |
| `资讯早报` | 每日 AI 新闻汇总类 |

要新增分类，先在 Issue 或对话里确认。

**标签复现已有的**（可以新增，但别每条都造新标签）：

`AI` `资讯` `早报` `论文` `大模型` `arXiv` `日报` `随笔` `建站`

## 八、提交之后会发生什么

```
你 push 到 master
   ↓
GitHub Actions 跑检查（build.yml: astro check + 构建）
   ↓ 格式不对会在这里报错
Cloudflare Pages 自动构建
   ↓
约 1 分钟后线上生效
```

- **GitHub Actions 红了** → 十有八九是 frontmatter 格式错，看报错改。
- **构建失败不会让站点挂掉** → Cloudflare 保留上一次成功的版本。
- **`draft: true` 时** → 能构建成功，但文章不出现在列表里。

## 九、不要碰的文件

除非明确要求，**不要修改**：

- `.github/workflows/`（CI 配置）
- `src/config/`（站点配置）
- `astro.config.mjs`、`package.json`、`pnpm-lock.yaml`
- `src/content/spec/`（关于页、友链页等特殊页面）

## 十、完整示例

文件路径：`src/content/posts/ai-news-2026-11-16.md`

```markdown
---
title: AI 资讯早报 · 2026-11-16
published: 2026-11-16
description: 11 月 15 日 08:00 到 11 月 16 日 08:00 的 10 条 AI 资讯，覆盖模型发布、融资、产品更新与监管动态。
image: ''
tags: [AI, 资讯, 早报]
category: 资讯早报
draft: true
lang: ''
slug: ai-news-2026-11-16
---

> 晨间读报，先看结论。

## 1. 某公司发布新模型

**要点**　这里写核心信息。

**看点**　这里写为什么值得关注。

**来源**　[某媒体](https://example.com/article)
```

## 十一、提交前自查

- [ ] 文件在 `src/content/posts/` 下，文件名是英文 slug
- [ ] frontmatter 九个字段齐全，`published` 是 `YYYY-MM-DD`
- [ ] `tags` 是方括号数组
- [ ] `category` 在三选一里
- [ ] 正文没有 `<script>` `<style>`
- [ ] 时间写成 `08\:00`
- [ ] 没有写任何密钥
- [ ] 第一次投稿 `draft: true`
