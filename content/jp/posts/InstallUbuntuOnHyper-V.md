---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Hyper-VでUbuntu仮想マシンを作成"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2019-09-04T16:45:47+09:00
lastmod: 2019-09-04T16:45:47+09:00
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

Ubuntuをインストールから、使用中に発生した問題に対しての対策などを簡単にメモします。

### Ubuntu のインストール
##### Hyper-V で Ubutun の仮想マシンを作成
- 参考記事：[Hyper-V クイック作成](http://hxn.blog.jp/archives/19035644.html)　
<br/>~~オペレーティングシステムは Ubuntu 18.04 LTS を選択します。~~

[Ubuntu Japanese Team](https://www.ubuntulinux.jp/download/ja-remix)から Ubuntu 18.04.3 LTS をダウンロードして仮想マシンを作成しました。

### Ubuntu 使用

##### ログインできない？
仮想マシンのログイン画面に、正しい username と password を入力しても、ログインに失敗します。
調べると、下記記事に書いたように、「基本セッション」ボタンを押せばログインできました。
- 参考記事：[Hyper-VにUbuntuのインストールでハマった点](https://qiita.com/nekobake/items/d5d607bb39283756e405)

<br/>ついでに、下記コマンドで ssh デーモンの導入し、net-tools もインストールしました。
- sshデーモンの導入

```
    sudo apt install openssh-server
```

- net-tools のインストール

```
    sudo apt install net-tools
```

##### Software Updater でアップデータ失敗
Software Updater でアップデータしましたが、snap のところで失敗し、止まってしまいました。
<br/>Software Updater を閉じた後、apt install で他のアプリのインストールもできなくなりました。
<br/>ちょっと調べて解決できなかった、且つ、インストールした直後で何もやっていないなので、仮想マシンを削除して作り直すことにしました。

補記：[Ubuntu日本語フォーラム](https://forums.ubuntulinux.jp/viewtopic.php?pid=120434)によると、アップデータ失敗を起因したバグを直したようなので、日本語版の「Ubuntu 18.04.3 LTS」ダウンロードして利用するようにしました。

- 自動アップデートを無効化
<br/>画面左上の「Activities」をクリック、softwareと入力、表示された「Software & Updates」をクリックして起動し、「Update」タブをクリック、セキュリティアップデートがあるときを「Download and install automatically」から「Display immediately」へ変更し、表示された「Authentication Required」画面でパスワードを入力後、「Close」ボタンをクリックしてウィンドウを閉じます。
- 参考記事：[Ubuntu 18.04 LTSをインストールした直後に行う設定](https://sicklylife.jp/ubuntu/1804/settings.html#disable_auto_upgr)

##### 日本語キーボードレイアウトを設定
キーボードレイアウトがUSのままだと、全角半角ボタンでIMEオンオフができなかったり、@ の場所が違ったりして使いづらいため、下記方法でキーボードのレイアウトを日本語にします。

```
   sudo dpkg-reconfigure keyboard-configuration 
```

Generic 105-key PC (intl.) → Japanese → Japanese → The default for the keyboard layout → No compose key → Yes と入力し、日本語キーボードレイアウトを設定できます。
<br/>もっと詳細は、下記記事を参照できます。
- 参考記事：[Ubuntu で日本語キーボードレイアウト](https://qiita.com/vochicong/items/6452ac54bde56b0e0bb3)

##### 画面の自動ロックをオフに
画面右上のシステム設定（▼）を開き、「Privacy」メニューを選択します。「Screen lock」をオフにします。
- 参考記事：[画面が自動的にロックされないようにする](https://sicklylife.jp/ubuntu/1804/settings.html#disable_lockscreen)

##### be continue

##### be continue

##### be continue


### Hugo でウェブサイトを作成
別途[Hugo でウェブサイトを作成]()を参照します。
