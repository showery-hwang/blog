---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Academic Kickstart で Hugo ウェブサイトを作成"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2019-09-03T16:57:59+09:00
lastmod: 2019-09-03T16:57:59+09:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### Netlify で Academic をインストール
下記記事通りにNetlify で Academic をインストールします。
- [Install Academic with Netlify](https://app.netlify.com/start/deploy?repository=https://github.com/sourcethemes/academic-kickstart)

これで GitHub で academic-kickstart が作成され、 https://yourSiteName.netlify.com/ で公開されます。

### プロジェクトをローカルにクローン
下記コマンドで GitHub のプロジェクトをローカルにクローンします。

```
    $ git clone https://github.com/GitHubUsername/academic-kickstart.git My_Website
```

下記コマンドで ``` theme ``` を初期化します。

```
    & cd My_Website
    & git submodule update --init --recursive
```

### ローカルでHugoを起動

コマンド ``` hugo server ``` を発行すると、いきなり以下のエラーが出ました。

```
error: failed to transform resource: TOCSS: failed to transform "main_parsed.scss" (text/x-scss): this feature is not available in your current Hugo version, see https://goo.gl/YMrWcn for more information
```

原因は、``` Hugo Extended ``` が必要なので、下記コマンドでインストールします。

```
    $ wget https://github.com/gohugoio/hugo/releases/download/v0.57.2/hugo_extended_0.57.2_Linux-64bit.deb
    $ sudo dpkg -i hugo_extended_0.57.2_Linux-64bit.deb
    $ sudo rm hugo_extended_0.57.2_Linux-64bit.deb
```

### 自分のコンテンツでサイトを書き換え

##### サイトの基本設定
- [Get Started | Academic](https://sourcethemes.com/academic/docs/get-started/)

##### 自分のコンテンツを管理
- [Managing content](https://sourcethemes.com/academic/docs/managing-content/)

- 
- to be continue

    
