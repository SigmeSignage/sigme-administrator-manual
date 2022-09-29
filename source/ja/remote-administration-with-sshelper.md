<!--toc=android_install-->

# SSHelperを使ったリモート管理

Android端末を再起動する必要がある場合があります。端末に直接アクセスできない場合でも、シェルコマンドを使えば、リモートでこれを行うことができます。シェルコマンドは、システムの電源管理オプションを制御し、レイアウトのアクティビティに基づいて外部コマンドを実行する機能を追加します。これにより、プレーヤー側の電源オプションに大きな柔軟性を持たせることができます。

{tip}
詳細については、[コマンド機能](/manual/ja/displays_command_functionality.html)と[シェルコマンドモジュール](/manual/ja/media_module_shellcommand.html)のページを参照してください。
{/tip}

root化したAndroid端末のSSHelperアプリケーションでシェルコマンドを制御・使用することが可能です。設定方法については、[端末のルート化]()のページを参照してください。

## SSHelperでリモート管理

Android端末のSSHelperを設定すると、端末をリモートで再起動したり、その他の便利なオプションが利用できるようになります。

以下の手順に従って、CMSとリモートアクセスポイント間の接続を設定します。

ルート化された Android デバイスに SSHelper アプリケーションをインストールします - SSHelper. Android端末にSSHクライアント（例：PuTTY）で接続し、以下の情報を入力します。

- Android端末のIPアドレス
- ポート2222
- パスワード "admin"

コマンドラインに "su" と入力し、Android の画面で accept をクリックします (SuperUser 権限)。

### SSHelperアプリケーション

- 設定メニューで「run SSHelper service at boot」を有効にし、それ以下のオプションはすべて無効にします。
- パスワードの表示」を有効にし、パスワードとポートを任意に変更します。
- 右上のメニューから、SSHサーバとアプリケーションを再起動します。

これで、あなたのAndroid端末にSSHelperが正しくインストールされました。

{tip}
セキュリティ上の理由から、**公開鍵認証**の使用をお勧めします。
{/tip}

{tip}
設定方法は、[SSHelperのウェブサイト](https://arachnoid.com/android/SSHelper/index.html)の公開鍵（パスワードなし）ログインの項に詳しく書かれています。
{/tip}

これで、新しい設定を使って、Android端末にPuTTYで再接続できるようになりました。

### Xiboで使える便利な追加コマンド
- "ps | grep xibo" - Xibo プロセスが実行されているかどうかを確認します。
- "am force-stop uk.org.xibo.client" - Xibo プロセスを停止する。
- "am start uk.org.xibo.client/uk.org.xibo.player.Player" - Xibo プロセスを開始します。
- "reboot" - デバイスを再起動する。

## 代替手段
Android端末にリモートアクセスする方法として、以下のようなものがあります。[シェルコマンド](restart-rooted-device-with-a-shell-command.html) VNCサーバー TeamViewer
