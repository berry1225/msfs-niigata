# msfs-niigata

scenery作成で使用したファイルの一部をアップロードしました。
複数ファイルを扱う時のxmlファイルの書き方が分からない場合などに、一例として参考にしてもらえればと思います。
(buildしてからsceneryを読み込むところのいい感じの書き方が分からず1時間くらい詰まりました)

## 手順 (基本的にMSFS日本語wiki記載の手順に準拠)
* RenderDocを使用してファイルを作成
* Blenderで余計な頂点の削除、必要に応じてライトなど追加
* BlenderからMSFSToolkitを使ってexport
* PackageDefinitions以下のxmlファイルを編集 (exportしたファイルの追記)
* MSFSでxmlファイルを読み込む (前の手順で編集したxmlではなく、ひとつ上のフォルダ階層にあるxmlファイル)
* 先にパッケージをbuild (生成されるpackageからsceneryを読み込むため、buildしないと配置できない)
* sceneryから作成したファイルを探して配置
* 配置の過程でpolygonを使って自動生成の建物を無効化 (先に消してしまうと角度合わせや大きさ合わせが難しい)
* 保存してbuild
* packageフォルダに生成されたフォルダをCommunityフォルダに入れてMSFS再起動、無事反映されることを確認

## 諸注意
画像ファイルは、元ファイルがGoogle地図というのもありgit管理および公開はしていません。

代わりにlistというファイルに使用しているファイル一覧を載せています。
(textureフォルダ自体はMSFSToolkitで出力するものなので、特に事前準備する必要なし)

Blender2.91を使用していたら4ファイル目あたりで唐突にexportできなくなってしまったので、LTS版の2.83をインストールし直し。

## シアン抜き
そのままエクスポートすると他の建物と比べて青白くなってしまうようです (Google地図のデータは基本的に青空の時に取ったデータなので、どうしてもそっちの方向に振れてしまう)

Blender素人なのでいい感じにフィルタをかける方法は分かりませんが、export後(build前)にpngファイルを直接編集すればいい感じになるかもしれません。いわゆる「シアン抜き」のようなもの。

PhotoshopやCLIP STUDIO PAINT等の画像編集ツールのオートアクション機能を使って「色調編集→保存→閉じる」まで一括で全ファイル適用するといい感じかもしれません。
個人的には、レベル補正やトーンカーブを使って「全体：少し濃いめ」「青：少し薄め」がおすすめかもしれません。
また、textureフォルダの中身は同じファイルをexportする毎に都度生成されるので、画像編集する場合は最終段階で行うか、
編集後のtextureフォルダごとバックアップしてexport後に都度フォルダごと上書きするのがいいかもしれません。

「かもしれません」ばかりなのは、編集したものを配布する行為それ即ち改変なり規約違反なり、という解釈に対する逃げ道のためです。くれぐれも自己責任でお願いします。

実際にそれをやったかどうかも記憶にありません。(色合いが元データのものと違うように見えてもそれはきっと気のせいです、赤と青の3Dメガネをかけてから再確認することを推奨します)

まぁ、Google的にはフェアユースであればOKでしょ、とは思ってます。

## 夜景
Google地図から持ってきたデータは当然ながら夜景に対応していません。

でも光らせたいですよね。光った方がかっこいいですし。

もしかして、Blenderで編集する時に窓の前に透明な発光体を配置してやれば、いい感じに夜景っぽくなるのでは？

というわけで、夜景を光らせるための我流の手順です。
Blender歴3週間のわたしでもできたので初心者でもきっと大丈夫ですし、経験者ならなおさら問題ないと思います。

配置等を一旦すべて終わらせてシーナリーを完成させてから、追加作業として取り掛かった方がいいです。でないとシーナリーが永遠に完成しません。
さっさとアップロードして、夜景はアップデートとして対応しましょう。

### 夜景：個人的なやり方

Blenderでやること
* 保存しておいたオブジェクトを開く (エクスポートだけじゃなくてちゃんと保存しておきましょう)
* オブジェクトビュー画面で「追加」→メッシュ→平面
* 平面が追加されるので、好きな場所に移動して好きな形に変形
* 大抵は窓の形に合わせて変形させる (縦を少し短めにするとカッコよくなります)
* コピーしてたくさん窓を配置 (1平面ごとにオブジェクト統合しておくといいです)

**※平面には表と裏があるので注意 (最初に上を向いている方が表側です、この手順でやると基本的に表側しか光りません)**

マテリアルプロパティを開き、「MSFS Material Params」の欄に行く
* 「MSFS Standard」を選択。
* Albedo Colorは黒(#000000)、放射カラーとアルファは後で決める

下の方に行って「Render Parameters」の欄に行く
* Alpha mod..は「ブレンド」
* No cast shadow：チェック
* Day/Night cycle：チェック (昼夜関係なく常時光らせたい場合はチェックしない)

これで基本的な設定はOK。

放射カラー(つまり発光させる色、デフォルトは黒：発光なし)、アルファ(透過：0で透明～1で不透明)の設定例はこんな感じ。お好みで。
* 白：#FFFFEEでアルファ0.1
* オレンジ：#FFCC88でアルファ0.15

これをオブジェクトに適用すれば光るようになるはずです。
ただし、Blender上ではオブジェクトは透明になるので、ちゃんと光っているかどうかはMSFS側で確認しないといけません。
Blenderからエクスポートでファイル上書きする毎に、MSFS側でもいい感じにリロードしてくれます。

### 夜景：注意と問題点

以下、注意点と問題点みたいなもの。

・手順どおりにやったのに反映されない

**もしかして：表と裏が逆**

わたしも数回やらかしました。慣れれば分かるようになるんですが、作り込んでからこれになるとショックなので、
早い段階で「その面をちゃんと光らせられるかどうか」を確認した方がいいです。

ちなみにマテリアルに「裏面の非表示」みたいな項目があって、それの有無で裏面を表示させたりさせなかったりが切り替わります。
でも窓の場合は基本的に表面だけに反映したいので、方向間違わないように頑張りましょう、という話で。

・透明にしたはずなのにうっすら見える

アルファを低くして透過度を高めていますが、それでもうっすら透けて見えてしまいます。
**ほぼ完全に見えなくするやり方もあったような気がしましたが、どうやって設定したか分からず再現不可能でした。**

そもそも窓になっている部分に作ると思うので、実害の出る目立ち方はしないと思います。これよりもフォトグラメトリの粗の方がよっぽど気になるので、許容範囲！

一応、アルファを落とせば落とすほど目立たなくはなりますが、そのぶん光も弱くなります。ちなみに、Albedoを黒にしているのはこれの軽減のためです。(白だとすごく目立つので)

・いっぱい窓を作ると、近くに行かないと見えるようにならない

オブジェクトの数が多すぎると遠景で読み込んでくれないようです。なので、1平面ごとにレイヤー(オブジェクト)を統合するなど工夫するといいかもしれません。
要するに、北側の窓、南側の窓、西側の窓、東側の窓、で合計4つのレイヤーだけにする、みたいな。(もっとも、1平面あたり20～30とか作らなければ大丈夫っぽいですが)

Photoshopとかでよくあるやり方としては、元のレイヤーをコピーしてコピーしたものを統合、元のレイヤーは保持したまま非表示、って感じで。そうすると後から編集し直す場合に手戻りが少なくて済みます。

・夜景がチカチカする

せっかく頑張って配置しても、遠景だと夜景がチカチカするかもしれません。発光オブジェクトと元のオブジェクトが「こっちの方が前だ！」と競合するからだと思われます。
そもそも元ファイルがフォトグラメトリで凸凹してるというのもあって、なかなか制御は難しいです。
あと、正面よりもナナメから見える面の方がよりチカチカして見えます。

ただ、Asobo公式のフォトグラメトリでも遠景では夜景がチカチカしてるので、そういうものだと思って諦めた方が早いです。
前に出せば出すほどチカチカしづらくなりますが、だからといって前に出し過ぎると近距離で見た時にズレまくりで風情が台無しです。

遠景を取るか近距離のリアリティを取るかという話なら、両方のバランスのとれるあたりで妥協した方がいいです。
(近距離リアリティ主義者でしたが、あまりにチカチカするものなので匙を投げました)

100mくらい離れたところでそこそこズレて見えず、かといって遠距離や高高度から見た時にチカチカしすぎず、くらいの場所がちょうどいいです。

・そもそも遠くだと見えづらい

1km先から1m四方の窓をくっきり見ることができますか？そういう話です。そもそも現実世界の仕様です。

ちなみに、展望台部分などでバーっとまとめて貼った場合は多少見やすくなります。1km先でも30m四方の窓くらいはなんとなく分かりますよね。そういう話です。

~でも昼間ならともかく現実世界の夜景ならピンポイントに光ってる部分はなんとなく分かるんじゃないか？って気がしてきたのでこのあたりでやめます。~

・なんかマテリアルをいじってたら急に光らなくなった助けて

色々いじりすぎるとパラメーターが滅茶苦茶になるので注意しましょう。
項目がありすぎるのと、どれがMSFSで有効なやつでどれが無効なやつか分からないのと、そもそも連動してたりするのとで、正直どれがどれだかさっぱり。

滅茶苦茶になったら一旦そのマテリアルを放棄して新しいマテリアル作ってやり直した方が早いです。(作り直すのはあくまでマテリアルの方、オブジェクトの配置はそのままで)

ちなみにマテリアルを新しく作る場合、古いマテリアルは「古いので使わない」みたいな名前に変えておくと色々便利です。

・マテリアルをコピーする時のBlenderの挙動

マテリアルのある状態でコピーすると、マテリアルまで新規コピーされてしまうっぽいです。
マテリアル反映前に必要分コピーするか、コピー前にマテリアルをクリアするかしないと面倒なことになります。

あと、編集モードとオブジェクトモードを行ったり来たりするのが地味に分かりづらかったり、
コピー後に長さや角度を変更しようとしたらコピー後のファイルじゃなくてコピー元のファイルを選択した状態だったり(仕様らしい)、
blenderは罠が多いので大変でした。仲良くしましょう。わたしは我慢できませんでした。

・いいからラクな方法教えて

マンションなんかは、窓1枚作製→横列1列作成して統合→ひたすら縦に等間隔でコピーして統合→高さと角度調整、で1平面分を作ってから削る方法がラクでした。
削ったら半分どころかほとんど残らないかもしれませんが、それでもひとつずつ窓を配置するよりは効率的にできます。
削る前にバックアップ用のオブジェクトをコピーしておけば、やり直したい時も簡単。

最後にマテリアルを反映して、チカチカ対策で少し前方向と上方向にずらして完成。
このやり方でもだいたい1棟あたり1時間くらいは所要したので、もっとラクな方に倒れたいです。

・もっとラクな方法を教えて

建物が多すぎる場合は、優先順位をつけて「目立つところから」作るといいです。全部の建物に頑張って反映しても労力に見合わない。
~それに優先度高い方からやってると、だんだんと「このへん低優先度だしもうそろそろいいか」って区切りがつけやすくなります。~

そもそものNiigata Landarksも、全部配置するのが面倒だから高いものと目立つものから順番にやってますし。そういうもの。

・だんだん作るのが面倒になってきた

オブジェクトをたくさん配置するのが面倒ですか？気合いでなんとかしましょう。ローランド製の電子和太鼓でも叩きながらノリと勢いで作業するのがオススメです。びっくりするほどユートピア。

でもオブジェクトを配置するのも大変ですが、「どこが光る面なのか分かるように映っている写真」を探すことも同じくらい大変でした。資料、大事。
「どこが光る面なのか」というよりは「どこが光っていない場所なのか」(＝どこが作業しなくていい場所なのか) という情報の方が、ラクさという観点では重要でしょうか？


## FAQ

Blender歴3週間のわたしのやり方なので最適解ではないかもしれませんが、メモ程度に。

### 「欠落している面がある！」

重複削除する時か、どのあたりかは分かりませんが、ときどき面が消失するようです。見つけ次第補完しましょう。

* まず頂点をいくつか選択してから右クリックで面を生成
* 外観の色がおかしかったら、面選択モードで面を選択してから上段タブ「UV Editing」
* マテリアルの指定が違う場合は、エクスポート後の画像ファイルなどを参照しながら適当なマテリアルを選び直し
* マテリアル上の頂点を頑張って移動させて正確な外観になるように修正

できれば最初のオブジェクト作ってる時に教えてほしかった･･･

### 「朱鷺メッセやメディアシップのロゴってどうやって光らせてるの？」

オブジェクトを光らせるんじゃなくて、発光体を配置しています。

* オブジェクトモードで追加→ライト→ポイント

ビル屋上の赤いやつも同様です。点滅させることも可能です。youtubeで調べれば解説動画が出てくると思うので頑張ってください。
もっとも、公式の東京フォトグラメトリとか見た感じだと別の光らせ方してましたけど…… (窓と同じような方法)

### 「そもそも高い建物が自動生成されてないのはなぜ？」

MSFSの建築物は衛星写真からの自動生成だけだと思っていませんか？
Open Street Map (OSM) のデータも参照しているようです。
「総階数」もそのうちのひとつです。メートルではなくてこの「総階数」を参考にしてMSFSの建物は生成されています。

何も入れないバニラな状態の新潟にも、駅前に高々と聳えているビルがひとつありますよね。LEXNという名前の施設です。
LEXNのOSM上のデータを参照してみると「総階数：31」としっかり入力されています。
なので、MSFS上でも31階建ての建物が見事に再現されています。見た目の色は違いますが。
~Niigata Landarksは「高い建物が反映されてないので作った」というMODなので、LEXNは既に反映されてるので作らなくていいよね･･･~

朱鷺メッセもNEXT21も「総階数」のデータは入っていませんでした。(メディアシップは今見たら「総階数：20」って入ってましたが･･･)

要するに、総階数をぽちぽち入れていけばビルの高さについてはそこそこの精度で生成してくれるのでは？って感じです。
外観についてはそれこそ自動生成でいい感じにやってるようです。ラブラ万代の色合いの最限度は鬼。
ということで、オブジェクト生成するよりもOSMにデータ入れる方が簡単な気がします。
MSFSへの反映までどのくらいの期間が必要なのかは分かりませんが･･･

で、なんでMSFSの新潟には高い建物が少ないのかというと、直球で言えば新潟県民のOSMへの貢献が足りないから･･･
とは言いつつも、日本中でも世界中でも反映されてないところは山ほどあるので、卑屈になる必要はなさそうですね。
誰でも登録して編集できるので、気長にぽちぽち貢献しましょう。おー。

小学校の授業の一環でOSMに貢献しようとかそういうのがあればいいのに。
まぁ、それで間違った数値を入れてしまうと「メルボルン・シタデル」みたいなのが出来上がるんですけどね･･･

### 「解説動画作って」

```
　　∧＿∧
　 ( ´･ω･) いやどす
　　ﾊ∨/~ヽ
　 ﾉ[三ﾉ　|
　(L|く＿ノ
　　|＊　|
　　ﾊ､_＿|
""~""""""~""~""~""
```

## その他メモ

柳都大橋の白い部分(化粧張りって言うんですか？)だけはフォトグラメトリになかったので自作しました。
雰囲気重視なのでよく見たらディテールが甘々ですが、雰囲気重視なので気にしない。
kynttilä橋と比べたら秒でできました。(あっちはきっちり資料見て測りながら作ったのでそこそこちゃんとした形してます)

---

やる気と時間とチョコレートが揃ったら追記します。

