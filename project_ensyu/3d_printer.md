# 3Dプリンターメモ
## 概要
North Star作るために使い始めた
基本的にForm2を用いる.
Form2ではサイズが小さいものに関してはZortrax M300を使う.
また,アクリルガラスなど切削が必要なものはKitMillを用いる.


## Zortrax M300について
300×300×300のものまでこれで作れる

- ソフトウェア: [Z-SUITE 2](https://support.zortrax.com/downloads/software/?os=windows)


## Fusion360でKitMill
Fusion360を用いてKitMillで扱えるようにする.

### 大雑把な手順
1. セットアップ ...CAMデータにする.(Fusion360の３DデータからKitMillで扱えるようなデータ型にする)
2. 素材のセットアップ
3. ツールパスを作成する



### セットアップ(CAMデータにする)
Fusion360の３DデータからKitMillで扱えるようなデータ型にする.

#### 実際の作業手順
1. 「作業スペース」を「CAM」に変更する
2. 上部の作業パネルから「セットアップ」クリック


### 素材のセットアップ
まず,使用する加工機器について知ってから,次に原点センサーセット.

#### 使用するCNC
KitMill SR200/420(白いやつ),もしくは,[KitMill AST200](https://www.originalmind.co.jp/products/kitmill_ast) (黒いやつ)

#### Fusion360を使う上で必要なオプション
- 原点センサーセット
- セットスクリュー式スピンドル シャンク径Φ4

> Fusion360のCAMを使う以前に、
- 自分はどんな加工をしたいのか
- 使用する（したい）CNCの性能、仕様
Fusion360のCAMでは修正するパラメーターがとても多いです。
適切なパラメーターを選べるように、素材、機械について知っておくことが とても大切です。
ぜひこの機会に確認しておきましょう。

### ツールパスを作成する
工具
最大荒取り切り込み
退避方法

#### 参考
- [Makke!のblog](http://makke.blog.jp/archives/cat_308646.html?p=2)
- [初めての方でも分かる Fusion 360 CAMの使い方](https://fusion360.3dworks.co.jp/tutorial/fusion-360-cam/)
