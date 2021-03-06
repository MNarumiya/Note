# 『リーダブルコード』を読む
### 1章 理解しやすいコード
**鍵となる考え(原則):**
- コードは理解しやすくなければならない

Point

- 優れたコードとは,他の人が最短時間で理解できるようなコードのこと
- 「理解する」= 変更を加えたり,バグを見つけたりできるということ
- コードは短い方がいいが,「理解するまでにかかる時間を短くする方」が大切
- 常に,コードを書くときは「このコードは理解しやすいだろうか?」と自問自答する

## 第Ⅰ部 表面上の改善
- 適切な名前を選ぶ
- 優れたコメントを書く
- 空白の使い方を綺麗にする

### 2章 名前に情報を詰め込む
**鍵となる考え(原則):**
- 名前に情報を詰め込む

Point

- 明確な単語を選ぶ
  - 関数なら何をするのか,他のもっとわかりやすい言い回し方の単語はないかかを考える.例えば,Size()よりも,高さを返すのならHeight()

- 汎用的な名前を避ける.(or使う状況を選ぶ)
  - tmpは,生存期間が短くて,一時的な保管がもっとも大切な役割の時のみに使う.
  - tmpやretval,fooなどの名前を使うときは,それ相応の理由を用意できる時だけ使う
  - ループイテレータを母集団?がわかりやすいように名前をつける

- 抽象的な名前よりも具体的な名前を使う...具体的な名前を用いて詳細に説明する

- 接尾辞や接頭辞を使って情報を追加する(必要な時だけ)
  - 絶対に知らせなければならない大切な情報があるときは,単語を変数名に追加する
  - 変数名に値の単位を付け加える(ex:size_mb)
  - 危険性や注意を歓喜する情報を追加する(ex:untrustedUrl)

- 名前の長さを決める
  - スコープが小さければ,短い名前でも可...全ての情報が前後で見られるから
  - スコープが大きければ,名前に十分な情報を詰め込んで明確にする
  - 名前は単語保管機能を用いて
  - 省略した名称は新たにプロジェクトに入ってくる人がわかるなら使ってOK
  - 不要な単語は投げ捨てる...名前に含まれる単語を削除しても情報が全く損なわれないなら消す(ex, ConverToStringとToString)

- 名前のフォーマットで情報を伝える
  - アンダースコア・ダッシュ・大文字を使って名前に情報を詰め込む
    - クラス名...CamelCase(キャメルケース)
    - 変数名...low_separated(小文字アンダースコア区切り)
    - 定数...kConstantName
    - #defineマクロ...MACRO_NAME
    - クラスのメンバ変数(プロパティ)...propaty_(最後の文字がアンダースコア)
  - その他使う言語によって使えるフォーマット規約はことなってくる
  - **どのようなフォーマットを使うにしても,プロジェクトで一貫性を持たせることが大切**

### 3章 誤解されない名前
**鍵となる考え(原則):**
- 名前が「他の意味と間違えられるとこはないだろうか?」と何度も自問自答する
- 最善の名前とは,誤解されない名前である

Point
- 限界値を含めるときは,名前にmax_やmin_をつける
- 範囲を指定するときは,firstとlastを使う(min,maxでもいい)
- 包合/排他的範囲(最初は含まれてて,最後は含まれてない)はbeginとendを用いる
- ブール値をもつ変数の命名は...
  - true,falseの意味を明確にする変数名にする
  - 頭にis,has,can,shouldを用いるとわかりやすくなる
  - 名前はなるべく肯定形にする(否定形は避ける)

- ユーザーの期待に合わせた命名にする
  - 例:get*()...getメソッドはメンバ変数を返すだけの軽量なメソッドであるのが一般的.それゆえ,getMean()がO(n)などの計算量のかかるメソッドであるなら,従来の感覚とずれることになる.compiteMeanなどにする
- 複数の名前を検討する際も,誤解を生む可能性が一番低いものにする

### 4章 美しさ
**鍵となる考え(原則):**
- 読み手が慣れているパターンと一貫性のあるレイアウトを使う
- 似ているコードは似ているように見せる
- 関連するコードをまとめてブロックにする

詳細
- 見た目が美しいコードの方が使いやすい
- 改行を一貫性のあるような位置にする
- 繰り返しを行わないよう,同じようなコメントがあるなら上の方でまとめて1つのコメントにする
- たての線を揃える

改善前

```
public class PerformanceTester {
  public static final TcpConnectionSimulator wifi =
    new TcpConnectionSimulator(
      500, /* Kbps */
      80, /* millisecs latency */
      200, /* jitter */
      1 /* packet loss % */
    );

  public static final TcpConnectionSimulator t3_fiber =
    new TcpConnectionSimulator(
      45000, /* Kbps */
      10, /* millisecs latency */
      0, /* jitter */
      0 /* packet loss % */
    );

  public static final TcpConnectionSimulator cell =
    new TcpConnectionSimulator(
      100, /* Kbps */
      400, /* millisecs latency */
      250, /* jitter */
      5 /* packet loss % */
    );
}
```

改善後

```
public class PerformanceTester {
  // TcpConnectionSimulator(throughput, latency, jitter, packet loss)
  //                            [Kbps]     [ms]    [ms]     [percent]

  public static final TcpConnectionSimulator wifi =
    new TcpConnectionSimulator(500  , 80 , 200, 1);

  public static final TcpConnectionSimulator t3_fiber =
    new TcpConnectionSimulator(45000, 10 , 0  , 0);

  public static final TcpConnectionSimulator cell =
    new TcpConnectionSimulator(100  , 400, 250, 5);
}
```

- メソッドを使って整列させる...ヘルパーメソッドを用いて整列をさせる.重複部分をメソッドに吸収させる
- 意味のある並べ方をする
  例えば
  - アルファベット順に並べる
  - 「最重要」なものから並べる
  - 出力する順から並べる

- 宣言をブロックにまとめる...グループ分けする
- 空行を使ってコードを段落ごとに分ける...意味のある単位でグループ化する.段落ごとにようやくコメントを残すとさらにわかりやすい.
- **一貫性のあるスタイルが最重要**

### 5章 コメントすべきことを知る
**鍵となる考え(原則):**
- コメントの目的は,書き手の意図を読み手に知らせること
- 大切なのは,とにかく意味のあるコメントを残そうとすること

Point
- コメントすべきで「ない」ことを知る
- コードを書いてる時の自分の考えを記録する
- 読み手の立場になって何が必要になるかを考える

#### コメントすべきで「ない」こと
- コードからすぐに抽出できるコメントはしない
  ...コードを理解するのとコメントを読む時間が変わらなければコメントしない
- ひどいコードの「補助的なコメント」はしない
  ...名前がひどいのなら,コメントをつけずに名前を変える
  - 自己文章化してる関数名をつけ変える
  - 「 優れたコード > ひどいコード + 優れたコメント 」

#### コードを書いてる時の自分の考えを記録する
- なぜコードが他のやり方ではなくこうなってるのかを記す(「監督コメンタリー」)
- コードの欠陥にコメントをつける
  - よく使うコメントのフォーマット例と意味:
    - `TODO:(タスク)` ...あとで手をつけるもの
    - `FIXME:(タスク)` ...既知の不具合があるコード
    - `HACK:(タスク)` ...あまり綺麗じゃない解決策
    - `XXX:(タスク)` ...危険!大きな問題がある!
  - 大切なのは,そのコードをどうしたいかを自由にコメントにかくこと

- 定数にコメントをつける...定数の値にまつわる「背景を残す」

#### 読み手の立場になって何が必要になるかを考える
「他の人」 = プロジェクトを自分のように熟知してない人

- 質問されそうなことを想像して,その答えをコメントにする
- コードに対して間違って使う可能性があるものについてコメントを残しておく
- 全体像に対するコメントを残す
- 要約コメントを残す...コードブロックにコメントをつけて概要をまとめる
  - 塊を関数に分割できるならまずそうすべし(「優れたコード > ひどいコード + 優れたコメント」)
  - 4章の"意味のある単位でグループ化する.段落ごとにようやくコメントを残すとさらにわかりやすい."

#### コメントを書く作業手順
1. 頭のなかにあるコメントをとにかく書き出す
2. コメントを読んで改善が必要なものを見つける
3. 改善する

### 6章 コメントは正確で簡潔にする
**鍵となる考え(原則):**
- コメントは領域に対する情報の比率が高くなるようにする(コメントは正確でかつ簡潔なものにする)

Point
- コメントを簡潔にしておく
- (曖昧な)代名詞を避ける
- 関数の動作はできるだけ正確に記述する
- コメントに含める入出力の実れを慎重に選ぶ
- コードの意図は,詳細レベルではなく高レベルで記述する...「考えていたこと」をかく
- 「名前付き引数」コメント...よくわからない引数にはインラインコメントを使う.特にブール型
- 多くの意味が詰め込まれた言語や表現を用いて,コメントを簡潔に保つ(ex: キャッシュ, 正規化など)
- 全体像に対するコメントを残す

## Ⅱ部 ループとロジックの単純化
### 7章 制御フローを見やすくする
#### 条件式の引数の並び順

- 左側を調査対象(動的変数)にする。右側を比較対象(あまり変化しない変数)を入れる。

#### if/elseブロックの並び順

- 条件は否定形よりも肯定形を使う
- 単純な条件を先にかく。(if/elseが同じ画面に表示されるので見やすい)
- 関心を引くを引く条件や目立つ条件を先にかく

#### 三項演算子
三項演算子によって簡潔になる時のみ用いる。例えば、単純な2つの値から1つを選びだす場合。

```
time_str += (hour >= 12) ? 'pm':'am';
```

#### ネストを浅くする
- 関数内のループを浅くする時はreturnを使う
- ループ内ではcontinueなどを使う

#### その他

- do-while文は避ける
- goto文は避ける
- 関数から早く返す
- 実行の流れを終えるか確認する


### 8章 巨大な式を分割する
コードの式が大きくなればそれだけ理解が難しくなる

#### 8.1 説明変数を用いる

```
if line.split(':')[0].strip() == "root":
  ...
```

ではなく

```
username = line.split(':')[0].strip();
if username == "root":
  ...
```

巨大な式を分割する際に最も簡単なのは説明変数を導入すること。  
メリットは、

- 巨大な式を分割できる
- 簡潔な名前で式を説明することでコードを文章かできる
- コードの主要な「概念」を読み手が認識しやすくなる

#### 8.2 要約変数

```
if (request.user.id == document.ower.id) {
}

if (request.user.id != document.ower.id) {
}
```

ではなく

```
final boolean user_own_document = (request.user.id == document.ower.id);

if (user_own_document) {
  // ユーザーはこの文章を編集できる
}

if (!user_own_document) {
  // 文章は読み取り専用
}
```

とする

#### その他
- ドモルガンを使って、全てくくってnotとするのをやめる
- ロジックが複雑になった時は、立ち止まって他の解決策を見つける
- 文を分解する場合でも、説明変数や要約変数を用いる方法は使える
  - タイプミスを減らせる
  - 横幅が縮まる(読みやすくなる)
  - クラス名などを変更するときも変更箇所が少ない
- ifの条件の中身はできるだけ1行にする。できない場合は問題を否定したり、反対のことを考えてみる。


### 9章 変数と読みやすさ
変数を適当に使うと、以下の様な問題にぶつかるのでそれに対処する

1. 変数が多いと変数を追跡するのが難しくなる
2. 変数のスコープが大きいとスコープを把握するのが長くなる
3. 変数が頻繁に変更されると現在の値を把握するのが難しくなる

#### 9.1 変数を削除する

- 役に立たない一時変数を削除する。役に立たない一時変数とは
  - 複雑な式を分割してない
  - より明確になってない。
  - 一度しか使ってないので重複コードの削除になってない
- 中間結果を削除する。(中間結果を直接使う)
- 制御フロー変数を削除する


#### まとめ
変数をできるだけ減らして、「軽量な」コードにしよう！

- 邪魔な変数を削除する
- 変数のスコープをできるだけ小さくする
- 一度だけ書き込む変数を使う

## 第Ⅱ部 コードの再構成
### 10章 無関係の下位問題を抽出する
**「エンジニアリングとは、大きな問題を小さな問題に分割してそれぞれの解決策を組み立てることに他ならない。この原則をコードに当てはめれば堅牢で読みやすいコードになる」**

つまり、**プロジェクト固有のコードから汎用コードを分離する**

method
1. 関数やコードのブロックをみて、「このコードの高レベルの目標は何か？」と自問する。
2. コードの各行に対して、「高レベルの目標に直接効果があるのか？あるいは無関係の下位問題を解決してるのか？」と自問する
3. 無関係の下位問題を解決してるコードが相当量あれば、それを抽出して別の関数にする。

コツ
- 無関係の下位問題を積極的に探し出すこと

Point
- 汎用コードをたくさん作る
- 汎用的でなくとも下位問題を取り除くだけでも効果はあり
- 既存のインターフェースで汚くなったり分かりずらいものがあれば、自分でラッパー関数を用意して汚いインターフェースを覆い隠す
- だけど、やりすぎて、小さな関数を作りすぎないようにする

メリット
- 小さな問題に集中できる
- 大きな目標に集中できる
- コードの再利用が可能になる

### 11章 1度に1つのことを
**1度に1つのタスクを行う**  
読みにくいコードがあれば、そこで行われているタスクを全て列挙して、別の関数やクラスに分割する。

### 12章 コードに思いを込める
コードは「簡単な言葉で」書くべき  
「簡単な言葉で」書く手順

1. コードの動作を簡単な言葉で同僚にもわかる様に説明する
2. その説明の中で使っているキーワードやフレーズに注目する
3. その説明に合わせてコードを書く

**Point**: ロジックを明確に簡潔に説明してみる...その説明を元にコードを再構成してみる

