---
marp: true
paginate: true
theme: marp
---

# APNIC56 を爆速で振り返る

## 慶應義塾大学 大谷亘 a.k.a. alt

2023/09/16 JSNOG-LT-2

---

![](images/230916-apnic-web.png)

<p style="text-align: center;">OPM の内容に絞ってご紹介します</p>

---

# [prop-148](https://www.apnic.net/community/policy/proposals/prop-148/)

## Leasing of Resources is not Acceptable

現在のポリシでは IP アドレスの割当に直接接続を条件としているが，リースは明示的に禁止されていない．
提案者・APNIC ともに「リースは許容されるものでない」というスタンスだが，リース事業者は実在し，積極的に対応できていない．
リース禁止を明文化しようという提案．

---

# [prop-148](https://www.apnic.net/community/policy/proposals/prop-148/)

## Leasing of Resources is not Acceptable

(議論を聞きそびれたのであとで追記)

**コンセンサスに至らず**: 新規提案として作り直すべきか?

---

# [prop-152](https://www.apnic.net/community/policy/proposals/prop-152/)

## Reduce the IPv4 delegation from /23 to /24

現在のポリシでは現行のアドレス在庫が枯渇した後に既存メンバには分配しないことになっている．
提案者は IPv6 実装のためにも IPv4 アドレスは必要だと考えている．
枯渇後は最大割当サイズを /24 に縮小し継続して再分配しようという提案．

---

# [prop-152](https://www.apnic.net/community/policy/proposals/prop-152/)

## Reduce the IPv4 delegation from /23 to /24

- もう IPv4 は死んだ．ポリシーは不要．
- 新規参入者の障壁になる

**コンセンサスに至らず**: メーリスでの議論へ

---

# [prop-153](https://www.apnic.net/community/policy/proposals/prop-153/)

## Proposed changes to PDP

現行の PDP では提案者が提案を Policy SIG チェアに提出後，Policy SIG チェアは OPM 4 週間前までにメーリスに投稿することになっているが，提案者がいつまでに Policy SIG チェアに提出するか期限が定まっていない．
この期限を OPM 5 週間前までと明示する提案．

---

# [prop-153](https://www.apnic.net/community/policy/proposals/prop-153/)

## Proposed changes to PDP

- もっと短くて良いのでは?
  - 日本などでは翻訳作業などに時間がかかる
  - JPOPF「タイムラインが決まっていれば間に合わせられる」

**コンセンサスに至らず**: 期間の見直しなどを検討

---

# [prop-154](https://www.apnic.net/community/policy/proposals/prop-154/)

## Resizing of IPv4 assignment for the IXPs

現行では IXP に /24 の割当を行っているが，事業規模によっては持て余すことがあるため，デフォルトの割当サイズを /26 に縮小する提案．
本提案では，ピアリング数に応じて /24 まで割当可，さらに利用率に応じて /22 までリナンバリングによって対応することとしている．

---

# [prop-154](https://www.apnic.net/community/policy/proposals/prop-154/)

## Resizing of IPv4 assignment for the IXPs

- もう IPv4 は死んだ
  - それでも IX にはまだ IPv4 がそれなりに必要なこともある
- それより IPv6 の議論にリソースを割くべきだ
- リナンバリングは大変な作業
- IX に必要サイズを計算・提出させて，最初から適切にサイジングするべきでは?
- そもそも IX は CGNAT のアドレスを使ったらいいのでは?
  - Global routable address という unique なアドレスが必要
- /26 しかもらえなくても /24 分の金を払うのか?
  - EC に料金を見直してもらう必要がある

**コンセンサスに至らず**: メーリスでの議論へ

---

# [prop-155](https://www.apnic.net/community/policy/proposals/prop-155/)

## IPv6 PI assignment for associate members

IPv4/IPv6 のどちらのアドレスも持たないアソシエイトメンバに対し，/48 の IPv6 アドレス割当を認める提案．

---

# [prop-155](https://www.apnic.net/community/policy/proposals/prop-155/)

## IPv6 PI assignment for associate members

- 特筆すべきコメント・異論は挙げられず

**コンセンサス**: PDP の次段階へ

---

# 参考

- [APNIC 56 での IP アドレス・AS 番号分配ポリシーに関する提案のご紹介 - JPNIC Blog](https://blog.nic.ad.jp/2023/9196/)
- [APNIC 56 に向けた意見交換ミーティング - JPOPF 運営チーム](https://www.jpopf.net/20230830announce?action=AttachFile&do=get&target=20230830-APNIC56%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E6%8F%90%E6%A1%88%E6%84%8F%E8%A6%8B%E4%BA%A4%E6%8F%9B%E8%B3%87%E6%96%99.pdf)

# その他

**本カンファレンスには [JPNIC 国際会議参加支援プログラム](https://www.nic.ad.jp/ja/intl/fellowship-program/index.html)による支援を受けて参加しました**
