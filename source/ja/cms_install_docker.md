<!--toc=cms_install-->

# Dockerでインストール
Dockerは、あらゆるアプリケーションをあらかじめ設定されたコンテナ内にパッケージして実行するためのアプリケーションです。このため、Xibo CMSを導入する際には、私たちが推奨する構成に従うことで非常に簡単に導入することができます。

## 既存のDockerクラスタ
独自のDocker環境をお持ちの場合、docker-composeが提供する自動化機能を使わずに実行したい場合があります。この場合、Dockerコンテナを引き出して起動し、[[PRODUCTNAME]]を手動でインストールすることになります。

コンテナに期待される構造は以下の通りです。

### コンテナ
2つのコンテナが提供されています。

- web
- xmr

これらはDocker Hubによってビルドされ、`xibo-signage/xibo-cms`と`xibosignage/xibo-xmr`にパッケージ化されています。

[[PRODUCTNAME]]はデータベースも必要とします。Docker Hubで利用可能な`mysql`コンテナの使用をお勧めします。`cms-web`コンテナにリンクできるものであれば、MySQLベースのコンテナでもかまいません。また、外部のデータベースを利用する場合は、以下のように `cms-web` コンテナに環境変数で指定することで利用可能です。

- MYSQL_HOST
- MYSQL_PORT
- MYSQL_HOST、MYSQL_PORT、MYSQL_DATABASE
- MYSQL_USER
- MYSQL_USER MYSQL_PASSWORD

コンテナを自分で実行する場合は、`/containers` フォルダにある各コンテナの `DockerFile` を読んで理解する必要があります。

また、コンテナ内のSMTPを設定するために、環境変数を渡すこともできます。すべてのオプションは、アーカイブにある `config.env.template` と `config.env.template-remote-mysql` ファイルに完全に記述されています。

### データの保存
Dataフォルダは、Dockerコンテナの外側にボリュームとしてマッピングし、コンテナのアップグレード時にもデータを保持できるようにします。

`docker-compose`では以下のDataフォルダが使用されますので、環境に合わせて設定してください。

- ライブラリストレージは、`/shared/cms/library`にあります。
- データベースストレージは、`/shared/db`にあります。
- 毎日の自動データベースバックアップは `/shared/backup` にあります。
- カスタムテーマは、`/shared/cms/web/theme/custom` に配置します。
- カスタムモジュールは、`/shared/cms/custom`に置く必要があります。
- ユーザーが作成したPHPや[[PRODUCTNAME]]の外部リソースで、同じウェブサーバーでホストされたいものは、`/shared/cms/web/userscripts`に置いてください。これらは、`http://localhost/userscripts/` から利用することができます。

## LinuxでのDocker

Dockerは64ビットLinuxベースのシステムにインストールすることができ、あなたの特定のLinuxディストリビューションの手順に従ってください。Docker EE (Enterprise Edition)をインストール済みであるか、またはそのオプションを検討しない限り、安定版ビルドをインストールし、Docker CE (Community Edition)をインストールする必要があります。

- **Dockerのインストール方法**は、Docker社のウェブサイトの以下の場所に記載されています。[Linuxディストリビューション向けDockerインストール](https://docs.docker.com/engine/install/)
- Linuxでは、**Docker Compose**は上記のパッケージには含まれていないため、インストールする必要があります。[Docker Releases](https://github.com/docker/compose/releases/tag/v2.10.2)にアクセスし、最新のリリースに記載されているコマンドを実行してください。

###[[PRODUCTNAME]] Docker アーカイブのダウンロードと展開

[[PRODUCTNAME]] をインストールするには、`root` ユーザーである必要があります。

- コマンドラインシェルで`sudo su`を実行するか、直接rootでログインして、`root`になりましょう。

最新の[[PRODUCTNAME]]のDockerインストールファイルは、[ここをクリック]してダウンロードできます。

- アーカイブは、ホストマシンの適切な場所に解凍する必要があります。ライブラリのコンテンツとデータベースは、このフォルダの下に書き込まれます。

アーカイブには`xibo-docker`というサブフォルダが含まれていますが、これはアーカイブの「ベストプラクティス」です。

この後の説明では、アーカイブに含まれる**サブフォルダ内にいる**ことを確認してください。

## 設定の確認と編集

[[PRODUCTNAME]] を初めてインストールするときは、Docker に環境の設定を伝えるための設定ファイルが必要です。

- このファイルは`config.env`と呼ばれます。このファイルには、ファイルを保存する場所やメール設定などが書かれています。

詳細な手順が書かれたテンプレートファイルがリリースアーカイブで提供されており、これは`config.env.template`と呼ばれています。

- このファイルをコピーして、`config.env` に名前を変えてから、nano や gedit などのテキストエディタで編集してください。

もし、[[PRODUCTNAME]]にメールを送信させたくない場合は、これらのオプションの設定を省略することができます。

Dockerは、データベースのデータやCMSのカスタムファイルを格納するためのデータフォルダをマッピングします。これらは、デフォルトでは、リリースアーカイブを含むフォルダの`共有`サブフォルダ内に表示されます。

### 異なるポートを使用する

デフォルトでは、Xibo はポート 80 をリッスンするウェブサーバーを起動します。

- もし、すでにホストマシンのポート80をリッスンしているウェブサーバーがある場合や、別のポート番号を使いたい場合は、`cms_custom-ports.yml.template`ファイルをコピーして、`cms-web`の`ports`セクションを変更する必要があります。

- このファイルは`cms_custom-ports.yml`という名前で保存されているはずです。

同様に、[[PRODUCTNAME]]のXMRサーバーはポート9505でリスニングを開始します。

- もし、別のポート番号を使いたい場合は、`cms_custom-ports.yml.template`ファイルをコピーして、`cms-xmr`の`ports`セクションを変更することで対応できます。

- Docker Compose YMLファイルのportsセクションは、`<host>:<container>`というフォーマットでポートをリストアップします - ポート8080に移動するには、`8080:80`と宣言します。

このファイルを使用するには、以下の説明で`docker-compose up -d`コマンドを`docker-compose -f cms_custom-ports.yml up -d`に置き換えてください。

### リモートMySQL

デフォルトの `docker-compose.yml` ファイルには MySQL 用のコンテナが含まれていますが、Xibo 用のデータベースとして外部/リモートの MySQL インスタンスを使用して実行することができます。

- そのためには、`config.env` ファイルを `config.evn.template-remote-mysql` のテンプレートに基づいて作成し、以下の説明の `docker-compose up -d` コマンドを `docker-compose -f cms_remote-mysql.yml up -d` に置き換えてください。

### HTTPS/SSL

[[PRODUCTNAME]]をセキュアなプライベートネットワーク以外で動作させる場合は、SSLで実行する必要があります。DockerコンテナはSSLを提供しないので、外部のウェブサーバでSSLの終了と`cms-web`コンテナへのリバースプロキシを行う必要があります。

このアーキテクチャを実現するための良いリソースがたくさんあります。例えば、[nginx-proxy](https://github.com/nginx-proxy/nginx-proxy)コンテナを使用することができます。

ホストマシンで既にウェブサーバが動作している場合、リバースプロキシの設定は簡単です。

Apache の 'VirtualHost' の例を以下に示しますが、ポート 8080 に対してカスタムポートを設定したものとします。

```
Listen 443

NameVirtualHost *:443
<VirtualHost *:443>

    SSLEngine On
    ProxyPreserveHost On
	RequestHeader set X-Forwarded-Proto "https"

    # Set the path to SSL certificate
    # Usage: SSLCertificateFile /path/to/cert.pem
    SSLCertificateFile /etc/apache2/ssl/file.pem


    # Servers to proxy the connection, or;
    # List of application servers:
    # Usage:
    # ProxyPass / http://[IP Addr.]:[port]/
    # ProxyPassReverse / http://[IP Addr.]:[port]/
    # Example:
    ProxyPass / http://0.0.0.0:8080/
    ProxyPassReverse / http://0.0.0.0:8080/

</VirtualHost>
```

{tip}
LetsEncrypt SSL証明書を使ったSSL用Apacheリバースプロキシの設定例については、[こちら](https://letsencrypt.org/ja/)をご覧ください。
{/tip}

### CMSコンテナのインストール
`config.env` に変更を加えてファイルを保存したら、アーカイブを解凍したフォルダでターミナル/コマンド・ウィンドウを開いてください。

- `docker`コマンドを実行できる権限を持つユーザーとして、単純に実行します。

```
docker-compose up -d
```

{tip}
コマンド実行後、CMSのセットアップが完了するまでに時間がかかる場合がありますが、しばらくお待ちください。
{/tip}

これで[[PRODUCTNAME]] CMSが起動されます。

CMSは、デフォルトの認証情報で完全にインストールされます。

- ユーザー名: [[PRODUCTNAME]]_admin
- パスワード: password

すぐに CMS にログインして、そのアカウントのパスワードを変更してください。

- CMS へのログオンは 'http://localhost' で可能です。別のポート番号を設定した場合は、URLにその番号を追加してください。例えば、'http://localhost:8080`。

### 設定の調整
iptablesベースのファイアウォールを使用している場合、CMSのインバウンドポートを許可する必要があります。

特に代替ポートを設定していない場合は、以下の設定が必要です。

- 受信TCPポート9505（XMRプッシュメッセージング用）
- インバウンドTCPポート80（HTTPトラフィック用） AND/OR
- 受信TCPポート443（HTTPSトラフィック用 - SSLを使用している場合）

## XMR - プッシュ型メッセージング

Dockerをインストールすると、XMRが自動的に実行され、ほとんどの設定が行われた状態で提供されます。ただし、初回ログイン時に、CMSのXMR公開アドレスを調整する必要があります（詳細は後述）。

**これは最初の起動時にのみ行う必要があります。**

CMSメインメニューの**管理**セクションにある**設定**ページに移動し、**ディスプレイ**タブを選択します。
XMRiパブリックアドレスィールドのデフォルトは`tcp://cms.example.org:9505`ですが、あなたのネットワークに合うように調整する必要があります。

- アドレスの形式は次のようにする必要があります。

tcp://<IP_address>:<port>

- デフォルトの<port>は9505で、docker-composeの設定でカスタムポートを指定していない限り、この値に設定する必要があります。

CMS が DNS 名で利用できる場合 - たとえば CMS のウェブページが https://mydomain.com にある場合、`cms.example.org` を `mydomain.com` に置き換えるだけでよいでしょう。CMSがIPアドレスによってのみ利用可能な場合は、代わりにIPアドレスを入力してください。

{tip}
CMSサーバーでローカルファイアウォールを使用している場合、XMRが動作するためにポート9505とウェブインターフェースのポート80/443の受信を許可する必要があります。
{/tip}

## XTR - ルーチンタスク
XTRは、dockerのインストール時にデフォルトで設定されています。

{tip}
Xibo CMSをインストールしたら、Xibo CMSインストール後のセットアップガイドを見て、さらなる設定オプションを確認してください。
{/tip}

## [[PRODUCTNAME]]の起動と停止

CMSコンテナを`docker-compose up -d`で初期化したら、基盤となるコンテナを削除せずに起動・停止することができます。

`stop`コマンドを実行すると、稼働中のXibo CMSサービスが停止する。再度起動させたい場合は、`start`コマンドを発行します。

```
docker-compose start
docker-compose start
```

## アンインストール/削除
CMSのインストールと関連するコンテナは、'down'コマンドを実行することで完全に削除することができます。

**docker-compose downを実行する前に**、メディアとデータベースのファイルが`共有`ディレクトリに正しく書き込まれていることを確認してください。

- そのためには、例えば画像をCMSにアップロードし、同じ画像が`shared/cms/library`ディレクトリに表示されることを確認します。
- また、`shared/backup/db/latest.tar.gz`が過去24時間以内に作成されているかどうかも確認することをお勧めします。

これらのチェックのいずれかに失敗した場合、データの損失につながるため、`docker-compose down`を実行しないでください。サポートに問い合わせてください。

[[PRODUCTNAME]]の痕跡をすべて削除したい場合は、`down`コマンドを**実行した後**、`/opt/xibo`フォルダを削除してください。

```
docker-compose down
```

##コミュニティリソース

- [Ubuntu 18.04でDockerを使ったXibo CMSの構築(英語)](https://community.xibo.org.uk/t/xibo-cms-with-docker-on-ubuntu-18-04/9392)
- [Windows 10 64bit(英語)](https://community.xibo.org.uk/t/xibo-for-docker-on-windows-10-64-bit/16298)
- [Windows 10 以外の Windows 64bit(英語)](https://community.xibo.org.uk/t/xibo-for-docker-on-windows-64bit-other-than-windows-10/16300/2)

## CMS/データベースのバックアップ
ユーザーデータを含むシステムと同様に、Xibo CMSの定期的なバックアップを維持することは非常に重要です。

Dockerをインストールすると、毎日のデータベースのバックアップと、アップグレードのためのバックアップが自動的に行われます。

- バックアップ計画の一環として、少なくとも以下のファイル/ディレクトリのバックアップを定期的に取る必要があります。
```
shared/backup/db/latest.sql.gz
shared/cms
config.env
*.yml
```

## カスタムスクリプトとページ
Dockerが提供する環境を使用して、カスタム開発、テーマ、モジュール、またはスタンドアロンファイルを実行したい場合、`/shared`フォルダを使用して実行することができます。

以下の場所が利用可能です。

- `/shared/cms/custom` : カスタムミドルウェアに使用します。
- `/shared/cms/web/theme/custom` : カスタムテーマに使用します。
- `/shared/cms/web/userscripts` : スタンドアロンファイルとして使用します。`http://localhost/userscripts` として提供されます。

## その他の共有フォルダ
Dockerでは、ライブラリやバックアップも共有フォルダとして利用できるようになっています。

- `/shared/backup` :
- `/shared/cms/library`

## ライブラリと帯域の制限
ライブラリのファイルサイズと月間の帯域幅使用量の制限を管理します。

- これは、データベース・テーブルの2つの設定（LIBRARY_SIZE_LIMIT_KB & MONTHLY_XMDS_TRANSFER_LIMIT_KB） によって行われます。
データベースに制限が入力されていない場合は、すべて無制限に動作しますが、制限が入力されると、Xiboはこれらの制限に対する検証を開始し、制限を超えるとプレーヤーへの新しいメディアや更新のアップロードを阻止するようになります。

ただし、この2つの設定は、現在のところユーザーインターフェースからは利用できず、統計データのみが表示されます。

{tip}
[ディスプレイ](/manual/ja/displays.html)が消費できる帯域幅の制限を適用することもできます。
{/tip}
