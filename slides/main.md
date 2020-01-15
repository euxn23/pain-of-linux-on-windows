<!-- classes: title -->

# Pain of Linux on Windows

<div class="bottom">
  <h4>2020/01/15 JDD LT</h4>
</div>

---

<!-- textlint-disable -->

import icon from '../images/euxn23.png';

# Who?

<div class="m-8" />

### Yuta Suzuki

<img src={icon} width={200} />

<div/>

- Engineer @ BDD::Dev1
- Web Frontend ~ Backend
- Cloud Infrastructure

<!-- textlint-enable -->

---

## Windows での Linux 系開発環境への執着

---

- [WindowsにRails開発環境を立てる(Vagrant+rails-dev-box+RubyMine)](http://yutaszk23.hatenadiary.jp/entry/2015/02/28/234455) -- 2015-02-28
- [Windows10のbashをzshで起動するように変更する](https://qiita.com/euxn23/items/ca0425456b5027d2ee0e) -- 2016-07-18
- [https://qiita.com/euxn23/items/fb77435296b131fd0a3a](Bash on Windows + cmderでまともなターミナルを獲得する -- 2016-08-11
- [Windows 上でのUnix 開発環境 2017年版](https://qiita.com/euxn23/items/90d921dc6748187fcec1) -- 2017-12-01

<div/>

## 懲りずにずっとやってる

---

## 歴史

<div/>

- cygwin とかでがんばってた時代(よくわからない)
- VirtualBox でちゃんとした Web 開発環境がまともに動くスペックになってきた時代
- Vagrant が流行り始めた時代
- WSL の Beta が出た時代
- Hyper / Alacritty などが安定してきた時代
- [NEW] WSL2

---

## 歴史

<div/>

- cygwin とかでがんばってた時代(よくわからない)
- __VirtualBox でちゃんとした Web 開発環境がまともに動くスペックになってきた時代__
- __Vagrant が流行り始めた時代__
- WSL の Beta が出た時代
- Hyper / Alacritty などが安定してきた時代
- [NEW] WSL2

---

## Vagrant + VirtualBox で仮想環境で開発！ Windows でもできるぞ！ -> 快適とは言っていない

- vagrant を起動するターミナル/シェルが……
  - nyaos, mintty, msys2, ConEmu(cmder), そして nyagos の登場……
  - 結局 cmder + nyagos とか msys2 とかが及第点だった気がする
- CentOS6 とか Ubuntu14 とかなので降ってくるパッケージが色々ツライ(gcc や tmux や open-ssh)
  - 降ってくるgcc のバージョンが古くてnode-sass ビルド落ちる問題
  - tmux.conf の breaking change がひどかった時期
- Windows とのファイル共有に問題が
  - 同期が遅い、差分ビルドが走らないなど
  - Windows の最大パス長を超える node_modules の深さ

---

## 歴史

<div/>

- cygwin とかでがんばってた時代(よくわからない)
- VirtualBox でちゃんとした Web 開発環境がまともに動くスペックになってきた時代
- Vagrant が流行り始めた時代
- __WSL の Beta が出た時代__
- __Hyper / Alacritty などが安定してきた時代__
- [NEW] WSL2

---

## WSL の Beta が出た時代 -> 動くぞ、しかし……

- Windows 上で Linux を動かす弊害
  - ファイルシステムが非常に遅く、ビルド等が致命的に遅い
  - bash.exe のように Win 領域にバイナリがあるので、ログインシェルの変更等が一苦労
- systemd が動かない
  - ので、 docker などが動かせない
  - 当時は(今もだが) Docker for WSL が Home Edition 向けに存在しなかったので、実質 docker 無し
- Windows のターミナルエミュレータが……
  - X Server を Windows 側に建てて Linux のターミナルを表示するなど……
  - Hyper や Alacritty で bash.exe / wsl.exe を指定できるようになってからはマシになった(が、日本語等難あり)

---

## 歴史

<div/>

- cygwin とかでがんばってた時代(よくわからない)
- VirtualBox でちゃんとした Web 開発環境がまともに動くスペックになってきた時代
- Vagrant が流行り始めた時代
- WSL の Beta が出た時代
- Hyper / Alacritty などが安定してきた時代
- __[NEW] WSL2__

---

## WSL2: Pure Linux on Windows!

- Hyper-V ベースの別 VM
  - メモリも CPU も共有する(らしい)のでパフォーマンス性にも優れる
  - 当然ファイルシステムも隔離されている
- ネットワークの自動コンフィギュレーション
  - localhost で VM と繋がるなど、ほぼローカルのように振る舞う
  - Windows の X Server で GUI を使うのも簡単
- 相変わらず systemd がない
  - が、 Ubuntu はなぜか service が動くし、 Pure Linux なので色々ハックできる
  - Penguin Linux や Fedora Remix ではこの辺は削除されている

---

## しかし WSL2 では常用には問題がある

- NFS 経由のファイル共有
  - Windows 側の IDE 等で開くと同期遅延が致命的
  - vscode の remote server extension は謎の技術(sshfs?)でリアルタイム編集を可能にしているが、エディタ選択の自由が無くなる
- Distro の選択肢が少ない
  - 公式で Ubuntu / Kali
  - サードパーティで Fedora や Alpine、 自分でビルドすればいくらでもあるが……

---

## Hyper-V で好きな Distro を動かそう！ -> せっかくだし Arch やるか

---

## 本編

<div/>

# Anarchy OS (Arch Linux Base) の GUI を Hyper-V で動かす

---

## ゴール

<div/>

- Anarchy OS を Hyper-V 上で起動する
- GPU ドライバを入れて画面解像度/FPSを変更できるようにする
- キーリマップを適用し日本語入力のオンオフをできるようにする

---

## Anarchy OS をインストール

<div/>

- Anarchy OS のイメージを落としてきて、CUI に沿ってインストールする
- パーティション設定を変にいじると起動しなくなる
- そこをいじらなければ普通にインストールできる

---

## GUI 環境を整える

<div/>

- インストール直後では小さい画面(変更不可)の低 FPS なので、 GPU まわりの設定をする
- 拡張セッションを使うための、 VirtualBox でいうところの Guest Addition は、 Ubuntu/Arch 向けに提供されている microsoft/linux-vm-tools を使う
- ビルドは通るが、解像度が変更できない

---

## なぜ解像度が変更できないか

<div/>

- Hyper-V で解像度変更をする場合は XRDP 経由でログインすることになる(Hyper-V 同梱のクライアント？)
- linux-vm-tools では xrdp サービスのインストールと有効化をしている
- __Arch 向けの xrdp サービスのビルド自体は成功しているが正しく動いていない__

---

## PKGBUILD を修正して yay でビルド

<div/>

- [neutrinolabs/xrdp#1403 0.9.11 broke my xrdp/Hyper-V enhanced session on Arch](https://github.com/neutrinolabs/xrdp/issues/1403) に沿ってビルド
- Arch ではパッケージのソースコードを取得してパッチを当ててビルドインストールする、という作法が主流(メジャーなパッケージマネージャでサポートされている)
- yaourt は死んだらしいので、 yay を使いソースコードを取得 -> 修正 -> ビルド

---

## 解像度変更

<div/>

- 素の Hyper-V からの GUI だと xrandr でディスプレイが見つからないと言われる
- XRDP でログインした状態で XRDP を実行する
- 2倍解像度は大変そうだったので、 Dot by Dot を指定し、 font size を x1.5 にして対応……

---

## キーリマップと日本語

<div/>

- XRPD Client のせいか不明だが SandS がトラップされてうまく動かない
- メタキーの入力がトラップされてうまく動かない
- なので、メタキーを使って日本語のオンオフをするのができない

---

## Failed...

<div/>

- Windows 上の XRDP Client を通しているため、一部のキーがトラップされるのがしんどい
- Hyper-V 以外の XRDP Client の情報が見つからなかったため比較できず
- 日本語入力のオンオフを左右 Shift に割り当てるなどするからこうなる

---

## Conclusion

<div/>

- # Arch Wiki をしらみつぶしに読む覚悟がなければ変なことはしないほうが良い
- # それでも理想の Linux on Windows がしたい！！！
