<!--toc=android_faqs-->

# 埋め込みYoutube動画の自動再生

YouTubeビデオの埋め込みは、埋め込みウィジェットを使用して行ってください。YouTubeの動画を埋め込んだレイアウトをXiboで公開する場合は、YouTubeの利用規約を必ずお読みいただき、ご理解の上ご利用ください。

まず、埋め込みウィジェットを作成します。

HTMLの部分に、以下のように記述してください。

```
<!-- BROWSER=edge -->
<div id="player"></div>
```

そして、スクリプトセクションに次のように記述します。

```
<script>
    var tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    var firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    var player;
    var playerState = -1;
    var readyCalled = false;
    
    function onYouTubeIframeAPIReady() {
        player = new YT.Player('player', {
          suggestedQuality: 'hd1080',
          height: '1080',
          width: '1920',
          videoId: 'onldzSzdqlM',
          events: {
            'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
          }
        });
    }
    function onPlayerReady(event) {
        readyCalled = true;
        event.target.playVideo();
    }
    function onPlayerStateChange(event) {
        if (readyCalled && playerState === 3 && event.data === -1) {
            setTimeout(function() { player.playVideo(); }, 1000);
        }
        playerState = event.data;
    }
</script>
```

上のスクリプトでは、欲しいビデオを再生するためのパラメータと、それを表示するリージョンのサイズを調整する必要があります。

そこで、以下の部分を慎重に調整します。

```
height: '720',
width: '1280',
videoId: 'onldzSzdqlM',
```

heightとwidthは、実際のディスプレイでビデオを表示するためのサイズです。videoId は、そのビデオの Youtube URL のものです。たとえば、ビデオの URL が http://www.youtube.com/watch?v=onldzSzdqlM の場合、videoId パラメータは onldzSzdqlM となります。

上記の方法を使用したレイアウトの例は、[こちら](https://xibo-firmware.fra1.cdn.digitaloceanspaces.com/Example-Layouts/export_youtube-embed-android.zip)でご覧いただけます。

また、デバイスに割り当てられたディスプレイプロファイルの「Hardware Accelerate web content」が「On」または「Off when transparent」（埋め込みウィジェットで透過性を設定しない場合）に設定されていることを確認することを強くお勧めします。

また、スクリプトに追加できるパラメータがありますので、以下を参照してください。

```
player = new YT.Player('player', {
    height: '1080',
    width: '1920',
    videoId: 'XCHMbYv70o8',
    playerVars: { 
        'playlist': 'XCHMbYv70o8',
        'loop': 1,
        'controls' : 0, 
        'rel' : 0,
        'fs' : 0,
        'showinfo' : 0,
        'cc_load_policy' : 0,
        'iv_load_policy' : 3,
        'modestbranding' : 1
    },
```

playerVars は [YouTube API Documentation](https://developers.google.com/youtube/player_parameters) で説明されているので、好きなものを選んで使ってください。videoと同じvideoIdを持つ'playlist'とloopパラメータを追加すると、そのビデオをループ再生するようになります - もちろん、もっとビデオを追加することもできます。

もちろん、もっと多くのビデオを追加することもできますが、上記は単なる例です。上記の例の他のプレーヤーバーは、ほとんどが、埋め込まれた YouTube プレーヤーから不要な情報、キャプション、コントロールなどを削除するためのものです。

### プレイリスト

プレイリストの自動再生とループ。

Youtube iframe API を使用してこれを実現する方法はいくつかありますが、ここでは `loadPlaylist()` の使用法に焦点を当てます。

次のコードは、プレーヤを埋め込み、提供された videoIds をプレイリストにロードし、すべての動画を順番に表示し、プレイリストを最初から再開してループさせるものです。

```
<script>
  function onPlayerReady(event) {
    event.target.loadPlaylist(['a329D581TAw','XCHMbYv70o8','wzQesXhFt_s']);
    event.target.setLoop(true);
  }
</script>
```

YouTube 上の既存のプレイリストのプレイリスト ID を使用するには、loadPlaylist() を呼び出す際にオブジェクト構文を使用する必要があります。
つまり、上記の例では、loadPlaylist の行を次のように置き換える必要があります。

```
event.target.loadPlaylist({list:'PLYgi4FbOVUSzsP627x-PKTHjoS13_fSJE', listType: 'playlist'});
```


