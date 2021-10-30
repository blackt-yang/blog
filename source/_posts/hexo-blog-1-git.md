---
title: Hexo系列（一）Hexo+Vercel搭建博客
date: 2021-10-30 21:04:16
tags: 
  - Hexo
  - Git
  - Github
  - 博客
  - Vercel
categories: Hexo博客系列
cover: https://i.loli.net/2021/10/30/5v6mnGqKVlHi9Cy.jpg
top_img: https://i.loli.net/2021/10/30/5v6mnGqKVlHi9Cy.jpg
---

**Hexo** 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

**Vercel**是一个静态网页托管平台（当然他也在发展相关的其他业务），提供**自动**构建、部署、托管服务，而且国内访问较快，提供**https**

{%note success%}使用**Hexo**和**Vercel**可以零成本创建个人或组织的博客。{%endnote%}

{%note info%}**[ThrawnのBlog](https://blog.admiralthrawn.me)**就是用**Hexo框架**+**Vercel托管**，使用**Github代码仓库**{%endnote%}

{%note info%}📚**[Hexo系列文章](https://blog.admiralthrawn.me/categories/Hexo博客系列/)**将会涉及**Hexo安装**/**Vercel托管**/**Hexo主题（以Butterfly为例）**/**PWA**/**SEO**等内容，配合**Hexo官方文档**和**相关主题官方文档**食用更香！💡{%endnote%}

## Hexo安装前提

需要：

- [x] Node.js
- [x] Git

{%note info%} **官方文档建议：**Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本 {%endnote%}

## Git与Node.js安装参考 💡

<i class="fab fa-git-alt"></i> [Git官方文档](https://git-scm.com/doc)

<i class="fab fa-node-js"></i> [Node.js官方文档](https://nodejs.org/en/download/)

## NPM安装Hexo 📦

如果已经安装了**Git**和**Node.js**，即可开始安装Hexo

{%tabs tab, 1%}

<!--tab <i class="fab fa-npm"></i>全局安装-->

全局安装**Hexo**

```bash
npm install -g hexo-cli
```

安装完成！🎊

<!--endtab-->

<!--tab <i class="fab fa-npm"></i>局部安装-->

对于熟悉npm的用户，可以直接局部安装

```bash
npm install hexo
```

然后将`node_modules`里面的**hexo**路径添加到环境变量中，即可使用`hexo`命令

**示例：**

```bash
echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.bashrc
```

{%note info%}此示例用于Linux，其他平台自行参考相应方法{%endnote%}

<!--endtab-->

{%endtabs%}

## 创建Hexo博客

`hexo`已经安装完成，现在使用`hexo init`命令**创建博客模板**:

```bash
hexo init blog
```

(`blog`为你的网站名称)

## 安装依赖

建议使用`npm`安装依赖，我之前用`yarn` ，然后出现了莫名其妙的问题

```bash
cd blog
npm install
```

## 目录介绍

**_config.yml**:

**Hexo**的全局配置文件，里面涉及了网站名称、url、作者、部署方案的配置

{%note info%}可以被**主题的配置文件**覆盖{%endnote%}

**source/**:

资源文件夹，里面存放文章的Markdown文件、图片、图标等静态文件

**themes/**:

主题文件夹，存放Hexo主题

## 修改网站基本信息配置

修改`_config.yml`为你自己的自定义配置

```yaml
# Site
title: ThrawnのBlog
subtitle: ''
description: 'Thrawn的博客'
keywords:
author: Mitth'raw'nuruodo
language: 
  - zh-CN
  - zh-TW
  - en
timezone: ''

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://blog.admiralthrawn.me
permalink: :year/:month/:day/:title/
```

{%note info%}请自行修改其中的内容！{%endnote%}

## 定制&创作

### 定制你的Hexo博客

Hexo博客具有很高的自由度，可以通过修改主题文件、安装插件等定制博客，后续会有单独一篇文章细嗦。

### 创作

**创建文章**

```bash
hexo new article
```

`article`替换为文章名称

**创建页面**

```bash
hexo new page xxx
```

`xxx`替换为页面名称

创建的**文章**在`source/_posts`下，是一个Markdown文件

**页面**在`source/xxx`下有一个`index.md`

都可以进行编辑和创作

### 预览效果

`hexo`提供了本地预览的命令

```bash
hexo s
```

执行后访问`localhost:4000`即可看到效果

## 部署网站

{%note info%}网上有很多推荐直接把**编译完成**的网站静态资源推送到**Github Pages**的方案，但我这里使用**Vercel**进行托管，我推荐使用**Vercel**提供的**构建+部署+托管**功能，充分利用其服务 {%endnote%}

{%note warning%}这一步先不要**添加主题**，可能会出错，后续会有一篇文章讲**主题管理**{%endnote%}

### 代码仓库

**Vercel**支持的代码仓库有**Github**、**Gitlab**、**Bitbucket**

{%note info%}这里以**Github为例** {%endnote%}

{%note info%}适合熟悉Git、Github操作的用户，需要先在本地配置git的邮箱、用户名，以及**Github相关的密钥**，才可以进行以下步骤，不熟悉的用户请自行搜索直接本地构建的方案 {%endnote%}

**创建一个空的Github仓库**

如： `admiral-thrawn/blog`

**本地项目Git初始化**

```bash
git init
```

**将本地项目与远程仓库关联**

```bash
git remote add origin git@github.com:admiral-thrawn/blog.git
```

{%note info%}这个仓库是**真实存在的**，注意替换成**你自己的仓库**{%endnote%}

**推送**

提交所有commit，并推送到远程仓库

```bash
git add .
git commit -m "init commnit"
git push -u origin master
```

{%note info%}因为仓库是空的，所以加上`-m`参数，以后每一次推送都不用加{%endnote%}

### 构建&部署

前往[Vercel](https://vercel.com)，注册一个帐号。

{%note warning%}注意不要把Github的主邮箱设置为**QQ邮箱**，我之前用QQ邮箱注册后就被Vercel封号{%endnote%}

接下去可能会让你关联Github帐号，按照他的指示做就行了

**创建项目**

在Vercel的[控制台](https://vercel.com/dashboard)，点击`New Project`，进入创建页面

![Screenshot_20211030_224717.png](https://i.loli.net/2021/10/30/qS1KbVM5Pl7ogCm.png)

在左侧的面板里面点击你的代码仓库，**导入到Vercel**

![Screenshot_20211030_225132](https://i.loli.net/2021/10/30/iIM5ADPBFmlHwct.png)

然后他会让你创建一个**Team**，这个要钱的！，咱们穷逼直接点**Skip跳过**！

下面是构建配置，可以配置你的项目构建脚本

目前直接用默认的就够用，直接Deploy部署

{%note info%}后面的文章讲到**Gulp压缩**的时候，会修改构建脚本，现在不用{%endnote%}

然后Vercel就会从你的Github仓库拉取网站的代码，执行构建脚本进行构建，部署的过程也是自动运行的，所以后面我们就不用管了。

部署成功后，Vercel会引导你去部署好的网页。

{%note success%}这样就零成本部署好博客啦！！{%endnote%}

## 后续

- [x] 后面几篇文章会继续讲主题配置和插件配置
- [x] Vercel支持配置自定义域名，以及很多有趣的功能，大家自行探索！

**May the force be with you, always !**







