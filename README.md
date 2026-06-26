# 我的小站（Hugo + Blowfish）

这是基于 [Hugo](https://gohugo.io/) 和 [Blowfish](https://blowfish.page/) 主题搭的博客骨架。相比上一版的 PaperMod，Blowfish 视觉效果更丰富：卡片式列表、首页大图背景、暗黑模式切换、内置搜索弹窗、图片画廊、时间线等都是自带功能，不用额外开发。已经配置好分类结构和 GitHub Actions 自动部署，改几处配置、写自己的内容，推到 GitHub 就能上线。

## 目录结构速览

```
content/
  _index.md              ← 首页头像下方的简介文字
  posts/                  ← 所有文章放这里（占位文章可以删掉或改成自己的）
  about.md                ← 关于页
static/
  images/                 ← 图片放这里，文中用 /images/xxx.jpg 引用
  CNAME                   ← 自定义域名配置（见下文）
hugo.toml                 ← 站点主配置（标题、菜单、主题参数）
.github/workflows/deploy.yml  ← 自动构建部署的 GitHub Actions 配置
```

预置了 4 个分类，对应你说的内容方向，已经各放了一篇占位文章，写明了建议的结构，照着改/删都行：

- **自建实验室** —— Wiki / Nextcloud / OpenWebUI 等折腾记录
- **生活随笔与摄影** —— 日常感受 + 照片
- **读书笔记**
- **学习总结**

分类不是固定的，想加新的分类直接在文章 front matter 的 `categories: [...]` 里写新名字即可，Hugo 会自动生成对应的分类页。

搜索和暗黑模式切换都已经内置在页面右上角的图标里，不需要单独的页面或配置。

## 上线前要改的几处

1. **`hugo.toml`**
   - `title` 改成你自己的站名
   - `baseURL` 改成你最终要用的域名，比如 `https://example.com/`
   - `params.author` 区块：把 `name`、`headline`、`bio` 换成你自己的介绍，`links` 里的 GitHub 链接换成你自己的（这部分会显示在首页头像旁边）

2. **`content/_index.md`**
   首页头像下方的一段简介文字，换成你想说的话。

3. **`content/about.md`**
   把"关于"页换成你自己的介绍。

4. **`static/CNAME`**
   把里面的 `your-domain.com` 改成你购买的域名（只写域名，不要加 `https://`）。这个文件会让 GitHub Pages 知道要绑定哪个自定义域名。

5. **DNS**（在你的域名服务商那边配置，不在这个项目里）
   - 顶级域名 → 4 条 A 记录指向：
     ```
     185.199.108.153
     185.199.109.153
     185.199.110.153
     185.199.111.153
     ```
   - 或者用 `www` 子域名 → 一条 CNAME 记录指向 `你的GitHub用户名.github.io`

## 推到 GitHub 并上线

```bash
# 在这个目录下
git init
git add -A
git commit -m "init blog"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

然后去仓库的 **Settings → Pages**，把 **Source** 设置为 **GitHub Actions**（不是 "Deploy from branch"）。推送后 Actions 会自动构建并发布，几分钟后就能通过你的域名访问。

> 主题文件直接放在 `themes/blowfish/` 下，不是 git submodule，clone/解压后就是完整的，不用额外处理。

## 日常写文章

本地预览（需要本机装 Hugo extended 版，装好后跑这条命令，浏览器打开 http://localhost:1313）：

```bash
hugo server -D
```

新建一篇文章：

```bash
hugo new content posts/文章名.md
```

写完把 `draft: true` 改成 `false`（或删掉这一行），`git add -A && git commit -m "xxx" && git push` 就会自动发布。

### 关于"随时随地写文章"

几种以后可能会用到的方式：

- **直接在 GitHub 网页里改**：在仓库里点开 `content/posts/`，点 "+" 新建文件，网页自带 Markdown 编辑器，手机浏览器也能用，写完直接 commit，自动触发部署。
- **GitHub Codespaces / GitHub 手机 App**：等于在云端开一个完整的开发环境，不挑设备。
- **本地装 Hugo + 任意 Markdown 编辑器**：最顺手，适合长文章和需要本地预览的场景。

三种方式本质上都是"改 Markdown 文件 + git push"，挑哪个纯看你当下用什么设备方便。

## 图片

把图片放进 `static/images/`，文章里这样引用：

```markdown
![描述](/images/你的图片.jpg)
```

文章封面图（会显示成文章顶部的大图）在 front matter 里加一行：

```yaml
featureimage: "/images/封面图.jpg"
```

## 想要更丰富的排版？Blowfish 自带的内容组件

正文里可以直接用这些 shortcode，不用额外配置：

**图片画廊**（适合一次放多张照片）：
```markdown
{{</* gallery */>}}
{{</* figure src="/images/a.jpg" */>}}
{{</* figure src="/images/b.jpg" */>}}
{{</* /gallery */>}}
```

**提示框**（比如踩坑记录里强调注意事项）：
```markdown
{{</* alert "提示文字" */>}}
```

**时间线**（适合写折腾过程的步骤记录）：
```markdown
{{</* timeline */>}}
{{</* timelineItem header="第一步" */>}}内容{{</* /timelineItem */>}}
{{</* timelineItem header="第二步" */>}}内容{{</* /timelineItem */>}}
{{</* /timeline */>}}
```

> 注意：实际使用时把 `{{</*` 和 `*/>}}` 换成 Hugo 的 shortcode 标记 `{{<` 和 `>}}`（这里多写了 `/* */` 是为了避免被渲染掉）。

## 换配色

`hugo.toml` 里的 `colorScheme` 可以换成下面任意一个，效果差异还挺大，可以都试试：

`autumn` / `avocado` / `bloody` / `blowfish`（默认） / `congo` / `fire` / `forest` / `marvel` / `neon` / `noir` / `ocean` / `princess` / `slate` / `terminal`

## 如果还是想换回简约风格

PaperMod 那一版我还留着，如果用一段时间觉得 Blowfish 太花了，找我说一声，我再给你搭一份就行。
