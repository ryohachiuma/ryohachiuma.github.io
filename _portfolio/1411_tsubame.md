---
title: "地球・天体観測衛星TSUBAME"
excerpt: "C&DH系組込SW開発，衛星自動運用システム，プロジェクトマネージャを担当<br/><img src='https://www.titech.ac.jp/news/img/n000534_tsubame_fig2s.jpg' width='500'>"
collection: portfolio
---

## TSUBAME
TSUBAMEは東工大松永研究室・河合研究室・理科大木村研究室が開発した地球・天体観測衛星であり、ミッションなどは[大学がまとめてくれた記事](<https://www.titech.ac.jp/news/2014/029067.html>)に詳しい。
今まで一番辛くてエキサイティングだったのがこのTSUBAME開発であり、いろんな意味で成長できた。

### C&DH系
私はC&DH: Command and Data Handling系（衛星の頭脳のようなもの）のソフトウェア開発を担当。
C&DHの主なタスクは下記などが挙げられる（[参考](https://mars.nasa.gov/mro/mission/spacecraft/parts/command/)）。

- 宇宙機内の全てのデータの管理
- 地球から送信されたコマンドを処理
- 全てのサブシステム、観測機の制御
- センサデータから異常検知・あれば適宜処置

全体統括が仕事なので全てのシステム試験に立ち合いが必要となり、打上げ直前の1ヵ月くらいは最終試験のためほとんど仮眠程度しかできず、自分の限界を底上げする良い経験ができた。

<img src="https://www.titech.ac.jp/news/img/n000471_tsubame_pic5.jpg">

### 打上げ
2014年11月6日16:35(JST)にロシアのヤスネ基地から打上げられました。最初の通信が成功した時は感涙。
<iframe src="https://www.youtube.com/embed/iM5j2MnLWpg" frameborder="0" allowfullscreen></iframe>

### 衛星自動運用アプリケーション開発
人工衛星は打上げて終わりではなく、その後に運用する必要があり、これが楽しいが辛い。
TSUBAMEは[太陽同期軌道](https://ja.wikipedia.org/wiki/%E5%A4%AA%E9%99%BD%E5%90%8C%E6%9C%9F%E8%BB%8C%E9%81%93)に投入されたので1日に4回：朝9時 + 約1時間半後、夜9時 + 約1時間半後に10分程度ある通信するチャンスで毎日運用する必要があり、とてつもなくしんどいので衛星運用を自動化するアプリケーションを開発した。

制御アプリケーションのUIをC# + Web (HTML, CSS, jQuery, MySQLなど)、バックエンドをC++で組んだ。
いろんなスキルを習得できて楽しかった。

（自動運用SWの画像探し中）
