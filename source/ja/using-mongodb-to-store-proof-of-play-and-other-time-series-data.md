<!--toc=cms_config-->

# MongoDBを使って時系列データを保存する

CMSはMongoDBを使用して実行証明統計やその他の時系列データを保存するよう設定することができます。

MongoDBは、CMSのインストール時に`settings.php`ファイルで有効にします。Dockerを使用している場合、`settings.php`はアクセスできませんが、`settings-custom.php`ファイルを`/custom` マウントポイントに作成することが可能です。そのファイルに以下の設定を追加してください。

## 設定

ユーザーとデータベースが作成された MongoDB インスタンスが動作している必要があります。

設定ファイルに `$timeSeriesStore` という新しい変数を追加します。この変数は、MongoDB接続の詳細で作成された `\MongoDbTimeSeriesStore` を返す関数である必要があります。

```
$timeSeriesStore = function() {
    return new \Xibo\Storage\MongoDbTimeSeriesStore([
        'host' => 'mongo',
        'port' => '1234',
        'database' => 'db_name',
        'username' => 'user',
        'password' => 'password'
    ]);
};
```

## Mongo インデックス

MongoDB データベースにある `stat` コレクションにインデックスを追加することをおすすめします。これをコンソールから行うには、データベースを選択していることを確認してから以下のコマンドを実行します。

```
db.stat.ensureIndex({ "start": -1, "end" : -1, "type": 1}, {background: true}) 
```

## 移行
CMS を新規にインストールしない限り、既存の実行証明データを制御された方法で MongoDB に移行したいと思うことでしょう。

[[PRODUCTNAME]]にはこれを実現するための移行タスクが用意されており、CMSのメインメニューの**管理**セクションにある**タスク**をクリックすることで有効にすることができます。

このタスクにはデフォルトのオプションが用意されていますので、ご自分の環境に合わせて調整してください。デフォルトのオプションは、既存データの移行をゆっくりと着実に行うことを意図しています。

### 利用可能なオプション

- killSwitch (0または1で処理を完全に無効化)
- numberOfRecords (一回に処理するレコード数)
- numberOfLoops (1回のタスク実行で何回ループさせるか)
- pauseBetweenLoops (各ループの間にどれだけの時間待機するか)
- optimiseOnComplete (切り詰めた後にOPTIMIZE TABLEスタットを発行する)
- configOverride (上記のすべての設定のオーバーライドを含むJSONファイルへのパス、オプション)

StatsArchiverタスクを既に使用している場合、競合を防ぐために移行中は無効化されます。

移行が完了するまで、MySQLデータベースのstatテーブルからは何も削除されません。
