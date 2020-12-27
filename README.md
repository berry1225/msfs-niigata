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

---

やる気と時間とチョコレートが揃ったら追記します。

