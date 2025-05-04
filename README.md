## Komorebi's Blog
[Komorebi's Blog](https://komorebi.001412.xyz)

> A blog for sharing my thoughts and experiences.
> 当前为**内容仓库**，配合`https://github.com/Komorebi-yaodong/komorebiBlog-frontend`使用


## 内容管理指南

管理博客内容非常简单，修改 **内容仓库** 中的文件即可，前端网站会自动拉取最新内容。

### 添加/修改导航链接

1.  更新 `link.json` 文件：
    *   添加或修改链接条目的 `title`, `link`。
    *   (可选) 添加或修改 `description`。
    *   (可选) 添加或修改 `image` 字段指向 `Linkfigures/` 目录中的图片。
2.  提交更改到内容仓库。


### 创建/修改文章

1.  在内容仓库的 `posts/` 目录中创建或修改 `.md` 文件。
2.  更新 `list.json` 文件：
    *   添加新文章条目或修改现有条目的 `title`, `time`, `file`。
    *   (可选) 添加或修改 `image` 字段指向 `figures/` 目录中的图片。
3.  提交更改到内容仓库。


### 添加图片

1.  将**文章**相关的图片上传到内容仓库的 `figures/` 目录中。
2.  将**导航链接卡片**相关的图片上传到内容仓库的 `Linkfigures/` 目录中。
3.  在文章 Markdown 文件中引用图片 (路径相对于仓库根目录，`main.js`/`post.js` 会处理)