<!--toc=android_install-->

# プレイヤー設定

**Xibo for Android**には、設定可能なオプションが多数あり、主なオプションを以下に示します。

**注意：**オプションは[ディスプレイプロファイル](/manual/ja/displays_settings.html)からアクセスできます（特に記載がない場合、またはローカルのみと表示されている場合を除く）。

## コード/電子メール・アドレス

ライセンスを消費する権限を持つライセンスプールのコード/電子メールアドレスです。これは、プロファイルの編集フォームの**一般**タブに表示されます。**Xibo for Android** の 14 日間の試用期間終了後、継続して使用するために必要です。

## CMSアドレス

[[PRODUCTNAME]] CMSのアドレスで、デザインしたレイアウトをこのプレイヤーに**スケジューリング**できるようにするためのものです。インストール時に「Connect to CMS」をクリックし、フォームフィールドに入力することで指定できます。

{tip}
**注意：**このオプションはプレイヤー側の設定であり、CMS側の設定ではないことに注意してください。
{/tip}

## CMSシークレットキー

**[[PRODUCTNAME]] CMS**に固有のシークレットキーです。Xibo CMSの管理者であれば、CMSメニューの管理セクションにある**設定**ページで確認することができます。管理者でないユーザーは、システム管理者にお問い合わせください。

## ディスプレイ名

CMSメニューの**ディスプレイセクション**にある、内部識別のためのディスプレイの名前です。この名前は、CMSでコンテンツを**スケジューリング**したり、**ディスプレイステータス**をチェックする際にディスプレイを参照するために使用されます。

## ローカルライブラリの保存オプション

**Xibo for Android**は、CMSからファイルやリソースをダウンロードし、デフォルトで`内部ストレージ`に保存します。しかし、サイネージプレイヤーは、`外部ストレージ`オプションを使用して、SDカード上で実行したい場合がよくあります。

{tip}
**注意：**このオプションは、CMSからではなく、プレイヤーの設定から構成されます。
{/tip}

## パスワード設定

設定画面は、プロファイル編集フォームの**一般**タブにある**パスワード保護**設定フィールドにパスワードを入力することにより、パスワードで保護することができます。

## 収集間隔

収集間隔は、クライアントが新しいコンテンツをチェックする頻度を定義します。ほとんどのユースケースでは、収集間隔を5分にすることをお勧めします。この設定は、**プロファイルの編集**フォームの**一般**タブに表示されます。

## ハードウェアキー

プレイヤーに固有のキーで、インストール時に自動的に生成されます。通常の場合、Hardware Keyを調整する必要はありません。ローカルのみ。

## スプラッシュを置き換える

オプションで、スプラッシュ画面をデバイスのフォトギャラリーの画像に置き換えることができます。**ローカルのみ**。

## オリエンテーション

プロフィール編集フォームの**位置**タブで、縦向き／横向きオプションを選択します。デバイスが選択したオリエンテーションをサポートしていることを確認する必要があります。

## 起動時に開始しますか

プロファイルの編集フォームの**詳細**タブにあるチェックボックスで、デバイスの起動中にXiboを起動するかどうかを選択します。このチェックボックスをオンにすると、停電の際、デバイスが再起動したときにXiboが起動するようになります。

## アクションバーモード

アクションバーはデバイスの上部に表示され、Xiboのメニュー項目とロゴが表示されます。アクションバーは、アプリケーション起動時に一度だけ表示された後、非表示にしたり、時間指定したり、ユーザーの操作の後に表示させたりすることができます。プロフィール編集フォームの詳細設定タブで選択します。

## アクションバーの表示時間

アクションバーを表示する時間を設定します。

## 自動再起動

クラッシュしたときにXiboを再起動させるかどうかをチェックボックスで選択します。

## 開始遅延

デバイスの再起動後にXiboが起動するまでの待ち時間（起動時に起動するように設定されている場合）を選択します。

## ログレベル
デバッグのために、Xiboは大規模なログを生成することができます。プレイヤーが記録する**ログレベル**は、プロファイル編集フォームのトラブルシューティングタブから選択できます。
