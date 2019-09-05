---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Hyper-V 仮想マシンの ゲストUbuntu から ホストWindows の共有フォルダをマウントして利用"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2019-09-04T16:38:59+09:00
lastmod: 2019-09-04T16:38:59+09:00
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

Windows10 の Hyper-V で Ubutun の仮想マシンを作りましたが、ホストとゲスト間のファイル共有に悩ませました。
<br/>下記手順でホストの共有フォルダをゲストにマウントし、簡単に利用できます。

### [Hyper-V で Ubutun の仮想マシンを作成](http://hxn.blog.jp/archives/19049511.html)

### ホスト(Windows10)側
Ubutunからアクセスしたいフォルダを共有フォルダにします。

### ゲスト(Ubutun)側
##### SMBクライアントのcifs-utilsをインストール
下記コマンドでパッケージcifs-utilsをインストールします。

```
    $ sudo apt install cifs-utils
```

- 参考記事：[Ubuntu 18.04: SMBクライアントのcifs-utilsをインストールする](https://www.hiroom2.com/2018/05/04/ubuntu-1804-cifs-utils-ja/)

##### マウント先のディレクトリを作成

```
    $ mkdir ~/windows
```

##### 共有フォルダをマウント

```
    $ sudo mount -t cifs -o username=[username],password=[password] //[server]/[share] ~/windows
```

コマンド例：

```
    $ sudo mount -t cifs -o username=showery,password=mypassword //192.168.0.1/shared ~/windows
```

##### 起動時に自動的にマウントするため

``` ~/.bash_profile ```ファイルを下記マウントコマンドを追加します。

``` 
    mount -t cifs -o username=[username],password=[password] //[server]/[share] ~/windows
```

- 参考記事：[Hyper-Vを利用したlinux windows間のフォルダ共有](https://qiita.com/bluelief/items/6188aeb9ac514f0023c5)
