# パイプラインの設計
## はじめに
Podder.ai を使う上で最も重要なパイプラインの設計を行います。このチュートリアルが完了するころには、皆さんの AI モデルをベースにパイプラインを設計して、アプリケーションの実行ができるようになっています。
- Podder CLI の基本的な使い方
- パイプラインの設計

## インストール
まずは Podder.ai を利用するために必要なツール`Podder CLI`をインストールしましょう。ターミナルを開いて以下のコマンドを入力してください。
```
$ pip install podder-cli
```
> `podder-cli` は、Podder.ai のコマンドラインインターフェースです。

## プロジェクトの作成
それでは、`podder init`コマンドを使用してプロジェクトを作成しましょう。
```
$ podder init tutorial
```
チュートリアル用プロジェクトが作成されたのが確認できると思います。プロジェクトの中身を確認してみると、次のようなディレクトリ構成になっています。
```
$ cd tutorial
$ tree -L 1
.
├── README.md
├── config        # Podder.ai の設定ファイル置き場
├── sensor-task   # インプットデータを取得するタスク
├── podder-task   # AI を実行するタスク
├── writer-task   # アウトプットデータを出力するタスク
├── input         # インプットデータ置き場
├── output        # アウトプットデータ置き場
└── tmp           # 実行時のワーキングデータ置き場
```
config フォルダに Podder.ai の設定ファイルが集約されていて、パイプライン設定ファイルもここに配置されています。


また、デフォルトで以下の 3 つのタスクが準備されています。sensor-task と writer-task はデフォルト設定では、input、outputディレクトリにあるデータを扱います。
- sensor-task：インプットデータを取得するタスク
- podder-task：AI を実行するタスク
- writer-task：アウトプットデータを出力するタスク

> 実際にアプリケーションを動かすときは、sensor-task、writer-task の設定を変更することで入出力先の変更が可能です。

## タスクの実行
それでは AI 処理をタスクに実装していきます。`podder-task/app/task.py`に実際の処理をコーディングしていきます。（デフォルトでは特に何も実装されていません。）

今回は Podder.ai が提供してあるモジュールを使っていきます。次のように`task.py`を修正してください。
1. OCR モジュールをインポート
2. インプットデータに対して OCR を実行
3. 結果をアウトプットデータとして返す

```
# app.task.py
・・・
# 1. Import OCR module
from podder_lib.ocr import OCR

@logger.class_logger
class Task(BaseTask):
    ・・・
    def execute(self, input_data: Any) -> Any:
        ・・・
        self.logger.debug("Start executing...")
        self.logger.debug("input_data: {}".format(input_data))

        # 2. Apply OCR to input data
        data_path = input_data["path"]
        result = OCR().execute(data_path)

        # 3. Put OCR result
        input_data["result"] = result

        output_data = input_data
        self.logger.debug("outputs: {}".format(output_data))
        self.logger.debug("Complete executing.")
        return output_data
```
これでタスクの実装が完了したので、実行して期待する動作になるか確認してみましょう。次のコマンドを`podder-task`ディラクトリで実行してください。
```
$ cd podder-task
$ podder task run
```
OCRの結果が`podder-task/output`フォルダに出力されて、タスクが成功していることが確認できます。
> `podder-task/input`フォルダにあるデータがインプットデータとして利用されています。

## パイプラインの構築
次にこのタスクを使用したパイプラインを構築してましょう。パイプラインの設計は YAML ファイルで記載します。`config/pipeline.yml`を確認してください。
```
version: 1.0                        # パイプラインバージョン
tasks:                              # パイプラインで使用するタスクを定義
  - task_name: sensor-task
    timeout: 30
  - task_name: podder-task
    timeout: 120
  - task_name: writer-task
    timeout: 30
dags:                               # パイプラインの設定を定義
  - dag_name: pipeline              # パイプライン名称
    schedule_interval: "* * * * *"  # 実行スケジュール
    flows:                          # タスクの実行順序
      - - sensor-task
        - podder-task
        - writer-task
```
今回は新しいタスク追加や実行順序の変更が必要ないので、そのまま設定ファイルを利用します。`pipeline.yml`に定義された設計をもとにパイプラインを構築するには、次のコマンドをプロジェクトディレクトリ以下で実行してください。
```
$ podder pipeline build
```

これで`pipeline.yml`の設計に基づいてパイプラインが構築されました。


## アプリケーションの実行
それでは、構築したパイプラインを使ってアプリケーションを実行してみましょう。次のコマンドを入力してアプリケーションを立ち上げてください。
```
$ podder serve
```
> 最初の実行時はビルド処理が走るため少し時間がかかります。

アプリケーションが立ち上がったら [http://localhost:8080](http://localhost:8080) にアクセスしてください。ダッシュボードが確認でき、ジョブが実行されていることが確認できると思います。

![Dashboard](images/dashboard_job.png)

ジョブの完了を待ってから`output`フォルダを確認すると、実行結果が出力されていることを確認できます。
> インプットフォルダ配下にあるデータが順次読まれて処理されています。

これでチュートリアルは終了です。Podder.ai を使用したパイプラインの設計方法が理解できたと思います。次はこのアプリケーションを実際に皆さんが作った環境にデプロイしていきましょう。
