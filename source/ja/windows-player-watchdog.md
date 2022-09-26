<!--toc=windows_install-->

# Windowsプレーヤーウォッチドッグ
ウォッチドッグは、メインアプリケーションの安定性を監視し、必要に応じて再起動するために使用することができるオプションのコンポーネントです。

ウォッチドッグは、Windowsプレーヤーのインストール時に、Windowsプレーヤーと同じフォルダにインストールされます。

## 設定ファイル
設定は、watchdogのインストールフォルダにある設定ファイルで管理されます。

ファイルの内容は以下の通りです。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <configSections>
        <sectionGroup name="applicationSettings" type="System.Configuration.ApplicationSettingsGroup, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" >
            <section name="XiboClientWatchdog.Properties.Settings" type="System.Configuration.ClientSettingsSection, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
        </sectionGroup>
    </configSections>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
    </startup>
    <applicationSettings>
        <XiboClientWatchdog.Properties.Settings>
            <setting name="ClientLibrary" serializeAs="String">
                <value>C:\Users\username\Documents\Xibo Library</value>
            </setting>
            <setting name="PollingInterval" serializeAs="String">
                <value>120</value>
            </setting>
            <setting name="Threshold" serializeAs="String">
                <value>300</value>
            </setting>
            <setting name="ProcessPath" serializeAs="String">
                <value>C:\Program Files\Xibo\XiboClient.exe</value>
            </setting>
            <setting name="LogFileName" serializeAs="String">
                <value>log.xml</value>
            </setting>
        </XiboClientWatchdog.Properties.Settings>
    </applicationSettings>
</configuration>
```

上記の設定ファイルの中で重要なのは、`<applicationSettings>`要素間の設定です。

### クライアントライブラリ
プレーヤーで設定したライブラリの場所です。

### ポーリング間隔
ウォッチドッグがアプリケーションのアクティビティをチェックする頻度。

### しきい値
ウォッチドッグが再起動する前にアプリケーションが非アクティブでなければならない秒数の閾値。

### プロセスパス
監視するアプリケーションEXEの完全修飾パス名。これは、アプリケーションが非アクティブであることが検出されたときに、アプリケーションを再起動するために使用されます。

### ログファイル名
監視対象のプレーヤーで設定されたログファイル名です。ダウンタイムイベントと再起動は、通常のエラーログの末尾に追加されます。

## Windowsで起動
ウォッチドッグEXEへのショートカットを作成し、スタートメニューのWindowsスタートアッププログラムグループに追加する必要があります。
