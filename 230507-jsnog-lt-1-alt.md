---
marp: true
paginate: true
title: うちの DNS 権威サーバ | JSNOG
theme: marp
image: https://slides.jj1lfc.dev/230507-jsnog-lt-1-alt.jpg
---

# うちの DNS 権威サーバ

## 慶應義塾大学 大谷亘 a.k.a. alt

2023/05/07 JSNOG-LT-1

---

# ホスト中のゾーン

```
zone:
  - domain: jj1lfc.dev
  - domain: oru.to
  - domain: kelo.jp
  - domain: sfckeioac.jp
  - domain: ohgai.dev
  - domain: sotsuron.fail
```

---

# 外から見ると

```
❯ dig NS jj1lfc.dev. @1.1.1.1 +rec

; <<>> DiG 9.10.6 <<>> NS jj1lfc.dev. @1.1.1.1 +rec
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 7423
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; NSID: 32 32 6d 32 35 30 ("22m250")
;; QUESTION SECTION:
;jj1lfc.dev.            IN NS

;; ANSWER SECTION:
jj1lfc.dev.             3600 IN NS nsv.jj1lfc.dev.
jj1lfc.dev.             3600 IN NS pseudo-nsv.jj1lfc.dev.

;; Query time: 57 msec
;; SERVER: 1.1.1.1#53(1.1.1.1)
;; WHEN: Sat May 06 09:36:46 JST 2023
;; MSG SIZE  rcvd: 92
```

---

# 権威サーバ群の構成 (L7)

- hidden master による事故防止
- signer 分離による DNSSEC 事故防止
- DNSSEC 秘密鍵を持つホストを隔離

![bg w:500 right](https://kroki.io/mermaid/svg/eNp1jsEKwkAMRO9-xdB7CvYsHnrrVfoDtabdwpKVZK1-vrsLYsGa28ybDDP58BzdoBF9ewDscZ11uDuI0RgkavCeNQHgK0FUvSatiM6wZZYSYLnl_yK3AbH1-A80u6BOt53SO0YnkVU4liW5EqccvbAFv7Lax2_2_Fz466fBb8vdS4o=)

---

# Signer (knot) の config (抜粋)

```
template:
  - id: default
    master: controller
    notify: [ nsv1, nsv2 ]
    acl: [ acl_controller, acl_nsv1, acl_nsv2 ]
    dnssec-signing: on
    dnssec-policy: nsec3
    semantic-checks: true
    zonefile-load: difference
    serial-policy: unixtime

  - id: no_dnssec
    master: controller
    notify: [ nsv1, nsv2 ]
    acl: [ acl_controller, acl_nsv1, acl_nsv2 ]
    semantic-checks: true
    zonefile-load: difference
    serial-policy: unixtime
```

---

# ロードバランシング (L3)

- Internet-facing な権威サーバにソフトウェアルータを同居
- 同一の VIP を iBGP で広告
- L3 でのロードバランサレスアーキテクチャ

![bg w:500 right](https://kroki.io/mermaid/svg/eNptjj8LwkAMxff7FKFTHW5oZilYESehVLejw_XP2YK0krvq1zfXo4K2yyMvyS955jG-606Tg1smAOxU3Uk_OxjsK2EPXDSJYilnZ4gSxeJdOzSs5LjDUv7RuND4Q-OKxoX2n2AvZeRIG9PXkZTpfH7V1LYiIUKaeZidczgdL3lASMVFsSvDAm4t-M-0DL4nVXy4ZgH01s9TGCdn-6YVISBuBcTtgAAftGhirA==)

<!--
  flowchart TB
    subgraph nsv1
      nsd1[nsd]
      frr1[frr]
    end
    rtr1[rtr]
    subgraph nsv2
      nsd2[nsd]
      frr2[frr]
    end
    rtr2[rtr]
    nsd1 <--"traffic"-\-> rtr1 <--"traffic"--\> asbr

    frr1 <--"BGP ECMP"--\> rr[(RR)]
    frr2 <--"BGP ECMP"--\> rr
    rr <--"BGP"--\> asbr[(ASBR)]
    asbr <--\> outside

    nsd2 <--"traffic"--\> rtr2 <--"traffic"--\> asbr
-->

---

# frr の config (抜粋)

`dummy1` IF に VIP を割当

```
interface dummy1
 ip address 207.148.88.188/32
 ipv6 address 2001:19f0:7002:b07::53/128
exit
```

---

# frr の config (抜粋)

RR と BGP peer を張り VIP を広告

```
router bgp xxxxx
 bgp router-id xxxxx
 ...
 address-family ipv4 unicast
  network 207.148.88.188/32
  ...
 exit-address-family
 !
 address-family ipv6 unicast
  network 2001:19f0:7002:b07::53/128
  ...
 exit-address-family
exit
```

---

# 感想

- 見た目以上に難しくない
- L3/L7 の合わせ技楽しい
- 本当は Anycast したい
- 個人ゾーンの NS にこんな LB はいらない

## 参考資料

[ccTLD 及び ccSLD 権威サーバの運用に関する報告 - WIDE 報告書 2021](https://www.wide.ad.jp/About/report/pdf2021/usb/22_wide-memo-two-lbdns-ops-2021.pdf)
