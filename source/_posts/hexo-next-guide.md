---
title: Hexo + NexT 建站备忘
date: 2026-10-22 22:15:00
updated: 2026-10-22 22:15:00
categories:
  - 折腾
tags:
  - Hexo
  - NexT
  - GitHub Pages
description: 把这次建站过程中真正会反复用到的命令和配置记下来，下次换机器时照着做就行。
---

建站只做一次，但**发布**要一直做。所以这篇只记长期用得上的东西。

## 日常三条命令

```bash
# 新建文章（会在 source/_posts 下生成 md 和同名图片目录）
npx hexo new "文章标题"

# 本地预览，改完保存刷新浏览器即可
npx hexo server          # 默认 http://localhost:4000

# 生成静态文件到 public/
npx hexo clean && npx hexo generate
```

## 文章头部的固定写法

```yaml
---
title: 文章标题
date: 2026-10-22 22:15:00
categories:
  - 折腾
tags:
  - Hexo
description: 一句话摘要，会显示在首页列表和搜索结果里
---
```

`description` 建议写，NexT 用它生成摘要和 `<meta name="description">`，对搜索引擎收录有直接影响。

## 插图

`_config.yml` 里开了 `post_asset_folder: true`，所以每篇文章都有独立图片目录：

```text
source/_posts/hexo-next-guide/
source/_posts/hexo-next-guide/cover.png
source/_posts/hexo-next-guide.md
```

正文里直接写相对路径即可，Markdown 与标签都能用：

```markdown
![封面](cover.png)
```

## 什么时候需要改 `_config.next.yml`

只有改主题外观时才动它。想改站点标题、作者、URL 就动 `_config.yml`，别弄反了——改错文件的现象是"配置写了但页面没变"。

## 从零到上线

```bash
# 1. 换机器后先装依赖
npm install

# 2. 本地确认没问题
npx hexo server

# 3. 提交推送，GitHub Actions 自动构建并发布
git add .
git commit -m "post: 新文章"
git push
```

推送后到仓库的 **Actions** 页面看构建状态，成功即发布完成。首次部署需要在仓库 **Settings → Pages** 把 Source 选成 **GitHub Actions**。

## 几个容易混淆的点

- `hexo clean` 只删生成产物，不删文章；`public/` 是纯输出目录，不要手动往里写东西
- 主题装在 `node_modules/hexo-theme-next`，但配置写在根目录的 `_config.next.yml`，升级主题不会丢配置
- 站内搜索是构建时生成的 `search.xml`，文章多了文件会变大，`limit` 别设置得太大
