# コンテナ型仮想化_メモ
---

##  最新cgroup事情

systemd
k dbus


## Cgroupとは

- Control Group の略
- タスクのグループを作る
- 作ったグループに対して制限・パメータ等を設定する


例えば、LVMノフロントエンドの livirt は各KVM
Cgroup はgoogleのエンジニアがメンテナンスしていた（代替わりはしたけど）




- 各グループはヒエラルキでツリーを作る
- 仮想ファイルシステムインターフェースで実装される（）
-すべてのタスクはいずれかのCcgroupに所属する

---

cpu

cpuacct
cpuset
memory
blkio
device
freezer


- net_class
 - cgoroupから送られてきたキューにDC
- net_pio
- perf
- hugetlb
 
---

- Cgroup のヒエラルキの実装は面倒
 - 自由度高杉

- sane_behavior

3.19 〜 3.20 でdefault化するkまお



---

新ヒエラルキー


cpuset はなくなるかも

systemd がすべてのCgroupを〜

---

## Memory croup

メモリ使用量の上限を設定できる（+swap）
TCPのバッファ量も制限できる


性能的にオーバヘッドがある
race condition


cgroup はタスクがいなくなると削除可能
memoery cgroup 
 ページキャッシュ
 カーネルメモリ
 スワップキャッシュ



1ページ 4096 単位にpage 構造体を（64）を割り当てる。memory cgroupは１ページごとに16byte
1G ごとに 4Mバイト

---

ホットパス？

ページフォルト


---








## LXCの本番運用事例 (仮) 

---

## Dockerfile徹底入門

中止

---

## CoreOSによるDockerコンテナのクラスタリング (仮)

---

## コンテナ？それfreebsdでもできるよ！ (仮)

---

## Solaris のコンテナ技術「Solaris Zones」

---

## LXC関係で何か (仮)

---
