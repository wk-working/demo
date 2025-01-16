---
title: 9 条进阶命令，把 HomeBrew 打造成管理第三方应用的 App Store - 少数派
date: '2024-12-24 10:33:21'
head: []
outline: deep
sidebar: false
prev: false
next: false
---



---

* 9 条进阶命令，把 HomeBrew 打造成管理第三方应用的 App Store - 少数派 - 少数派 - 高品质数字消费指南
* [https://sspai.com/post/43451](https://sspai.com/post/43451)
* HomeBrew 不仅能下载应用，还能搜索、更新、清理缓存或者访问应用官网，用好它，你可以在一个地方集中管理第三方应用。
* 2024-12-24 10:33:21

---

　　使用 Mac 的读者可能都听闻过 HomeBrew，这是一个简单易用的 [包管理器](https://sspai.com/link?target=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E8%25BD%25AF%25E4%25BB%25B6%25E5%258C%2585%25E7%25AE%25A1%25E7%2590%2586%25E7%25B3%25BB%25E7%25BB%259F)，可以让你轻松下载、管理第三方应用。

　　可惜的是，我们读到的文章往往止步于 `brew install 某某应用[^1]`​（用 HomeBrew 安装应用）这一条命令。其实 HomeBrew 的作用远不只下载，我们多学几条命令，就可以把 HomeBrew 打造成一个​**第三方应用的 App Store**​，集搜索、下载和更新功能为一身，简洁高效。

## 搜索应用

　　就像在 App Store 中搜索应用一样，HomeBrew 也支持搜索，而且它会同时从 GitHub、应用官网等多个源头搜索，很容易找到需要的应用，无广告、速度快。

　　要搜索的话，请在终端输入这串命令：

```undefined
brew search 应用名（一般需英文名）
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/2018-02-28-search-fs8-20241224103321-qvbqr88.png)​

　　我们可以看到 HomeBrew 提供了多种结果，如果只是单个应用名（如 `squirrel`​），你可以用 `brew install squirrel`​ 直接安装[[1]](https://sspai.com/post/43451#fn:1 "see footnote")，一般这类能直接下载安装的都是命令行工具。你还可以看到一类名字前带着 `cask`​ 的应用，它们需要换个命令来安装：

```undefined
brew cask 应用名
```

　　就如其名字所代表的一样，`brew cask`​（木桶）下载下来的是一个个打包好 `.app`​ 文件。

　　若想了解更多关于 cask 的内容，请阅读：

* [再谈 Homebrew Cask 在 macOS 上安装应用的轻松感](https://sspai.com/post/40321)
* [借助 Homebrew Cask，教你快速下载安装 Mac App 新姿势](https://sspai.com/post/32857)

## 更新应用和清理旧版

　　有的应用不会自动更新（或默认不打开），我有个同学的 Chrome 现在就还停留在二十多个大版本之前。其实通过 HomeBrew 的命令，哪些应用需要更新一目了然，即使它们不提供自动更新，我们时不时去检查、更新一下也能保证应用处于最新版。

　　首先用下面的命令检查一下可更新的应用有哪些，由于我比较勤快，只有一个 imagemagick 不是最新版本 🌚。

```undefined
brew outdated
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/2018-02-28-outdated-fs8-20241224103321-s6j5vaa.png)​

　　接下来更新一下可更新的应用。一般我会更新所有应用，所以我最常用的是这条命令：

```undefined
brew upgrade
```

　　但有时我们不想更新所有应用，比如 Chromium 有个历史版本不禁用 Flash，我一直留着它以应对那些食古不化的网站，不希望 Chromium 更新到更高版本。此时我们可以在上面那条命令的基础上加上需要更新的应用名，避开不需要更新的应用：

```undefined
brew upgrade 应用名
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/2018-02-28-upgrade-fs8-20241224103321-qp02udh.png)​

　　更新完后可以运行一下下面的命令，把应用的旧版本和缓存删除。

```undefined
brew cleanup
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/2018-02-28-cleanup-fs8-20241224103321-d3c7p2k.png)​

　　如果你只是想看看有哪些应用可以清理，但暂时不需要处理它们，则可以通过这个命令一窥究竟：

```undefined
brew cleanup -n
```

　　当然，有的应用缓存和旧版应用是有用的（比如可能保存了我的用户配置文件），那就不能一杆子打尽，而是像指定更新个别应用一样，指定需要清理缓存的应用：

```undefined
brew cleanup 应用名
```

　　👀**Tips：访问应用官网**

　　有时我们不确定自己是否需要更新一个应用，比如，它的新功能我是不是需要？它的新版本适不适配我的系统？纠结这些，不如即刻去应用官网上一探究竟：

```undefined
brew home 应用名
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/2018-02-28-HomeBrew-home-20241224103321-mjg7ii1.gif)​

## 小结

　　电脑里的第三方应用越多，HomeBrew 的优势越明显。

　　如果只下载一个应用，可能径自前往其官网也不会觉得麻烦，但如果你每次下载第三方应用就要前往官网、每次更新都得去其菜单栏中寻找 update 按钮，那显然是不便的。HomeBrew 就为这些的零碎的操作提供了一个集中的管理办法。

　　学会了本文的几条命令，对你来说 HomeBrew 就不再是晦涩的命令行工具，而是一个简单好用的第三方应用版 App Store。

　　316

　　18

　　搜索应用

　　更新应用和清理旧版

　　小结
