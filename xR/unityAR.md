# 『UnityによるARゲーム開発』
## 1章 はじめに
#### 概要
- リアルアドベンチャーゲームの定義
  - 位置情報に基づいた,拡張現実ゲーム

- リアルアドベンチャーゲームの核となる要素
  - 位置情報...プレイヤーの現実世界における位置のトラッキング.位置情報を用いたインターフェースの開発にはGISスキルが必要.
  - 拡張現実感(AR)...モバイルデバイスのカメラウの映像をゲームの背景として使用する.
  - アドベンチャーゲーム...リアルワールドという範囲内で,プレイヤーがアバターになり,調査任務やパズルを解決することを繰り返し,最終的に何らかのストーリーに基づいたゴールを達成することが求められるゲーム.
    - 現在はRPGが主流
    - その他シューティングゲームなど新しいものが出るかも?
    - 新しいコンセプトの統合についても学ぶ

- サンプルゲームFoody Goのゲームデザイン
  - 位置情報に基づく拡張現実という要素に重点をおく
  - その他プレイヤーマッピングなどの機能にも導入していく

- ゲームプロジェクトの作成
- ソースコードはWebに上がってる：[ここ](https://github.com/oreilly-japan/augmented-reality-game-development-ja/tree/master#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%82%B3%E3%83%BC%E3%83%89)

### モバイル開発を始める
iOSのセッティングに関することは次のURLを参照にする

- [Unity iOS アカウントセットアップ](https://docs.unity3d.com/jp/current/Manual/iphone-accountsetup.html)
- [Unity iOS 開発を始める](https://docs.unity3d.com/jp/current/Manual/iphone-GettingStarted.html)
- [UnityでiPhoneアプリ（iOS）向けにビルドする方法【初心者向け】](https://techacademy.jp/magazine/2483) ...バージョンは古いがこれが一番わかりやすかった

#### 大雑把な手順
1. Apple デベロッパーアカウントをセットアップする
2. Unity XCode プロジェクトの構成(ビルド)
3. XCodeでシミュレーション

以下,詳細を見ていく

#### Apple デベロッパーアカウントをセットアップする
1. Apple IDの登録 or  アップルデベロッパープログラムに登録(後者は年間1万ほどかかる)
2. 最新の Xcode をインストール
3. Xcode に Apple ID を加える
  - Xcode > Preference > Accounts で出てくる画面の左下の+のとこををクリックしてApple IDを登録
  - 「Team のセクションにはアップルデベロッパープログラムのすべてのチームのリストがあります。アップルデベロッパープログラムに加入しないフリー版の Apple ID を使っている場合は、自分の名前を使った個人のチームが自動的に作成されます。」

#### Unity XCode プロジェクトの構成
UnityでBulidするといろんなファイルが構成される
詳細はここ:[Unity XCode プロジェクトの構成](https://docs.unity3d.com/jp/current/Manual/StructureOfXcodeProject.html)

1. Swicth PlatformでiOSに
2. Player Settings > Other Settings > Bundle Identifier
  - 「iOS Developer PortalのIdentifiresで設定するApp ID及びBundle IDと一致している必要があります」
  - 今の段階では設定しなくてもビルドは通る
3. Player Settings > Configurarion > Target SDKを「Simulator SDK」にする(?)
  - Target iOS Versionはそのままにしてる
  - 記事では,"Optimization > SDK Versionを「Simulator SDK」にしてシミュレーターで動かせるようにする"と書いてたが,手元のUnityではみつからなかった.
4. [Add Open Scene]でSplashを追加してチェック
5. Buildする

以上でビルド成功

+α
- 「Resolution and Presentation」でiPhoneでの画面の向きを決める
  - たてゲームの場合そのまま
- 「Exit on Suspend」...Homeボタンが押された場合に,バックグラウンド処理

エラーが出る場合はPlayer Settingsの各項目などを確認

#### XCodeでシミュレーション
1. ビルドで作成されたファイルの1つの".xcodeproj"拡張子のつくファイルをクリックしてXCodeを立ち上げ
2. ⌘＋RのXcodeのショートカットキーでシミュレータ起動

注意警告はいくつか出るが実行可能

- 3.5-inchと4-inchで確認すること.App Storeへ提出するには、両方の画面のスクリーンショットが必要だから.

#### +α
iOS機能へのアクセスとして,「iOS デバイスの大半の機能は Input や Handheld クラスを介して公開されています」


### Unityを始める
ここでは「メモ」だけ
やったことはスプラッシュ画面を作ること

#### プロジェクトの作成
- Sceneの保存
  - 保存の際は,[File] > [Save Scene as...]で"Splash"
- Layoutを整える
  - レイアウトをTallにしたあと,GameタブとSceneタブが同じ幅で横並びになるようにする.
  - これをレイアウトとして保存する
  - [Window] > [Layout] > [Save Layout]

- UIのPanelとTextを配置

#### ビルドとデプロイ
ここではiOSについて記す
ビルドの基本は上記で述べた通りだが,ビルドでまだ必要な手順があるらしい

- [Configurarion] > [Camera Usage Description]を「AR画面の背景に使用」に変更
- [Configurarion] > [Location Usage Description]を「マップの現在値を更新するために使用」に変更

これによりそれぞれの機能が使えるようになる

## 2章 プレイヤー位置のマッピング
Google Static Maps APIを用いる
使い方は

```
https://maps.googleapis.com/maps/api/staticmap?{パラメータ}
```

これでリクエストに対する一つの画像が返される
説明のあったパラメタ
- center ...リクエストする地図の中心の座標の緯度経度
- size ...画素サイズ(これがないとerror)
- zoom ...ズームレベル.だいたい17
- format ...取得する画像の形式.pngがいい
- maptype ...地図の種類のリクエスト.次の4種類がある
  - roadmap ...よく見るGoogle Mapのマップ
  - satellite ...衛星地図
  - terrain ...標高情報+roadmap
  - hydrid ...衛星地図+roadmap

### マップ実装
- 領域全てを1まいのタイルでやると画質が悪い
  → タイルを敷き詰めることで画質向上を図る
- cmd+Dで複製
- このタイル敷き詰めに対する計算は"GoogleMapTile"スクリプトがやってくれる


### コルーチン
#### コルーチンとは
> コルーチンとはプログラミングの構造の一種。コルーチンの特徴は処理の途中で一旦中断したりあとからその行から再開できたりすることだ。C#では以下の様な実装の仕方をする。
> ```
> IEnumerator coRoutine(){
    msgText.text += "1 ";
    yield return null;
    msgText.text += "2 ";
    yield return null;
    msgText.text += "3 ";
}
> ```

- IEnumeratorはコルーチンのインターフェースという意味
- yield return null はココで処理を中断するということ

#### コルーチンを使う(Unity only)
例
```
//例えばStart()関数で使いたい時
void Start(){
    StartCoroutine("coRoutine");
}
```

- コルーチンを動かしたいときは、StartCoroutine()を使う
  - そもそも,コルーチンはインターフェースゆえ`coRoutine();`と書いても動かない
  - `StartCoroutine("coRoutine")`で「"coRoutine"というコルーチンを開始する」ということ

#### yield return nullについて
```
//例えばStart()関数で使いたい時
void Start(){
    StartCoroutine("coRoutine");
    StartCoroutine("coRoutine");
}
```

とすると,出力は

> 1 1 2 2 3 3

となる(`yield return null`で1フレームぶん中断されてるため.)

#### コルーチンの中断・終了の仕方
- `yield return null;` ...1フレーム処理を中断し、再開する
- `yield return new WaitForSeconds( num );` ...num秒間の間処理を中断する
- `yield break;` ...コルーチンを途中で終了.再開はしない.

#### Update()内でStartCoroutine()を使うときの注意
> Update()内でStartCoroutine()を使うときは注意が必要です。Update()は毎フレームごとに処理される関数なので、スタートさせるコルーチンが１フレームで終了しない場合だと、毎フレームごとにどんどん増えていきます

コルーチンの唯一性を保ちたい場合は,例えば下のように,isRunningでコルーチンの動作状態を保持し、動いている場合はyield breakして、余計なコルーチンが作られるのを阻止する.

例
```
bool isRunning = false;

IEnumerator coRoutine(){
    if (isRunning)
        yield break;
    isRunning = true;

    msgText.text += "1 ";
    yield return null;
    msgText.text += "2 ";
    yield return null;
    msgText.text += "3 ";

    isRunning = false;
}
```

#### 参考
- [Unityにおけるコルーチンの性質まとめ](https://qiita.com/kazz4423/items/73219068684e87adc87d)

### サービスの設定
- サービス...本書では,他のゲームオブジェクトに消費され,自己管理クラスとして実行されるコードのことを指す

ここでは2つのサービスを設定する
- GPS Location Service ...プレイヤーの位置情報を得るためのサービス.
- CUDLR ...デバックのためのサービス.コンソールデバックツール.

#### CUDLRの設定
- Asset Storeから取ってくる(なお,Unity2018には対応してるか怪しい模様.でも,ちゃんと動いた.)
- デバイスの監視に加えて,簡単なコンソールコマンドをリモートで実行するためにも利用できる
- CUDLRはゲームの一部をWEBサーバーに変えてしまう
  - CUDLRはネットワーク上のどのコンピューターからでもアクセスできるため,ゲームを制御するために物理的な接続もUnityの実行も必要ない
  - ゆえに,ゲームのリリースをする際はCUDLRのオブジェクトを消すべし

#### Asset Store
- 場所が変わってた
  - [Window] > [General] > [Asset Store]


## エラーメモ
### iOS実機ビルドの際
XCode側で

```
Code Signing Error: Signing for "Unity-iPhone" requires a development team. Select a development team in the project editor.
Code Signing Error: Code signing is required for product type 'Application' in SDK 'iOS 11.4'
```

[参考文献1](https://qiita.com/ohkawa/items/044dd9b3bed2e5ee5fce)を見たら,Bundle Identifireの問題っぽい?
実際,Bundle Identifireを見たら,ちゃんと書き換えられてなかった.正しいものにしたらエラーが変わった.
これは,iPhoneデバイスを繋いでなかったり,実行先をiPhoneデバイスにしたなかったりした.

他にもエラーがでた

```
Code Signing Error: Signing for "Unity-iPhone" requires a development team. Select a development team in the project editor.
Code Signing Error: Code signing is required for product type 'Application' in SDK 'iOS 11.4'
```

ん?変わってねえwww
[参考文献2](https://vracademy.jp/blog/?p=936)を見たら,実機ビルドのために,これは,Signal > Team に Apple ID を登録しなければならないらしい.(参考文献2より)

これやったら,
```
The backing file has been modified outside of Xcode.

~/dev/unity/FoodyGO/FoodyGo for iOS/Unity-iPhone.xcodeproj
```

というポップアップがでたので,適当に`From Disk`選んだら,XCodeが終了した...(ただのアプリケーションエラーだった模様)

もう一度,気を取り直してTeam登録して再生ボタン押したら...なんかいっぱい警告出たけどビルドできた!...と思ったらまたエラー

- ロックしてないのにロックエラー
  ```
  Development cannot be enabled while your device is locked
  ```
  - 解決策:参考文献3によるiPhoneのプライバシー設定を位置情報とプライバシー設定に関してリセット(設定 > 一般 > リセット > 位置情報とプライバシー設定をリセット)すれば解決するらしい.理屈は知らない.

- 最後もう一つエラー
  ```
  Could not launch "FoodyGO"
  ```
  - 解決策は参考資料２にまんま書いてあった.iPhone側で認証してあげれば良い.
  - 設定 > 一般 > デバイス管理で,自分のApple IDを洗濯して,「信頼」

以上をへて,実機ビルド完了!!(長い)



#### 参考
1. [アプリを実機で動かそうとしたら CodeSign error: code signing is required for product type 'Application' in SDK 'iOS 7.1'エラー](https://qiita.com/ohkawa/items/044dd9b3bed2e5ee5fce)
2. [【Unity】iOS・Android向けビルドのチュートリアル](https://vracademy.jp/blog/?p=936)
3. [iPhoneアプリ開発実践！その5 -「Development cannot be …」エラー](https://ameblo.jp/t-higashi5494/entry-12296196378.html)


## 3章 アバターの作成
### 概要
やることは以下のPoint
- 標準のUnityアセットのインポート
- 3Dアニメーションキャラクターの扱いについて
- 3人章視点コントローラーとカメラのポジショニング
- 自由視点カメラ
- クロスぷらっとフォーム入力
- キャラクターコントローラーの作成
- コンポーネントスクリプトの更新
- iCloneキャラクターの使用

### 標準のUnityアセットのインポート
アセットを利用して手軽にゲーム開発しましょー.
標準アセットを利用して、都合に置いてはアセットを書き直したり、置き換えたりする.

#### 標準アセットのインポート
1. メニューバーの[Asset] > [Import Package] から標準アセットの項目がみられる.このうち,CamerasとCharactersとCrossPlatformInputのものを全てインポート(しかし、そのままだと、不必要なものも含んでおり、プロジェクトの肥大化を招くためその対処が必要となる.この章の後半で扱う)

#### 3Dアニメーションキャラクターのの追加
2. Assets/Standard Assets/Characters/Third Person Character/PrefabsからThirdPersonControllerをHierarchyウィンドウに.
  - 開発が問題ないことを確認するために、標準キャラクターのEthanでプロトタイピング.
3.  ThirdPersonControllerの名前をPlayerにする
  - 標準Assetsの多くは、Playerオブジェクトに対し、Playerという名前でアクセスするようになってる
  - この状況でゲームスタートしても動かない

#### カメラの切り替え
Mapシーンを自由に動き回れる三人称視点のカメラの追加.

4. Assets/Standard Assets/Cameras/PrefabsからFreeLookCameraRigを選択、Hierarchyウィンドウに移動.Playerのこオブジェクトにする.
  - Free Look CmaコンポーネントのAuto Target PlayerのチェックボックスにチェックするとPlayerという名前がついたオブジェクトを自動追跡
5. Main Cameraを消す
6. Assets/Standard Assets/CrossPlatformInput/PrefabsからDualTouchControlsをHierarchyウィンドウに.
  - これにより、「デュアルタッチコントロールインターフェースがオーバーレイ」されるらしい(日本語でおk)
  - ここでエラー
    ```
    Assertion failed on expression: 'modifications.empty()'
    UnityEditorInternal.InternalEditorUtility:HierarchyWindowDrag(HierarchyProperty, Boolean, HierarchyDropMode)
    UnityEngine.GUIUtility:ProcessEvent(Int32, IntPtr)
    ```
  - ということで、まず、CrossPlatformInputについて調べた
    - CrossPlatformInput:複数のプラットフォームに対応した入力インタフェース
    - なんのエラーかわからない...
    - 参考:
      - [【Unity】CrossPlatformInputを使う その1 Import編](https://www.urablog.xyz/entry/2016/07/24/070000)
      - [【Unity】CrossPlatformInputを使う その3 DualTouchControls編](https://www.urablog.xyz/entry/2016/07/26/070000s)
    - 次のスッテプの[Moblie Input] > [Enable]押せない。。。と思ってたら、最初からEnableになってたらしい...なんだよ...
7. メニューバーから[Moblie Input] > [Enable]
8. 再生すると、パネルをCtrl+クリックでカメラが動かせるようになる...らしいがJumpしかできないw(とりあえず、先に進む)(実機ではできた)

インポートしたAseetの中身を書き換える

Input.compassはUnityEngineクラスのInputインターフェース?のインターフェース？Compassとして標準装備
Input.compass.enabledはstatic変数

```
// axis の周りを angle 度回転する回転を作成します
public static Quaternion AngleAxis (float angle, Vector3 axis);
```

Vector3.up ...upはVector3のstatic変数.Vector3(0, 1, 0)と同じ.
- [Vector3 - Unity スクリプトリファレンス](https://docs.unity3d.com/ja/current/ScriptReference/Vector3.html)

float型の後にはfをつける
Quaternion.Slerp ...球場補間.(なおビルド時回転せず...)

#### CharacterGPSCompassController
名前空間を使う(Unityは標準ではこれを必要としない)


## 4章 獲物の生成
