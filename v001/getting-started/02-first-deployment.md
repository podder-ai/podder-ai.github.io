# 初めてのデプロイメント
## はじめに
Podder.ai が提供しているサンプルパイプラインを使用して、初めてのデプロイを実行してみましょう。ここでは次のことを学習することができます。
- Podder CLI を使用した環境構築
- Podder.ai を使用した AI アプリケーションのデプロイ

## クラスターの構築
早速、`podder cluster` コマンドを利用してクラスターを構築しましょう。

まずは`Launch Terminal` をクリックしてください。ブラウザ上で Podder.ai を使用できる環境を利用できます。
> `Launch Terminal` ボタン

ブラウザ上のターミナルを確認すると `初めてのパイプライン実行` で使用したサンプルプロジェクトが既にインストールされています。
```
$ ls
sample-podder
```

`sample-podder` ディレクトリに移動して、次のコマンドを入力して、クラスターを構築しましょう。
```
$ cd sample-podder
$ cd podder cluster install ??
```
> クラスター構築の流れを確認する

これでクラスターの構築が完了しました。

## パイプラインのデプロイ
それでは、先ほど構築したクラスターにアプリケーションをデプロイしていきます。

> 通常は皆さんが設計したパイプラインを使ってデプロイ用のビルドを先に行い成果物を準備します。しかし、今回はテンプレートを利用しているため、既に準備してある成果物を使用します。

`sample-podder` ディレクトリ以下で、次のコマンドをブラウザのターミナルから入力してください。
```
$ podder deploy apply
```

デプロイが成功しているか確認してみましょう。STATUS が `Running` になっているとデプロイに成功しています。
```
$ kubectl get pods
NAME                          READY     STATUS    RESTARTS   AGE
hello-node-5f76cf6ccf-br9b5   1/1       Running   0          1m
```

最後にブラウザ上からアクセスできるか確認してみましょう。ブラウザ上のターミナルから IP アドレスを確認してこちらにアクセスしてください。
`https://[IP]:30000`

> ダッシュボード画面イメージ

これで **初めてのデプロイメント** は完了です。Podder.ai を使用した AI アプリケーションがどのような振る舞いをするか理解できたかと思います。次はこのアプリケーションをデプロイしていきましょう。