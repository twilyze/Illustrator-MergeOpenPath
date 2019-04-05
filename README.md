MergeOpenPath.jsx
====

パスの端にある近接点を連結するIllustratorスクリプト

![banner](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/image/banner.png)


## 主な機能
- パスの端にある設定距離以下のアンカーポイントを連結
- 2通りの連結方法： 2点を中間位置に移動・2点を繋ぐ線を追加
- 角度や線の太さなどのプロパティが一致するものに対象を絞る機能


## ダウンロード
[GitHubのReleasesページ](https://github.com/twilyze/Illustrator-MergeOpenPath/releases) 
`MergeOpenPath.zip`

普通(.jsx)のと圧縮(.min.jsx)したのが入っていますが機能は変わりません。


## 使い方
`[ファイル]-[スクリプト]-[その他のスクリプト]`から選択するか、
あらかじめスクリプトフォルダにファイルを配置すれば`[ファイル]-[スクリプト]`内に表示されます。

 OS | スクリプトフォルダの場所
--- | ---
Win | Program Files/Adobe/Adobe Illustrator (ver)/プリセット/ja_JP/スクリプト/
Mac | アプリケーション/Adobe Illustrator (ver)/プリセット/ja_JP/スクリプト/


__念のためスクリプトを使用する前にファイルを保存してください__

連結したいアンカーポイントを選択してからスクリプトを起動し実行します。


## 設定
![main_window](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/image/main_window.png)


### これ以下の距離なら連結する
- アンカーポイント同士の距離がこの値以下なら連結します（単位はピクセル）
- スライダーは`0～10`までですがテキストボックスに直接入力すれば好きな値を設定できます


### 連結対象
1. 両方  
  `他のパスとの連結`後、`同じパスの両端を連結`を実行
1. 他のパスとの連結  
  2つのパスの端の点が連結できるかを探します
1. 同じパスの両端を連結  
  パスの最初と最後の点が連結できるかを探します


### 連結方法
距離や後述の条件をクリアし、2点のアンカーポイントを連結する時の動作。

1. 2点を中間位置に移動  
  `[オブジェクト]-[パス]`の`平均`(2軸とも)->`連結`と同じ動作（後述のハンドル情報を保持する場合）
1. 2点を繋ぐ線を追加  
  `[オブジェクト]-[パス]`の`連結`と同じ動作（後述のハンドル情報を保持しない場合）


### 縦軸・横軸で対の角度のパスに絞る
- パスの角度が縦軸（垂直）・横軸（水平）で反転されたものに絞ります
  - ※角度はパスの端とその隣にあるアンカーの位置から求める
  - 許容量に５を設定した場合、反転した角度から+/-５度分のパスも含めます

![v_h](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/image/v_h.png)


### プロパティが一致するパスに絞る
- チェックしたプロパティの値が一致する時だけ連結します
  - `レイヤー`はレイヤー名ではなく同じレイヤーに属するかどうか
  - あんまりデバッグしていないので上手くいかないプロパティがあるかも
  - （まず使わないと思いますがスクリプトの冒頭にあるコメントアウトを解除すれば`evenodd`も使えます）


### ハンドル情報を保持
連結方法によって動作が変わります。

- `2点を中間位置に移動`
  - 保持する  
    `[オブジェクト]-[パス]-[平均]`と同じ動作
  - 保持しない  
    両側のハンドルが削除されます
- `2点を繋ぐ線を追加`
  - 保持する  
    アンカーポイント間のハンドルがあれば保持されます
  - 保持しない  
    `[オブジェクト]-[パス]-[連結]`と同じ動作

![handle](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/image/handle.png)


### 非表示のパスを削除
- 選択したパスのうち非表示のものを連結後に削除します
  - 一つでも連結が行われていなければ削除しません
  - 非表示のパスを選択するにはパスの所属するレイヤーやグループを選択します


## 使用例
アピアランスの`変形`を使った鏡面編集後に、近接点をまとめて連結したい場合などに。  
（もっといい方法があるのかもしれない…）


### 2分割の鏡面編集
1. 中央に縦のガイドを設置
1. 鏡面編集用の直線を横向きにアートボードサイズで作成
1. レイヤーに`[効果]-[パスの変形]-[変形]`を`コピー数1・垂直軸にリフレクト`で適用
1. いろいろ描く
1. 鏡面編集用の直線を非表示
1. レイヤーを選択して`[オブジェクト]-[アピアランスを分割]`
1. スクリプトで連結


__レイヤー構造__

- 変形を適用するレイヤー
  - いろいろ描くレイヤー
  - 鏡面編集用の直線

__※アピアランスを分割する際、ロックされたガイドがレイヤーに混ざっていると分割できないので注意__


### 16分割の鏡面編集
![example_16](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/image/example_16.png)

[example_16.ai](https://github.com/Twilyze/Illustrator-MergeOpenPath/releases/download/v1.0.0/example_16.ai)

360° ÷ 16 = 22.5°

- 画像左：
  1. 鏡面編集用の直線を縦・横・112.5°(90+22.5)の3つ作成
  1. レイヤーに`[効果]-[パスの変形]-[変形]`を2つ適用  
    `コピー数1・垂直軸にリフレクト`と`コピー数7・回転45°`に設定
  1. いろいろ描く
- 画像右：
  1. 鏡面編集用の直線を非表示
  1. レイヤーを選択して`[オブジェクト]-[アピアランスを分割]`
  1. スクリプトで連結


## 注意
- 念の為実行前に保存しましょう
- 別のグループや複合パス同士でも条件を満たせば連結します
- 複合パスの中にあるグループは無視されます（スクリプトからはアクセスできない）
- なにかエラーが出る時はIllustratorを再起動すると治るかもしれません

- 基本的に最前面のパスから連結可能なパスを見つけ次第連結していきます
  - 距離が近い順に全て連結したい場合はshspageさんのスクリプトをお使いください
    > [s.h's page - [Illustrator] JavaScript scripts](http://shspage.com/aijs/#renketsu)


## 動作確認環境
Adobe Illustrator CS5.1 (Windows10 64bit)  
（確認できてませんがCS3以降ならたぶん動きます）


## お問い合わせ
[Googleフォーム](https://goo.gl/forms/COrRnU3ME2gcIzj62)  
[Twitter](https://twitter.com/twilyze)


## ライセンス
[MIT](https://github.com/twilyze/Illustrator-MergeOpenPath/blob/master/LICENSE)
