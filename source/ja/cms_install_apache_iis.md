<!--toc=cms_install-->

# Apache/IISインストール

Dockerを使わずに[[PRODUCTNAME]]を手動/カスタムインストールするには、Webサーバーのインストール、設定、保守の方法について知識が必要です。

{tip}
[[PRODUCTNAME]]は、さまざまな環境、Webサーバー、ソフトウェアバージョンで動作させることができます。[[PRODUCTNAME]]チームが得意とする[サポートバージョンと環境](intro_version_environment.html)のリストがあります。このリスト以外の環境でのインストールを選択した場合、問題が発生した場合はコミュニティのサポートに頼ることになります。[ [[PRODUCTNAME]]コミュニティフォーラム](https://community.xibo.org.uk/)
{/tip}

## 最小要件

- MySQL 5.6
- ウェブサーバー（nginx、apache、iis)
- PHP 7.2.9+ (下記のPHPバージョンを参照)
- PHP-CLI 7.2.9+ (下記のPHPバージョンを参照)
- URLリライト
- バーチャルホストまたは専用Webサーバ（DocumentRootの変更）
- CRON/スケジュールタスク

### PHPのバージョン
- PHP 7.2.9以降が必要です。

### PHPモジュール。
- PHAR
- JSON
- GD
- DOM
- PDO
- PDO-MySQL
- Zip
- gettext
- Soap
- Curl
- Iconv
- Ctype
- File Info
- XML
- SimpleXML
- Mbstring
- zlib
- ZeroMQ

## 環境の準備
Dockerを使わずに動かすには、[[PRODUCTNAME]]が動作するように環境を設定する必要があります。ファイルをどこに置くか、どのウェブサーバーを動かすかなど、考慮すべきことがいくつかあります。

{tip}
このドキュメントでは、考えられるすべての環境に関する考察を網羅することはできませんが、以下のセクションで、よくある問題を取り上げます。
{/tip}

## CMSファイルの配置
CMSは安全な設計のため、ウェブサーバーが提供する場所にウェブフォルダーのみを置くことを意図したフォルダー構造になっています。つまり、ウェブサーバーやホスティングは、ファイルをウェブ以外の場所に配置することを許可しなければなりません。

これを実現するために、いくつかの戦略があります（これは完全なリストではありません）。

### 1. DocumentRoot を変更する
ドキュメントルートが/var/wwwの場合、CMSをそのフォルダにコピーし、ドキュメントルートを/var/www/webに変更します。

### 2. シンボリックリンクを使用する
ドキュメントルートが/var/wwwの場合、CMSを別のフォルダ（例えば/home/user/xibo-cms）にコピーし、/home/user/xibo-cms/webと/var/www/webの間にリンクを作成する。home/user/xibo-cms/web の所有者を www-data （または Web サーバが動作しているユーザ）に変更します。

###3.バーチャルホストの利用
バーチャルホストを利用することもできます。

```
<VirtualHost *:80>
    DocumentRoot "/var/www/web"
    ServerName www.example.com

    # Other directives here
    AllowOverride All
    Options Indexes FollowSymLinks MultiViews
    Order allow,deny
    Allow from all
    Require all granted
</VirtualHost>
```
リリースアーカイブの /web フォルダを指し、.htaccess ファイルを有効にする Virtual Host の設定であれば、どのような設定でもかまいません。

### 4. エイリアスを使用する
CMSはエイリアスで実行することができます。これはApacheの例です。

```
Alias /[[PRODUCTNAME]] "/home/user/[[PRODUCTNAME]]-cms/web"

<Directory "/home/user/[[PRODUCTNAME]]-cms/web">
    AllowOverride All
    Options Indexes FollowSymLinks MultiViews
    Order allow,deny
    Allow from all
    Require all granted
</Directory>
```

## URLリライト
CMSはURLの書き換えをサポートするウェブサーバー上で動作する必要があります。それができない場合は、URLにindex.phpを指定してアプリケーションにアクセスする必要があります。

## Apache
.htaccessファイルがweb/.htaccessに用意されています。このファイルは、CMSがウェブサーバのドキュメントルートまたはバーチャルホストから提供されていることを想定しています。

## エイリアスを使った書き換えルール
エイリアスが必要な場合は、エイリアスに一致する RewriteBase ディレクティブを含むように .htaccess ファイルを修正する必要があります。

例えば、エイリアスが /[[PRODUCTNAME]] である場合、.htaccess は以下のようになります。RewriteBase /[[PRODUCTNAME]] .

## nginx
nginxの設定例を以下に示します。

```
RewriteBase /xibo .

nginx
A sample nginx config is provided below:

location / {
    try_files $uri /index.php?$args;
}

location /api/authorize {
    try_files $uri /api/authorize/index.php?args;
}

location /api {
    try_files $uri /api/index.php?$args;
}

location /install {
    try_files $uri /install/index.php?$args;
}

location /maint {
    try_files $uri /maint/index.php?$args;
}

location /maintenance {
    try_files $uri /index.php?$args;
}
```
