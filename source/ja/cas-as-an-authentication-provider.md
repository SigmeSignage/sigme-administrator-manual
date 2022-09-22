<!--toc=cms_config-->

# CAS認証プロバイダ

CMSは、認証プロバイダとしてCASを使用するように設定することができます。

{tip}
Central Authentication Service（CAS）は、Web用のシングルサインオンプロトコルです。その目的は、ユーザーが複数のアプリケーションにアクセスする際に、ユーザーIDやパスワードなどの認証情報を一度だけ提供することを可能にすることです。また、Webアプリケーションはパスワードなどのセキュリティ情報を得ることなくユーザーを認証することができます。(Wikipediaより）
{/tip}

CASの統合は、CMSのインストール時に`settings.php`ファイルを介して有効になります。Dockerを使用している場合、`settings.php`にはアクセスできませんが、`/custom`マウントポイントに`settings-custom.php`ファイルを作成することが可能です。そのファイルに以下の設定を追加することで代用できます。

この統合の目的は、[[PRODUCTNAME]] CMS で認証するための CAS 対応 IdP（identity provider）を設定することです。

IdPで認証されたユーザーは、自動的にCMSにログインすることができます。ユーザが存在しない場合は、デフォルトの認証情報で作成されます。

## 設定

CASとの連携は、CMSのインストール時に`settings.php`ファイルで設定します。このファイルは、`/web` フォルダにあります。

調整する部分は、'$authentication' ミドルウェアと '$samlSettings` 設定配列の2つです。


## ミドルウェア

認証ミドルウェアを CASAuthentication に変更します。

```
$authentication = new \Xibo\Middleware\CASAuthentication();
```

## CASの設定
CAS設定配列には、CMSがCAS対応IdPに接続し、利用するために必要な情報がすべて含まれています。設定は、主に3つのセクションに分かれています。

- `server` : サーバプロバイダのオプション（CMS が ID プロバイダを特定し、通信するために使用します）。
- `port` : 使用するポートを指定します。
- `uri` : 上記で指定したサーバーのCASアプリケーションの場所。

### 設定例

```
$authentication = new \Xibo\Middleware\CASAuthentication();
$casSettings = array(
    'config' => array (
        'server' => 'your.cas.server',
        'port' => '443',
        'uri' => '/cas'
    )
);
```
