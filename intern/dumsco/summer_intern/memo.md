## 加速度センサーでノイズ除去
フィルタの次数を変えていく
適応フィルタ
https://ja.wikipedia.org/wiki/%E9%81%A9%E5%BF%9C%E3%83%95%E3%82%A3%E3%83%AB%E3%82%BF
iphysiometer

加速度センサの入力でフィルタ次数を決める...動的フィルタ,適応フィルタ
このフィルタ字数決め

### 今は,Savitzky-Golay FIR
平滑化フィルタ

センサーバー
モックアップ


### 以下,提出用
アプリ開発・アルゴリズム開発


### しばらくの方針
正解としてはピークを変動させないぐらいの滑らかさ
1. まず静止状態をとる.RGBの値と加速度の値を取ってきてグラフ化. (by iPhysioMeter)
2. 何パターンか,人工的にノイズを作って,グラフ化しノイズ発生時の波形について観察する
3. 2を元にフィルターを作成してみて,1のような波形に近ずくかをtry and error


## 論文読みメモ
### スマートフォン式光電容積脈波測定法─日常生活中における有効利用へ向けて─
(Kenta MATSUMURA,Jihyoung LEE,Takehiro YAMAKOSH,2016)
体動について
 - 可視光は，近赤外線光と比べ非常に生体吸収性が高いため,可視光PPGは，近赤外線光PPGよりも，非常に浅い部分を測定していることになる.ここより，PPGの大きな弱点である体動アーチファクトへの耐性が，可視光，それも特に緑色や青色光において向上する，という興味深い現象が生じる.
 - 区間平均処理
