# Skara Brae Songbook

爱尔兰盖尔语民谣四重唱 **Skara Brae**（1971 同名专辑）歌词的三语对照网站：**盖尔语原文 · 英译 · 中文**。单页静态站，纯个人欣赏与学习用途。

盖尔语不作直译，故每首保留「原文 → 英译 → 中文」三层：英译作校准中介，中文据英译转出。页面上盖尔语原文始终显示在左，右栏可在 **中文 / English** 间切换对照。

## 结构

```
skara-brae-songbook/
├── public/
│   ├── index.html      # 页面结构 + 样式 + 渲染逻辑（不含歌词数据）
│   ├── data/
│   │   └── albums.js   # 全部歌词数据（window.ALBUMS 数组）
│   ├── robots.txt      # 禁止搜索引擎收录
│   └── .nojekyll       # 关闭 GitHub Pages 的 Jekyll 处理
├── .github/workflows/
│   └── deploy.yml      # GitHub Actions 自动部署到 Pages
├── README.md
└── .gitignore
```

**数据与代码分离，但仍是零依赖静态站**（双击 `index.html` 即可打开，无需服务器或构建）：
- `public/data/albums.js` —— 全部歌词数据，一个 `window.ALBUMS` 数组，每首歌一个对象。**改歌词只动这个文件。**
- `public/index.html` —— 页面骨架、样式和渲染逻辑，用 `<script src="data/albums.js">` 引入数据。改样式或交互只动这里。

## 数据格式

```js
{
  t: "歌名",
  lang: ["GA"],             // 语言码：GA 爱尔兰语 / EN 英语 / Instr. 器乐
  note: "一句背景介绍（可选）",
  lyrics: [
    { role: "Verse 1",      // 段落标记（可选）
      chorus: false,        // true = 副歌样式（ember 竖线高亮）
      orig:  ["盖尔语第一行", ...],   // 原文（锚，始终在左）
      en:    ["English line", ...],   // 英译（校准中介）
      trans: ["中文第一行", ...] },     // 中文
  ]
}
```

- `lyrics: null`（或不写）= 整首待录入，页面显示占位提示，导航标"待录"。
- `transOnly: true` = 只有中文、暂缺原文与英译（页面标「暂缺英译」，只显示中文一栏）。
- 纯器乐：`lang: ["Instr."]`，不写 lyrics，`note` 里注明。
- 反复的副歌只列一次，在 `role` 或 `note` 里注明"全曲反复"。

## 部署（GitHub Pages via Actions）

通过 `.github/workflows/deploy.yml` 自动部署：`main` 分支一 push，GitHub Actions 就把 `public/` 目录发布为站点。

**首次启用**（在 GitHub 仓库网页上）：
1. 仓库 **Settings → Pages → Build and deployment → Source** 选 **GitHub Actions**。
2. push 后在 **Actions** 标签看部署进度，成功后 Pages 里显示网址（形如 `https://<用户名>.github.io/skara-brae-songbook/`）。

> 仓库虽公开，但 `robots.txt` + `index.html` 里的 `noindex` meta 会阻止搜索引擎收录。

本地预览：任意静态服务器指向 `public/`，例如 `npx serve public`。

## 待补

- **An Chrúbach**：整首待录（原豆瓣文只给了链接，无正文）。
- **Suantraí Hiúdaí · Bánchnoic Éireann Ó · An Saighdiúir Tréigthe · Caitlín Óg**：仅有中文译文，暂缺盖尔语原文与英译（原文里只收录了中文）。补上原文+英译后即可走完整三语对照。
- **Angela · Casadh an tSúgáin**：纯器乐，无歌词。

## 版权

歌词版权归原作者所有。本站仅供个人欣赏与学习。仓库公开托管于 GitHub Pages，但已用 robots/noindex 阻止搜索引擎收录，不主动对外分发。
