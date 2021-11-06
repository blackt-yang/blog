---

title: Hexo系列（二）Git Subtree管理Hexo主题
date: 2021-11-06 19:38:16
tags:
  - Git
  - Github
  - Hexo
  - 主题
categories: 
  - [开源生活,Hexo博客系列]
  - [开源生活]
cover: /images/top-img/sunset.jpg
top_img: /images/top-img/sunset.jpg
typora-root-url: ../../source
---

{%note info%}本文只讲**Git**方案，不讲**NPM**{%endnote%}

{%note success%}阅读本文需要上一篇文章的基础{%endnote%}

Hexo博客模板支持博主自定义主题，通常有两种安装主题的方式：

1. 通过npm安装:直接从npm仓库拉取主题包，安装到`node_modules`，和Hexo以及其他插件一起用**npm**管理
2. 通过git拉取Github仓库内的主题代码，下载到`themes`，由**git**管理

第一种**npm**方法的弊端在于：主题代码下载到`node_modules`，和其他插件一起管理，如果有魔改需要，就要修改`node_modules`里的主题代码，改好之后如果`npm install`其他插件，修改就会丢失。

所以我推荐第二种方法，本文只介绍Git方法的使用和管理

## Git安装Hexo主题

首先，找到想要的主题，可以去Hexo的官网看看，也可以逛一逛各大交流社区

我这里推荐几个我比较喜欢的：**Butterfly(就是这个博客的主题)**，**Icarus**，**Fluid**，**NexT**

**安装方法**：

找到主题的**Github仓库**，如`somebody/some-theme`

把他clone到themes目录下：

```bash
git clone -b master git@github.com:somebody/some-theme.git themes/some-theme
```

这里以**Butterfly**为例

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

在Hexo的根目录配置文件里把`theme`改成`butterfly`

`_config.yml`

```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: butterfly
```

运行`hexo s`，在浏览器中打开`localhost:4000`，就可以看到主题效果了！

## 主题配置

把`themes/butterfly`下的`_config.yml`复制到博客根目录下，改名为`_config.butterfly.yml`，把它作为主题的配置文件，然后按照你使用的主题的官方文档做个性化配置。

## 推送到仓库

这时候我们如果直接把修改推送到**Github**，触发**Vercel**构建，会报错，报错信息是说找不到主题文件，这是因为`git push origin master`的时候只把博客的代码推送到Github，`themes`下的是主题代码独立的，所以Github仓库里的主题目录是空的，Vercel构建时Hexo报错。

## 使用Fork+Git Subtree管理

我们的解决方案是用`fork+git subtree`管理主题，这样既能把主题推送到仓库，有能修改主题文件

先`fork`你想要的主题项目到你自己的Github

例如`admiral-thrawn/hexo-theme-butterfly`

## 拉取到本地

先删除原有的`themes/butterfly`文件夹，在把修改推送到仓库。

```bash
git add .
git commit -m "删除主题"
git push origin master
```

{%note info%}这时又会触发Vercel构建，还会报错，不用理会{%endnote%}

然后添加我们fork的仓库

```bash
git remote add -f butterfly git@github.com:admiral-thrawn/hexo-theme-butterfly.git
```

```bash
git subtree add --prefix=themes/butterfly butterfly master --squash
```

{%note info%}`--squash`参数是用来合并commit的{%endnote%}

## 再次推送到仓库

这时再执行`git push origin master`，就可以成功构建了

## 同步更新

我们可能会对主题进行魔改，这时就需要把我们fork的仓库`admiral-thrawn/hexo-theme-butterfly`单独`clone`下来

（使用单独的目录）

```bash
git clone git@github.com:admiral-thrawn/hexo-theme-butterfly.git
```

对里面的代码进行魔改，该完后再推送到仓库

**现在回到Hexo目录下**

获取子项目的更新

```bash
git fetch butterfly master
```

推送到主项目

```bash
git subtree pull --prefix=themes/butterfly butterfly master --squash
git push
```

推送到子项目

```bash
git subtree push --prefix=themes/butterfly butterfly master
```

这样就可以实现主项目和子项目的同步更新和修改，为我们后续魔改主题奠定了基础（我不建议魔改主题，我自己的博客主题也没魔改，因为这样就很难使用官方的更新）

## 获取官方更新

官方有更新时，我们需要先进入子项目下，把官方的仓库pull下来

```bash
git pull git@github.com:jerryc127/hexo-theme-butterfly.git
```

然后用上一节的方法，推送到主项目和子项目即可。

{%note warning%}注意这样更新以后你的子项目就和官方一样了，所有魔改都丢失了{%endnote%}

## 其他

这是我自己搭建博客时遇到了这一问题，在搜索之后得到的解决方案，参考了别人的博客，觉得挺有用的，遂总结和发散了一下，发到自己的博客。

**参考**： [ 通过 Subtree 实现 Hexo 主题自定义配置的同步 ](https://www.iszy.cc/posts/wci14e/)











