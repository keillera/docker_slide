# Docker を使ってみた！
# LT風 - 2014/09/18

---

# 今日の勝敗

---

# リッキーが眠くなったら俺の負け

---


# Docker ってなあに？

Docker は Web アプリケーション等を仮想化するための技術の１つです。Linux KVM や VirtualBox といった「ハイパーバイザ型の仮想化」に対して、「コンテナ型の仮想化」とも呼ばれています。



![inline](./docker.png)

---

#  ( ´・∀・)ﾉ ハーィ
# さっそく質問いいですか

---

## Q. Docker とハイパーバイザ型って何が違うの？

---

## A. ハイパーバイザの代わりにDocker Engineを使用してます

---

## 図で見るとこんな感じ
![inline](./comp.png)

---

## ゲストOS のエミュレートは不要！
![inline](./comp_2.png)

---

#  ( ´・∀・)ﾉ ハーィ
# エミュレートが不要だと何がうれしいんですか？

---

## うれしいこと - その１

1. ハイパーバイザ型の場合はゲストOSをエミュレートして、その上でプロセスが動いているのに対し、Docker Engine は各プロセスをグループで隔離しているだけで、**プロセス自体は直接ホストOS上で動かしている**ためオーバーヘッドが小さい。ようするに少ないリソースで仮想化が実現できる（**超大事**）。

![](./docker.png)

---

## うれしいこと - その2

2.オーバーヘッドが少ない分、ハイパーバイザ型と比べて起動が比較にならないほど速い！処理も速い！もう体感では物理サーバで動かしているのと変わらないくらい速い！

![](./docker.png)

---

#  ( ´・∀・)ﾉ ハーィ
# その1.について質問です

---

## Q. 少ないリソースで仮想化実現できるのがそんなにうれしいんですか？

---

# A. そんなにうれしいんです！

---


## 単純にサーバに負荷がかからないためということだけでなく、クラウド視点からすると「負荷増=コスト増」となるため、負荷はお金と直結しています。負荷が減ってお金減らせたらうれしいじゃん！

---


![inline](./danbo.jpg)


---


#  ( ´・∀・)ﾉ ハーィ
# その2.について質問です


---

## Q. 体感で速いとかいわれても分からないよ。実際どれくらい速いんですか？見える化してよ

---

## A. 速さを比較している表があったので貼付けてみます
### 参考：軽くて使いやすい仮想化技術 「Docker」の仕組みとエンタープライズ開発における4つの活用事例 (http://codezine.jp/article/detail/7894)


---
##    .
![inline](./comp_bar.png)

---

## 数値で比較しても物理サーバと違いが見られない！ようするに、

![inline](./comp_bar.png)

---

## ハイパーバイザ型は
## 速さが足りない

![inline](./hayasa.jpg)

---

## これはもう仮想化は
# コンテナ型を
## 使うしか無いでしょ

![](./docker.png)

---

## ここで、Docker 以外にもコンテナを実装しているものがあるので幾つかご紹介

- LXC
Linuxのコンテナ化ソフトウェア。Docker 0.8 まではLXCを利用してコンテナ化を行っていたが、Docker 0.9 より libcontainer をベースとしたドライバがビルドインされ、このドライバによってコンテナ化を行うようになった。このため0.9以降では LXCは必須ではなくなった（0.9 以降もLXCドライバを利用してコンテナ化することは可能）。

--- 
- OpenVZ
RHEL(Red Hat Enterprise Linux) 用のコンテナ実装。Kernel に手を入れているため、公式にはRHELのみのサポートとなっている。また、ライブマイグレーションに対応している（この機能は Docker には存在していないのでライブマイグレーションを使いたい場合はOpenVZを利用）
- Warden
元々VMwareがCloud Foundry向けに開発したLinux向けコンテナ技術。現在この Warden を活用し、Dockerのlibcontainerプロジェクトの強化を行っている。


---

## ちなみに

---

## すでにGoogleは全部のソフトウェアをコンテナに乗せており、毎週20億個ものコンテナを起動しているらしいです。

---

## 実績もあるので、コンテナ化は今後もっと使われそうなイメージです。
## また、コンテナ化に関しては今 Docker が勢いがあるため、とりあえずコンテナ化したい場合は Docker を選ぶのが良いかと思います。

---


# ということで、Docker の仕組み
![](./docker.png)


---

# Docker は既存技術を組み合わせているだけです

---

# 主な技術

- Go言語：Docker は Go 言語で記述されています
- Linux Namespace：名前空間によるプロセス、ネットワークの独立化（この技術でコンテナ化を実現）
- cgroups：リソース(CPU、メモリ、ディスクI/Oなど)の制御
- AUFS：データを差分で管理するファイルシステム（Docker Image を差分で管理するために使用）

---


# 補足_1 Linux Namespace

- Linux Namespace は Kernel 3.8 以降で実装された比較的新しい技術です（Ubuntuでいうと Version 12.04 くらい）。このため Docker を使用するためにはホストOS の Kernel が 3.8 以降である必要があります。

---

# 補足_2 AUFS

- AUFS は聞き慣れないファイルシステムかと思いますが、CD Boot で Linuxを起動した際に( KNOPPIX とか)、あたかもファイルを書き換えてるかのように動かせていたのは、この AUFS を利用していたためです。
⇒ 具体的にいうと〜

---

- システム関連のファイルを書き換えようとしても、光学ディスクに書き込まれているため、物理的にファイルの内容を書き換えることができません。そこで、変更があった内容についてはメモリー上に保存しておき、差分として管理しています。

---

## こんな感じ

![inline](./aufs.png)

---

また、Docker がコンテナ化するときは、コピーオンライト（原本をそのまま参照させておき、変更した際に初めて複製する）を行っているため、同一のイメージを複数仮想化させた際に無駄なリソースをとらないで済む

---

## 具体的うれしいこととしては
- 1GBのイメージを 10台立ち上げたとしても必要とするストレージは1GBで良い（ハイパーバイザ型の場合は10GB必要となる）ためストレージを節約できる
- 同様の理由でメモリも無駄なリソースを食わないため節約できる
- メモリ上にキャッシュされているため高速でデプロイできる

---


# 仕組みは以上
# 次はお待ちかね？の利用方法

---

# Docker の利用方法

1. Docker をインストール(apt-get や yum)1. DockerFile を作成1. Docker run コマンドで Docker Image を作成/起動

※ Docker stopで停止。start で再度起動できます。

---

# [fit] 簡単です。

---

# ただ、注意点が

## 繰り返しますが kernel は 3.8 以上である必要があります。よく使われてそうな CentOS を調べてみると・・・


---

# あ、

![inline](./6kei.png)

---

# ６系全滅だ(3.x ですらない・・)
![inline](./6kei.png)

---

## 一応 Kernel version を上げることで使えますが、可能であれば 7 系使いましょう(サポート期間も6系より長い 2020年11月30日 ⇒ 2024年6月30日)。

---

## むしろOS選べるのであれば、Docker に特化した軽量な **CoreOS**  がおすすめです（Docker はインストール済み！）

---

## CoreOS について

##  CoreOS は 2013/8 に α リリースされた、まだ新しい OS ですが 2014/07/25 に安定板がリリースされています。OSS で公開されているため無料で利用可能。

---

## 主要なクラウドサービス（AWS、GCE、Azure 等）にも対応されており、すぐに利用可能です（Dockerに特化しているということもあり今後も使われていくOSかと思います）。

---

# Docker の利用方法にもどります
# (さっきと同じ)

1. Docker をインストール(apt-get や yum)
2. DockerFile を作成
3. Docker run コマンドで Docker Image を作成/起動

※ Docker stopで停止。start で再度起動できます。

---

# DockerFile の書き方



---

# Docker Image の取得方法

- Docker Hub を利用しよう！
- 既に他の人が作成してくれたDocker Image を利用することができます。

---
# 参考文献
- Docker 入門 - Immutable Infrastructure を実現する - (技術評論社)
- アプリ開発者もインフラ管理者も知っておきたいDockerの基礎知識 (http://www.atmarkit.co.jp/ait/articles/1405/16/news032.html)
- DockerをCentOS 7にインストールする方法 (http://blog.yuryu.jp/2014/07/docker-centos7.html)

---

- Dockerが利用しているAUFSとLXC
http://shibayu36.hatenablog.com/entry/2013/12/30/173949
- CentOS で aufs (another unionfs) を使う
http://d.hatena.ne.jp/dayflower/20080714/1216010519
- Docker基礎+docker0.9, 0.10概要
http://www.slideshare.net/mainya/dockerdocker09-010
- コンテナ型仮想化「Docker 0.9」リリース。LXCに依存しなくなり、安定性向上など
http://www.publickey1.jp/blog/14/docker_09lxc.html

---

- Docker 0.9リリースドキュメント日本語訳: Execution driversとlibcontainer導入
http://d.hatena.ne.jp/mainyaa/20140311/p1
- PaaS基盤「Cloud Foundry V2」内部で使われるBuildpack、Wardenコンテナの仕組みとは？
http://www.publickey1.jp/blog/14/paascloud_foundry_v2buildpackwarden_1.html

---

- VMware、Docker、Google、Pivotalと協業し、企業向けコンテナ技術の導入を簡素化
http://www.vmware.com/jp/company/news/releases/vmw-Docker-Google-Pivotal-082514

