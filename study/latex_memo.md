# Latexについてのサンプル集
## 冒頭にとりあえずこれ貼っとけ
```
\documentclass[11pt,dvipdfmx]{jarticle}


\oddsidemargin 0cm
\evensidemargin 0cm
\textwidth 16cm
\textheight 24cm
\topmargin -2.0cm
\usepackage[dvipdfmx]{graphicx}
\usepackage{amssymb,amsmath}
\usepackage{color}
\usepackage{fancybox}
\usepackage{ascmac}
\usepackage{amsthm}
\usepackage{url} %%% パッケージ url を読み込む
\usepackage{cases}
\usepackage{bm}
\usepackage{here}
\usepackage{listings}
% \usepackage{listings,jlisting} %日本語のコメントアウトをする場合jlistingが必要
%ここからソースコードの表示に関する設定
\lstset{
  basicstyle={\ttfamily},
  identifierstyle={\small},
  commentstyle={\smallitshape},
  keywordstyle={\small\bfseries},
  ndkeywordstyle={\small},
  stringstyle={\small\ttfamily},
  frame={tb},
  breaklines=true,
  columns=[l]{fullflexible},
  numbers=left,
  xrightmargin=0zw,
  xleftmargin=3zw,
  numberstyle={\scriptsize},
  stepnumber=1,
  numbersep=1zw,
  lineskip=-0.5ex
}
%ここまでソースコードの表示に関する設定

\begin{document}

\begin{center}
\vspace*{6mm}
{\Large タイトル}\\
\vspace*{4mm}
学籍番号 **-*******\\
氏名 **** ****
\end{center}

\tableofcontents
\newpage

\end{document}
```

## 数式環境
### 一本の数式
- `\[ \]`
- `\begin{equation} \end{equation}`

### 複数本(それぞれの数式に番号を振る)
- `\begin{eqnarray} \end{eqnarray}`

## 微分表現
- ドット表現...`\dot{x}`や`\ddot`

## 連立方程式
```
\begin{numcases}
  {}
  m \frac{\mathrm{d}^2}{\mathrm{d}t^2} (x+l\sin\theta) = H &\\
  m \frac{\mathrm{d}^2}{\mathrm{d}t^2} (l\cos\theta) = V
\end{numcases}
```

## 行列
### 具体的に書き下す
- ()を使う時
  ```
  \[
    A = \left(
      \begin{array}{ccc}
        a & b & c \\
        d & e & f \\
        g & h & i
      \end{array}
    \right)
  \]
  ```

- []を使うとき
  ```
  \[
    A = \left[
      \begin{array}{rrr}
        -1 & 20 & 3 \\
        4 & -5 & 600 \\
        7 & 8 & -9
      \end{array}
    \right]
  \]
  ```

### 一般形
```
\[
  A = \left(
    \begin{array}{cccc}
      a_{11} & a_{12} & \ldots & a_{1n} \\
      a_{21} & a_{22} & \ldots & a_{2n} \\
      \vdots & \vdots & \ddots & \vdots \\
      a_{m1} & a_{m2} & \ldots & a_{mn}
    \end{array}
  \right)
\]
```

## 集合
`\mathbb{}`であらわす

### 例
実数集合は`\mathbb{R}`


## アルゴリズムをかく
### パッケージを読み込む
```
% アルゴリズム用のパッケージ
\usepackage{algpseudocode}
\usepackage{algorithm}
\usepackage{algorithmic}
```

### 補足：パッケージのインストールの仕方
以下のサイトから`algorithms.zip`をDLし、解凍しておく。

[CTAN: tex-archive/macros/latex/contrib/algorithms](https://ctan.org/tex-archive/macros/latex/contrib/algorithms)

解凍した場所（algorithms.insなどがある場所）で

```
latex algorithms.ins
```

をTerminalから実行して`algorithm.sty`と`algorithmic.sty`を生成する。そしてそれらのパッケージをTeXのpathが通っていることろに保存する。

TeXのPathの確認の仕方は、

```
export -p
```

で環境変数の一覧をだし、`PATH=...`のところをみるとPATHの一部としてTeXのPathを確認できる。

そのPathが通っているところで、`algorithms`ディレクトリを作成し、そこに先ほど生成した`algorithm.sty`と`algorithmic.sty`を写す(もしくはコピーする)。

実際にやったものがこちら。

```
export -p
cd /Library/TeX/texbin # TeXのPathの通っているディレクトリ
sudo mkdir algorithms
cd [algorithm.styとalgorithmic.styがあるディレクトリ]
sudo cp algorithmic.sty algorithm.sty /Library/TeX/texbin/algorithms/
```

### アルゴリズムの記述のやり方
#### 設定を整えておく
```
% RequireとEnsureをInputとOutputにする
\renewcommand{\algorithmicrequire}{\textbf{Input:}}
\renewcommand{\algorithmicensure}{\textbf{Output:}}
\newcommand{\AND}{\algorithmicand{} }

% アルゴリズムの表示の設定を変更
\makeatletter
  \renewcommand{\thealgorithm}{%
  \thesection.\arabic{algorithm}}
  \@addtoreset{algorithm}{section}
\makeatother

\makeatletter
  \renewcommand{\ALG@name}{アルゴリズム}
\makeatother
```

#### 記述する
実際のアルゴリズムの記述はこんな感じ

```
\begin{algorithm}[H]
  \caption{最短経路のアルゴリズム}
  \label{alg:hoge}
  \begin{algorithmic}
    \Require{グラフ$G(V,E)$}
    \Ensure{$x$:ほにゃらら}
    \State Q = $\emptyset$
    \For {v $\in$ V}
      \State d(v)=$\infty$;
    \EndFor

    \State Q.enqueue(s);
    \State d(s) = 0;

    \While {Q $\neq 0$}
      \State v = Q.dequeue;
      \For {w $\in$ Adj$_v$}
        \If {d(w) = $\infty$}
          \State d(w) = d(v)+1;
          \State Q.enqueue(w);
        \EndIf
      \EndFor
    \EndWhile
  \end{algorithmic}
\end{algorithm}
```

行番号をつけたい場合は、`\begin{algorithmic}`を`\begin{algorithmic}[1]`に変更する
