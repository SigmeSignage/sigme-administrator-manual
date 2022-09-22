<!--toc=cms_config-->

# SAML認証プロバイダとしての SAML

CMS は、認証プロバイダとして SAML を使用するように設定することができます。

{tip}
Security Assertion Markup Language (SAML、発音はサムエル) は、XMLベースのオープンスタンダードなデータ形式で、当事者間、特にIDプロバイダとサービスプロバイダ間で認証と承認のデータを交換するためのものである。(Wikipediaより)
{/tip}

SAMLの統合は、CMSのインストール時に'settings.php`ファイルを介して有効になります。Dockerを使用している場合、`settings.php`にはアクセスできませんが、`/custom`マウントポイントに`settings-custom.php`ファイルを作成することが可能です。そのファイルに以下の設定を追加することで代用できます。

この統合の目的は、Xibo CMS で認証するための SAML 対応 IdP (identity provider) を設定することです。

すでに IdP で認証されている User は、自動的に CMS にログインするようになります。ユーザが存在しない場合は、デフォルトの認証情報で作成されます。

## 設定
SAML インテグレーションは、CMS のインストール時に `settings.php` ファイルで設定します。このファイルは、`/web` フォルダにあります。

調整する部分は `$authentication` ミドルウェアと `$samlSettings` 設定配列のふたつです。

### ミドルウェア
認証用ミドルウェアを`SAMLAuthentication`に変更します。

```
$authentication = new \Xibo\Middleware\SAMLAuthentication();
```

### SAML 設定
SAML 設定配列には、CMS が SAML 対応の IdP に接続し、それを使用するために必要なすべての情報が含まれています。この設定は、4 つの主要なセクションに分かれています。

- `idp`: ID プロバイダのオプション (CMS が ID プロバイダを特定し、通信するために使用します)。これらの設定値は、IdPに記載されています。
- `sp`：サービスプロバイダのオプション（これらはCMSからIDプロバイダに送信され、IDPがCMSに通信できるようにするものです）。これらの設定には、ターゲット・インストールのCMS URLを使用する必要があります。
- `security`：IdP が要求するセキュリティを有効にするためのオプション。
- `workflow`：CMS固有のオプションで、IdPからのデータを[[PRODUCTNAME]]のデータにどのようにマッピングするかを決定します。このセクションは、IdPとCMSの間のマッピングを決定するために使用されます。シングルログアウトが無効な場合、CMSで「ログアウト」を選択しても、次のリクエストでユーザーはすぐに再ログインされるため、何の効果もありません。


### ジャストインタイムプロビジョニング(JIT)
ワークフローセクションで、ジャストインタイムプロビジョニングを有効にすることができます。JIT プロビジョニングでは、CMS にアクセスした、現在アカウントを持たない有効な User が自動的に作成されます。

JITを使用する場合、CMSでUserを作成するためにどのような情報を使用するかを定義する必要があります。

IdPが属性を提供しない場合、`workflow`設定の`mapping`プロパティを除外することが重要です。

### 設定例

```
$samlSettings = array (
   'workflow' => array(
        // Enable/Disable Just-In-Time provisioning
        'jit' => true,
        // Attribute to identify the user 
        // if set to nameId then the NameID from SAML will be taken and used as the
        // username in Xibo.
        'field_to_identify' => 'UserName',   // Alternatives: UserID, UserName, email
        // Default libraryQuota assigned to the created user by JIT
        'libraryQuota' => 1000,
        // Initial User Group
        'group' => 'Users',
        // Home Page
        'homePage' => 'icondashboard.view',
        // Enable/Disable Single Logout
        'slo' => true,
        // Attribute mapping between XIBO-CMS and the IdP
        'mapping' => array (
            'UserID' => '',
            'usertypeid' => '',
            'UserName' => 'uid',
            'email' => 'mail',
            'ref1' => '',
            'ref2' => '',
            'ref3' => '',
            'ref4' => '',
            'ref5' => ''
        )
    ),
   // Configure the IdP and SP
   'strict' => false,
   'debug' => true,
   'idp' => array (
            'entityId' => 'https://idp.example.com/simplesaml/saml2/idp/metadata.php',
            'singleSignOnService' => array (
                'url' => 'http://idp.example.com/simplesaml/saml2/idp/SSOService.php',
            ),
            'singleLogoutService' => array (
                'url' => 'http://idp.example.com/simplesaml/saml2/idp/SingleLogoutService.php',
            ),
            'x509cert' => 'MIICbDCCAdWgAwIBAgIBADANBgkqhkiG9w0BAQ0FADBTMQswCQYDVQQGEwJ1czETMBEGA1UECAwKQ2FsaWZvcm5pYTEVMBMGA1UECgwMT25lbG9naW4gSW5jMRgwFgYDVQQDDA9pZHAuZXhhbXBsZS5jb20wHhcNMTQwOTIzMTIyNDA4WhcNNDIwMjA4MTIyNDA4WjBTMQswCQYDVQQGEwJ1czETMBEGA1UECAwKQ2FsaWZvcm5pYTEVMBMGA1UECgwMT25lbG9naW4gSW5jMRgwFgYDVQQDDA9pZHAuZXhhbXBsZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAOWA+YHU7cvPOrBOfxCscsYTJB+kH3MaA9BFrSHFS+KcR6cw7oPSktIJxUgvDpQbtfNcOkE/tuOPBDoech7AXfvH6d7Bw7xtW8PPJ2mB5Hn/HGW2roYhxmfh3tR5SdwN6i4ERVF8eLkvwCHsNQyK2Ref0DAJvpBNZMHCpS24916/AgMBAAGjUDBOMB0GA1UdDgQWBBQ77/qVeiigfhYDITplCNtJKZTM8DAfBgNVHSMEGDAWgBQ77/qVeiigfhYDITplCNtJKZTM8DAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBDQUAA4GBAJO2j/1uO80E5C2PM6Fk9mzerrbkxl7AZ/mvlbOn+sNZE+VZ1AntYuG8ekbJpJtG1YfRfc7EA9mEtqvv4dhv7zBy4nK49OR+KpIBjItWB5kYvrqMLKBa32sMbgqqUqeF1ENXKjpvLSuPdfGJZA3dNa/+Dyb8GGqWe707zLyc5F8m',
        ),
   'sp' => array (
        'entityId' => 'http://xibo-cms.example.com/saml/metadata',
        'assertionConsumerService' => array (
            'url' => 'http://xibo-cms.example.com/saml/acs',
        ),
        'singleLogoutService' => array (
            'url' => 'http://xibo-cms.example.com/saml/sls',
        ),
        'NameIDFormat' => 'urn:oasis:names:tc:SAML:2.0:nameid-format:emailAddress',
        'x509cert' => '',
        'privateKey' > '',
    ),
    'security' => array (
        'nameIdEncrypted' => false,
        'authnRequestsSigned' => false,
        'logoutRequestSigned' => false,
        'logoutResponseSigned' => false,
        'signMetadata' => false,
        'wantMessagesSigned' => false,
        'wantAssertionsSigned' => false,
        'wantAssertionsEncrypted' => false,
        'wantNameIdEncrypted' => false,
    )
);
```

SAML 固有の設定についての詳しい説明は [GitHub - onelogin/php-saml](https://github.com/onelogin/php-saml#settings) で見ることができます。PHP 用のシンプルな SAML ツールキット。
