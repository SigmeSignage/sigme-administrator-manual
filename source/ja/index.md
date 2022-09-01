<!--toc=intro-->

# [[PRODUCTNAME]]管理者マニュアル 

[[PRODUCTNAME]]管理者マニュアルへようこそ！このマニュアルでは、[[PRODUCTNAME]]システムのインストールとメインセットアップを管理者向けに説明しています。

[[PRODUCTNAME]]を使い始めるには、CMSと少なくとも1台のサイネージプレーヤーが必要です。

お客様のインフラに[[PRODUCTNAME]]をインストールする準備ができましたら、ご利用の環境に適したインストールガイドをお選びください。

すでにCMSを導入している場合は、[ユーザーマニュアル](/manual/ja/index.html)をご覧いただき、[[PRODUCTNAME]] CMSの機能をすぐにお使いいただけます。

## オープンソース
[[PRODUCTNAME]]のソフトウェアの中心はオープンソースです。[[PRODUCTNAME]]でデジタルサイネージネットワークを運用するために必要なものはすべてオープンソースであり、今後もオープンソースであり続けることをお約束します。

オープンソースソフトウェアは、フリーソフトウェアであり、販売することはできませんが、ソフトウェアを顧客に無料で提供し、インストール、サポート、デザインサービスなどの付加価値サービスに課金することは可能です。これはAGPLv3ライセンスの下でカバーされており、これを行うために開発元であるXibo Signage社、および日本語化/機能拡張をおこなっているSigme Projectとの再販契約は必要ありません。

顧客は、本ソフトウェアがオープンソースであることを知らされ、要求に応じてソースコードのコピーを提供されなければなりません。

## [[PRODUCTNAME]]の ライセンス情報

[[PRODUCTNAME]]は原本であるXiboのライセンスポリシーを継承しています。

[[PRODUCTNAME]]は、いくつかのオープンソースプラットフォームとライブラリの力を借りています。

Xibo - デジタルサイネージ - http://www.xibo.org.uk

[[PRODUCTNAME]] - デジタルサイネージ - https://www.sigme.net


Copyright © 2018 Xibo Signage LTD and the Xibo Developers.

Portion Copyright © 2022 Sigme Project.

[[PRODUCTNAME]]はフリーソフトウェアです：あなたは、[Free Software Foundation](https://www.fsf.org/)によって発行された[GNU Affero General Public License Version 3](https://www.gnu.org/licenses/agpl-3.0.ja.html)またはそれ以降のバージョンのいずれかの条項の下でそれを再頒布および/または変更することができます。

[[PRODUCTNAME]]は有用であることを願って配布されていますが、市場 性や特定目的適合性についての暗黙の保証も含め、いかなる保証も行いません。詳しくはGNUアフェロ一般公衆利用許諾書をご覧ください。

[[PRODUCTNAME]]と一緒にGNU Affero一般公衆利用許諾書のコピーを受け取っているはずです。そうでない場合は、http://www.gnu.org/licenses をご覧ください。

## ドキュメント
このマニュアルは、[Creative Commons Attribution ShareAlike UK v2 License](https://spdx.org/licenses/CC-BY-SA-2.0-UK.html)の下でライセンスされています。

## 商用スポンサー
[[PRODUCTNAME]]は、Sigme Projectによってサポートおよび保守されています。

## サードパーティーライセンス
[[PRODUCTNAME]]は、サードパーティのライブラリやツールを使用しています。これらは、互換性のあるさまざまなオープンソースライセンスの下で[[PRODUCTNAME]]と一緒に提供されています。

## フォント
[[PRODUCTNAME]]には、[OFL](https://licenses.opensource.jp/OFL-1.1/OFL-1.1.html)および[CC-0](https://creativecommons.org/choose/zero/)ライセンスで提供されるフォントが含まれています。

- Aileron Regular (CC-0)
- Aileron Heavy (CC-0)
- Dancing Script Regular (OFL)
- Linear Regular (OFL)
- Railway Regular (OFL)

## コンテンツのライセンスについて
[[PRODUCTNAME]]は、このドキュメントに記載されている以上のコンテンツを規制する手段を持ちませんので、ご注意ください。コンテンツは、そのライセンスに記載された方法で使用されるようにしてください。

## [[PRODUCTNAME]]の翻訳
[[PRODUCTNAME]] CMSは完全に翻訳可能なので、すべての項目とエラーメッセージをローカライズすることができます。デフォルトでは、CMSはユーザーのブラウザーの言語を自動的に検出し、その翻訳が利用可能であればそれを使用します。

{tip}
CMSのメインメニューの管理セクションにある設定ページで、地域タブを使用して、特定の言語を使用するように設定することもできます。
{/tip}

Launchpad は翻訳を管理するために使用され、ここで見ることができます: https://translations.launchpad.net/xibo. 既存の翻訳を変更したり、新しい翻訳をプロジェクトに含めるには、Launchpadで行う必要があります。

翻訳は製品に依存しないものでなければなりません。Xiboの使い方や独自のテーマに合わせてLaunchpadの翻訳を変更することは許可されません。

Launchpad にはヘルプガイドがあり、システムの動作が説明されています。

## 技術的な詳細
[[PRODUCTNAME]]はGNU GetTextと呼ばれるシステムを使って翻訳を管理しています。このシステムは、ソースコードから英語の「キー」を取り出し、各言語の定義済み翻訳セットに渡します。これらの定義済み翻訳は、「コンパイル済み」形式でMOファイルに格納されており、ソースコードの/localeフォルダにあります。

キーはコードでは __() 構文で、Twig ビューでは trans ヘルパーで定義されます。

プロジェクトは通常の開発サイクルの中で、新しい翻訳可能な文字列を追加し、古い文字列を削除します。Launchpad のコンテンツは、現在の開発リリースのコンテンツを表します。

## Launchpadの外での翻訳
Launchpad は、翻訳に関する「真実の源」ですが、より便利であれば、POEdit のようなツールを使って Launchpad の外部で翻訳することも可能です。

そのためには、「翻訳のダウンロード」をクリックし、必要な言語を選択し、PO形式を選択する必要があります。出来上がったPOファイルはPOEditで編集し、再びLaunchpadにインポートすることができます。

POEditはCMSの/localeフォルダと互換性のあるMOファイルを作成できますが、もしあなたの翻訳がLaunchpadにアップロードされていない場合、MOファイルは次のリリースであなたの翻訳を含まないものに上書きされます。

## 翻訳を適用する
ソフトウェアの新しいリリースには、新しい翻訳が含まれています。もし、リリース前に翻訳を適用したい場合は、Launchpad から MO ファイルのダウンロードをリクエストし、CMS インストールの /locale フォルダに手動でコピーすることができます。

## プレーヤーの翻訳
Windowsプレーヤーは英語版のみです。

