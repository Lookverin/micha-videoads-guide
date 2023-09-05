# Michao SSP Ad Solution 仕様書

Michao SSP Ad Solutionは3つの広告フォーマットをサポートしています：リワード広告、バナー広告、ワイプアドです。以下ではそれぞれの広告フォーマットの概要と実装方法を説明します。

## 最初に

Michao!!と接続した際にはメールでコードとplacement_idが送られてきますが、placement_idはリワード広告のみで使用されます。また、配布されたコードの一つ目のScriptタグ(<script async src="https://serving.lookverin.com/player/adlook.js"></script>)はheadに埋め込んでください。

## バナー広告

- 対応アスペクト比：16:9、4:3、1:1、9:16
- 呼び出し方法：指定した要素内に以下のコードを埋め込む
`
<div class="banner">
  <script data-playerPro="current">(function(){var s=document.querySelector('script[data-playerPro="current"]');s.removeAttribute("data-playerPro");(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();</script>
</div>
`

- 複数のバナー広告を呼び出す場合、同じコードを複数の要素に埋め込むことができます。
- 画面外に出た際の挙動をカスタマイズし、スティッキー広告を設定することも可能です。

## ワイプアド

- 実装方法: body内の好きな位置に以下のコードを埋め込む

<body>
  <div class="container">///</div>
  <script data-playerPro="current">(function(){var s=document.querySelector('script[data-playerPro="current"]');s.removeAttribute("data-playerPro");(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();</script>
</body>

## リワード広告

- 呼び出し方法: 広告再生ボタンのクリックなどのタイミングで以下のコードを発火

`(playerPro=window.playerPro||[]).push({id:"placement_id",after:s});})();`

- 動画広告の再生終了時やスキップ時にコールバックを設定可能です。以下は例です。

`
(playerPro=window.playerPro||[]).push({
  id:"placement_id", // プレースメントID
  after:s,  
  init: (api) => {
    api.on('AdVideoComplete', function(){
      // 動画広告再生終了時の処理
      // 報酬処理（例：ページ遷移やポイント付与）
    });
    api.on('AdVideoSkip', function(){
      // 広告スキップ時の処理
      // 報酬処理（例：ページ遷移やポイント付与）
    });
  }
});
`

リワード広告にはほかにも様々なコールバック(イベント)が用意されています。詳しくはWikiをご覧ください。

## 例




