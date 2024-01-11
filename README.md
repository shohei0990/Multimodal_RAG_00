## 概要
https://zenn.dev/harappa80/articles/multimodal_rag

基本的な内容はこちらの記事に沿ったコードを環境依存なく誰でも試せるようにGoogle colobの内容で作成したものになります。また jupyterNotebook(ipynbファイル)でも作成も合わせて行いました。

※　GCPのVertexAI APIの利用手順も追加しました。 

## Git-Hub

https://github.com/shohei0990/Multimodal_RAG_00/tree/main

sample_googlecolob.ipynbを開けば大まかな内容・手順が確認できるかと思います。


sample_googlecolob.ipynb を使う場合には google　colabを開いて、新規作成かファイルを読み込んでください。colabはcolab上でライブラリのインストールさえしてしまえばコードが動かせるので環境設定に依存せず動作確認できるので便利です。ただ、ファイルのアップロード後ランタイムによる更新などでファイルが消えたりとするので少し面倒だったりしますが、致し方なし。

※ 一度、ライブラリをインストールしたあとランタイム再起動が必要です。

https://colab.google/

## VertexAI API/Gemini APIの使用設定について
記事内だと、APIを使用する環境についての記述が特にありませんでしたので、内容補足しますと
Gemini APIの使用するためにはGCPでプロジェクト作成して下記項目を取得する必要があります。

・APIキー
・プロジェクト名
・認証キー

gemin のAPIキーの取得は2つの方法あるようですが
・Google AI StudioのGemini API
・Vertex AIでGeminiProのモデル利用

https://auto-worker.com/blog/?p=8667

今回は、VertexAIの方から行っています。上の方が簡単なようですが、結局VertaexAIのライブラリいくつか使用しているので、単にGoogle AI StudioのAPIキーの発行だけだと動きませんでした。

## Vertex AIでGeminiPro利用までの手順
◼︎ プロジェクトの作成
・　GCPログイン
・　プロジェクト作成
・　プロジェクト選択
・　ダッシュボードにて、プロジェクト名を確認・メモ

◼︎ APIの有効化
・　左上の三本線「ナビゲーションメニュー」
・「APIとサービス」
・「有効なAPIとサービス」
・「＋APIとサービスの有効化」
・「vertex ai api」検索して選択

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1820416/b7cd6ad1-ba27-9f9b-8ac8-ba063cf8ab07.png)
・「有効にする」を選択
・　有効なAPIとサービスに「VertexAI API」が追加されていることを確認
・　認証情報の作成　→ 「アプリケーションデータ」を選択

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1820416/bfb39600-b07f-cda7-e7bc-387dc6e89fca.png)


◼︎ APIキーの発行
・「認証情報」→「＋認証情報を作成」→「APIキー」選択
・　作成されたAPIキーをメモ、認証情報に追加されていることを確認

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1820416/0a3b490a-c898-c435-f928-be5a4368f53b.png)


◼︎ 同意画面の構成
・「認証情報」→ 「同意画面を構成」を選択
・「外部」選択し、作成。
・「アプリ名」「サポートメール先」「デベロッパー連絡先」
・　そのほか、そのまま特に入力せず次へで完了しました。

◼︎ サービスアカウントの作成と認証キーの取得
・「認証情報」→「＋認証情報を作成」→「サービスアカウント」選択
・　サービスアカウント名を入れて作成
・　ロールで「オーナー」を選択、続行　(一回目error出ましたがもう一回クリックしたら無くなりました。)
・　完了
・　サービスアカウントが作成されたことを確認
・「サービスアカウント」をクリック
・「キー」タブをクリック
・「鍵を追加」、「json」を選択して、jsonファイルを保存

◼︎ Generative Language API を有効化
・左側のナビゲーションメニューから「APIとサービス」→「ライブラリ」を選択
・「ライブラリ」ページで、「Generative Language API」を検索
・「Generative Language API」をクリックして詳細ページを開く
・「有効にする」ボタンをクリック

◼︎ 動作確認
こちらのコードで、動作すればAPIは有効になっていることが確認できるかと。

```
import os
import vertexai
import google.generativeai as genai

# 環境変数 GOOGLE_APPLICATION_CREDENTIALS を設定
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = "＊＊＊＊＊.json"
# プロジェクトIDを設定
vertexai.init(project='＊＊＊＊＊')
# APIキーの設定
api_key = "＊＊＊＊＊＊"
genai.configure(api_key=api_key)

for m in genai.list_models():
  if 'generateContent' in m.supported_generation_methods:
    print(m.name)
```
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1820416/00b223ea-ac7d-c148-5146-07bd0a4ab0fd.png)

```
gemini_pro = genai.GenerativeModel("gemini-pro")
prompt = "日本で、美味しい魚料理を教えてください?"
response = gemini_pro.generate_content(prompt)
print(response.text)
```
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1820416/d826fa2a-17d6-7b79-cdfb-e2a70ba835a8.png)

## 参照記事のお試し結果
「Vertex AI Gemini ProとLangChainで実現するMultimodal RAG」 の
記事通りの最終的な結果通りに行く場合もありますが出力エラーになったり、検索する画像があったり、なかったたりと少し不安定な印象です。プロンプト指示の仕方や検索の仕方の工夫でこの辺りの安定性が上がってくるやり方を見つけていきたいですね。

・読み込みのPDFとプロンプトを変更すれば、別のものでも作成することができます。画像が多すぎると途中で、errorが発生しました。うまく分割できてくると良さそうですね。


## その他 雑談
・最近、Cursorを使っているのですがvscodeの拡張機能と同じものを使うには、「表示」タブ → 「拡張機能」→ 「Jupiter」で検索すれば、jupiter codeを使用するための拡張機能がインストールできます。
