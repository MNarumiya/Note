# プロジェクト演習
## 動機
GGOをやりたい

## やりたいこと
- センシングを手軽精度良くするのに,neuron以外の奴を使う
- (反動デバイスの無線化 xor 線を減らす) xor 装着しやすいようにする
- 銃撃の感覚

→入力インターフェースは製品プロモーションになってしまう...ので、出力インターフェースに注目.これを作成する.
→ 銃の反動フィードバックデバイス(出力インターフェース)制作を主にする

## 仕様設計
- 反動デバイスの良化
  - 装着の容易さ
  - 擬似感覚

## 方針決めの基本
1. 「表現したいこと」を明確にする
2. 1の上で,それをコンポーネントごと(表現するために必要なこと)に分解していく.
3. コンポーネントごとに,「最低限達成したいこと」と「それ以外のこと」に切り分ける


## +Ultra センシング
センシング手段としては

- カメラによるもの
- 磁気によるもの
- 曲げセンサによるもの
- 生体信号によるもの

がある.
いずれにせよ、製品を使ったり、OpneCVで行ったりと言う手段になるか？
