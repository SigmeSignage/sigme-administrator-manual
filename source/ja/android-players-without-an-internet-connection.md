<!--toc=android_install-->

# プレイヤーのオフライン実行

Xibo for Androidプレイヤーは、コンテンツのオフライン再生に完全に対応していますが、以下の点にご注意ください。

## [[PRODUCTNAME]]コンテンツ管理システム

プレイヤーが将来にわたってキャッシュするコンテンツの量は、[[PRODUCTNAME]] CMSで定義された設定によって決定されます。これらの設定は、[[PRODUCTNAME]] CMSの管理者がCMSの管理セクションにある**設定**ージで設定します。特に**全般** タブの**事前にファイルを送信しますか？**設定は、RequiredFiles への呼び出しが何秒先の未来を見るかを決定します（デフォルトでは 2 日間に設定されています）。

プレイヤーは、[[PRODUCTNAME]] CMSに接続し、登録情報を送信し、スケジュールをダウンロードするために、まずインターネット接続が必要です。

## ライセンスについて

Xibo for Androidは、14日間の試用期間後、およびその後30日に1回、ライセンス情報を確認するために有効なインターネット接続を必要とします。この期間が過ぎると、プレイヤーはXibo CMSからの新しいコンテンツのダウンロードを停止し、キャッシュされたコンテンツの再生を継続します。

{tip}
インターネット接続が不安定なプレイヤーや、インターネットに常時接続されていないプレイヤーについては、[オンプレミスライセンス](on-premise-licensing.html)をご覧ください。
{/tip}