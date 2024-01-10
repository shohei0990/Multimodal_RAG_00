## 概要
https://zenn.dev/harappa80/articles/multimodal_rag

基本的な内容はこちらの記事に沿ったコードを環境依存なく誰でも試せるようにGoogle colobの内容で作成したものになります。またjupyter(ipynbファイル)でも作成も合わせて行いました。

## Git-Hub

https://github.com/shohei0990/Multimodal_RAG_00/tree/main

sample_googlecolob.ipynbを開けば大まかな内容・手順が確認できるかと思います。



googlecolabを開いて、新規作成かファイルを読み込んでください。colabはcolab上でライブラリのインストールさえしてしまえばコードが動かせるので環境設定に依存せず動作確認できるので便利ですね。ただ、ファイルのアップロード後ランタイムによる更新などでファイルが消えたりとするので少し面倒だったりします。

https://colab.google/

## Gemini APIの使用設定について
記事内だと、APIを使用する環境についての記述がありませんでしたので補足しますと
Gemini APIの使用するためにはGCPでプロジェクト作成して下記項目を取得する必要があります。

・APIキー
・プロジェクト名
・認証キー

gemin のAPIキーの取得は2つの方法あるようですが
・Google AI StudioのGemini API
・Vertex AIでGeminiProのモデル利用

https://auto-worker.com/blog/?p=8667

今回はVertexAIの方から行っています。上の方が簡単なようです。

## 結果
・記事通りの最終的な結果通りに行く場合もありますが出力エラーになったり、検索する画像があったり、なかったたりと少し不安定な印象です。プロンプト指示の仕方や検索の仕方の工夫でこの辺りの安定性が上がってくるやり方を見つけていきたいですね。

・読み込みのPDFとプロンプトを変更すれば、別のものでも作成することができます。画像が多すぎると途中で、errorが発生しました。うまく分割できてくると良さそうですね。
