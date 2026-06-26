# 我的小站（Hugo + PaperMod）

这是一个基于 [Hugo](https://gohugo.io/) 和 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题搭好的博客骨架，已经配置好分类、归档、搜索页和 GitHub Actions 自动部署。拿到手后只需要改几处配置、写自己的内容，推到 GitHub 上就能上线。

## 目录结构速览

```
content/
  posts/                  ← 所有文章放这里（占位文章可以删掉或改成自己的）
  about.md                ← 关于页
  archives.md             ← 归档页（不用改）
  search.md               ← 搜索页（不用改）
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

## 上线前要改的几处

1. **`hugo.toml`**
   - `title` 改成你自己的站名
   - `baseURL` 改成你最终要用的域名，比如 `https://example.com/`
   - `params.homeInfoParams` 里的首页欢迎语换成自己的话
   - `params.socialIcons` 里的 GitHub 链接换成你自己的，不需要的图标可以删掉这个区块

2. **`content/about.md`**
   把"关于"页换成你自己的介绍。

3. **`static/CNAME`**
   把里面的 `your-domain.com` 改成你购买的域名（只写域名，不要加 `https://`）。这个文件会让 GitHub Pages 知道要绑定哪个自定义域名。

4. **DNS**（在你的域名服务商那边配置，不在这个项目里）
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
git add -A
git commit -m "init blog"
git branch -M main
git remote add origin https://github.com/你的用户名/你的仓库名.git
git push -u origin main
```

然后去仓库的 **Settings → Pages**，把 **Source** 设置为 **GitHub Actions**（不是 "Deploy from branch"）。推送后 Actions 会自动构建并发布，几分钟后就能通过你的域名访问。

> 主题是用 `git submodule` 引入的，所以推送/拉取这个仓库时要注意：如果你在别的电脑上重新 clone，记得 `git clone --recurse-submodules`，或者 clone 完后跑一次 `git submodule update --init --recursive`。GitHub Actions 这边已经配置好自动处理，不用管。

## 日常写文章

本地预览（需要本机装 Hugo，装好后跑这条命令，浏览器打开 http://localhost:1313）：

```bash
hugo server -D
```

新建一篇文章：

```bash
hugo new content posts/文章名.md
```

写完把 `draft: true` 改成 `false`（或删掉这一行），`git add -A && git commit -m "xxx" && git push` 就会自动发布。

### 关于"随时随地写文章"

你说不强求方便性，但顺手提一下几种以后可能会用到的方式：

- **直接在 GitHub 网页里改**：在仓库里点开 `content/posts/`，点 "+" 新建文件，网页自带 Markdown 编辑器，手机浏览器也能用，写完直接 commit，自动触发部署。
- **GitHub Codespaces / GitHub 手机 App**：等于在云端开一个完整的开发环境，不挑设备。
- **本地装 Hugo + 任意 Markdown 编辑器**：最顺手，适合长文章和需要本地预览的场景。

三种方式本质上都是"改 Markdown 文件 + git push"，挑哪个纯看你当下用什么设备方便。

## 图片

把图片放进 `static/images/`，文章里这样引用：

```markdown
![描述](/images/你的图片.jpg)
```

文章封面图可以在 front matter 里加：

```yaml
cover:
  image: "/images/封面图.jpg"
  alt: "描述"
```
