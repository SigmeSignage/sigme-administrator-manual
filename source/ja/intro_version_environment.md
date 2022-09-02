<!--toc=intro-->

# バージョンと対応環境

[[PRODUCTNAME]]は、さまざまな環境、ウェブサーバー、ソフトウェアバージョンで動作させることができます。

{tip}
最新の機能、改善、バグフィックスを最大限に活用するために、常に最新のCMSリリースへのインストール/アップグレードを推奨しています。
{/tip}

旧リリースは、新しいメジャーバージョンのリリース後、一定期間サポートされます。

お客様がCMSのセルフホスティングを選択された場合、コミュニティによるベストエフォートサポートがプロジェクトのコミュニティサイトで利用可能です。ヘルプデスクによるサポートは提供されません。

お客様がCMSのクラウドホスティングを選択された場合、またはセルフホスティングCMSをエンタープライズプランに追加された場合（下記のサポート対象環境を満たすことが条件）、ヘルプデスクへのアクセスが含まれ、お客様はCMSまたはプレーヤーに関するチケットを開くことができます（それらがサポート対象バージョンであることが条件です）。

## 対応バージョン

[[PRODUCTNAME]]はXibo CMSのオープンソースをベースに、日本語化、機能追加などをおこなっています。[[PRODUCTNAME]]のバージョンのXibo CMSとの対応は以下の表の通りとなっています。

| [[PRODUCTNAME]]のバージョン　　　　| Xibo CMSのバージョン　　　　|
| ---- | ---- |
| Version 1.0.0                      | Version 3.1.4               |

## サポート対象環境
[[PRODUCTNAME]] CMS を企業等でご利用し、サポート等を必要とする場合は、システムの提供業者とエンタープライズサポート契約等が必要となります。通常これは有償契約となります。

サポート対象外の[[PRODUCTNAME]] CMSをインストールした場合、サポートとアドバイスはコミュニティを通じてのみ提供され、その内容は限定的なものになる可能性があります。

## サポート可能な構成

システムの構成状況によっても異なりますが、以下の条件が基本となります。

- Ubuntu サーバー 64 ビット - 現在の LTS リリース (18.04、20.04、22.04) または Debian の同等品
- SSH/HTTPS または TeamViewer 経由で CMS を実行しているサーバにアクセスできること、または CMS に SSH/HTTPS でアクセスできるワークステーションに TeamViewer でアクセスできること。
- ユーザー root または root になるための完全な sudo 権限を持つユーザーとしてアクセスできること。
- サーバーは、提供されたコンテナイメージをダウンロードするために、インターネットにアクセスできる必要があります。
- CMSのインストールに提供するdocker-composeファイルを使ってDockerでインストールする。
- CMS の前に TLS 終端処理を行う nginx または Apache のリバースプロキシーを設置することを推奨します。
- cms-web、cms-db、cms-xmrコンテナをそれぞれ1つずつ、memcachedを有効にした状態で設置します。
- カスタムコードは、カスタムモジュールまたはミドルウェアとして別途作成する必要があります。