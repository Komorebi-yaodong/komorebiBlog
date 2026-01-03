## Komorebi's Blog

[Komorebi&#39;s Blog](https://komorebi.141277.xyz)

> A blog for sharing my thoughts and experiences.
> 当前为**内容仓库**，配合 `https://github.com/Komorebi-yaodong/komorebiBlog-frontend`使用
>
> 原图片存储使用picx存储在 `https://Komorebi-yaodong.github.io/picx-images-hosting`

## 内容管理指南

管理博客内容非常简单，修改 **内容仓库** 中的文件即可，前端网站会自动拉取最新内容。

### 1. 添加/修改作者信息

* 编辑根目录下的 `author.json` 文件。
* 可以配置作者的 `name`, `avatar` (头像图片路径), `description` (简介), 以及 `link` (社交媒体链接等)。
  * 头像图片建议存放于 `Figures/avatars/` 目录，并在 `avatar` 字段中填写相对路径，如 `Figures/avatars/my-avatar.png`。

### 2. 添加/修改文章

1. **创建/编辑 Markdown 文件**:
   * 在内容仓库的 `Posts/` 目录下创建新的 `.md` 文件，或编辑已有的文章。
   * 文章内的图片引用：
     * 可以直接使用图片文件名，如 `my-image.png`。系统会自动尝试从 `Figures/` 目录加载。
     * 可以使用相对路径，如 `./subfolder/my-image.png` (相对于当前文章所在的 `Posts/` 目录下的子目录)。
     * 也可以使用从仓库根目录开始的绝对路径，如 `/Figures/specific/my-image.png`。
     * 或者直接使用完整的图片 URL。
2. **更新文章列表 `list.json`**:
   * 在根目录下的 `list.json` 文件中，为每篇文章添加或修改一个条目。
   * 必填字段:
     * `title`: 文章标题。
     * `time`: 文章发布日期 (格式: `YYYY-MM-DD` 或 `YYYY-MM-DDTHH:mm:ss`，例如 `2023-10-26` 或 `2023-10-26T15:30:00`)。
     * `file`: 指向 Markdown 文件的路径 (相对于仓库根目录，例如 `Posts/my-first-post.md`)。
   * 可选字段:
     * `description`: 文章的简短描述，会显示在文章卡片上。
     * `image`: 文章卡片和文章顶部英雄区域的封面图片。路径规则同文章内图片引用，建议将封面图统一存放于 `Figures/post_covers/` 或类似的子目录中。
     * `tag`: 文章标签数组，例如 `["技术", "生活"]`。
3. **上传相关图片**:
   * 将文章内引用的图片、文章封面图片等上传到内容仓库的 `Figures/` 目录或其子目录中。

### 3. 添加/修改导航链接

1. **更新导航链接列表 `link.json`**:
   * 在根目录下的 `link.json` 文件中，为每个导航链接添加或修改一个条目。
   * 必填字段:
     * `title`: 导航链接的标题。
     * `link`: 实际的 URL 链接。
   * 可选字段:
     * `description`: 链接的简短描述。
     * `image`: 导航卡片的封面图片。建议将此类图片存放于 `Figures/` 目录，并在 `image` 字段中填写相对路径，如 `Figures/website-logo.png`。
     * `tag`: 导航链接的标签数组。
2. **上传相关图片**:
   * 将导航链接卡片相关的图片上传到内容仓库的 `Figures/` (或你选择的其他) 目录中。

### 4. 添加/修改相册集与照片

1. **创建/编辑相册元数据文件 (例如 `Photos/albumName.json`)**:
   * 在内容仓库中，建议在 `Photos/` 目录下为每个相册创建一个 JSON 文件 (例如 `Photos/wallPaper.json`, `Photos/travelPics.json`)。
   * 每个相册的 JSON 文件是一个包含照片对象的数组。每个照片对象可以包含:
     * `image`: 照片的URL或相对路径。**强烈建议将相册内的所有图片存放在 `Figures/albums/albumName/` 这样的子目录结构中** (例如 `Figures/albums/wallpapers/image1.jpg`, `Figures/albums/travel/photo_abc.png`)。在 JSON 中填写相对于仓库根目录的路径。
     * `title`: (可选) 照片标题。
     * `time`: (可选) 照片拍摄或上传日期 (格式: `YYYY-MM-DD`)。
     * `description`: (可选) 照片描述。
2. **更新相册列表 `albums.json`**:
   * 在根目录下的 `albums.json` 文件中，为每个相册集添加或修改一个条目。
   * 必填字段:
     * `name`: 相册集的名称 (例如 "壁纸收藏", "旅行足迹")。
     * `path`: 指向该相册对应的照片元数据 JSON 文件的路径 (相对于仓库根目录，例如 `Photos/wallPaper.json`)。
   * 可选字段:
     * `image`: 相册集的封面图片。路径规则同上，建议将相册封面图存放于 `Figures/album_covers/` 目录，并在 `image` 字段中填写相对路径，如 `Figures/album_covers/wallpaper-collection.jpg`。
3. **上传照片及相册封面**:
   * 将相册内的所有照片上传到 `Figures/albums/` 下对应的子目录中。
   * 将相册集的封面图片上传到 `Figures/album_covers/` (或你选择的其他) 目录中。

### 5. 配置动态背景图片

1. **更新背景图片列表 `background.json`**:
   * 在根目录下的 `background.json` 文件中，维护一个图片路径列表。
   * 路径可以是完整的 URL，也可以是相对于仓库根目录的路径 (例如 `Figures/backgrounds/bg1.jpg`)。
2. **上传背景图片**:
   * 将用作动态背景的图片上传到 `Figures/backgrounds/` (或你选择的其他) 目录中。

### 提交流程

完成上述任何内容的修改后，将更改提交 (commit) 并推送 (push) 到你的内容仓库的 `main` (或主) 分支。前端网站配置了自动拉取后，会在短时间内显示更新。

---

**目录结构建议 (示例):**

```
.
├── README.md
├── list.json         // 文章列表
├── link.json         // 导航链接列表
├── author.json       // 作者信息
├── albums.json       // 相册集列表
├── background.json   // 动态背景图片列表
├── Posts/            // 存放 .md 文章文件
│   ├── my-first-post.md
│   └── another-topic.md
├── Figures/          // 存放所有图片资源
│   ├── avatars/      // 作者头像
│   │   └── my-avatar.png
│   ├── post_covers/  // 文章封面图
│   │   └── cover1.jpg
│   ├── link_figures/ // 导航链接卡片图
│   │   └── website-logo.png
│   ├── album_covers/ // 相册集封面图
│   │   └── wallpaper-collection.jpg
│   ├── albums/       // 相册内图片 (按相册分子目录)
│   │   ├── wallpapers/
│   │   │   └── image1.jpg
│   │   │   └── image2.webp
│   │   └── travel/
│   │       └── photo_abc.png
│   ├── backgrounds/  // 动态背景图
│   │   └── bg1.jpg
│   └── some-image-used-in-post.png // 其他文章内嵌图片
├── Photos/      // 存放相册的照片元数据 JSON 文件
│   ├── wallPaper.json
│   └── travelPics.json
└── ... (其他前端代码如果也在此仓库)
```
